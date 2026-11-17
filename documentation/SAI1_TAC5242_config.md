# STM32F446VETx SAI1 ↔ TAC5242 — Configuration Reference

STM32 is the audio **master** (generates BCLK and FSYNC); the TAC5242 runs in **target I2S mode**, 32-bit slots, 48 kHz, stereo, full duplex. Sources: STM32F446 datasheet (DS10693 Rev 11), RM0390 Rev 9, TAC5242 datasheet (SLASF27A). Items marked **(hand-computed)** or **(unverified)** should be checked in CubeMX / the manuals before relying on them.

---

## 1. Why SAI1 (not I2S)

The F446 I2S peripherals are simplex only (no `I2Sxext`). SAI1 has two sub-blocks, so block A transmits and block B receives, synchronised to A. This gives true full duplex with one BCLK/FSYNC pair.

---

## 2. Pin connections

All pins use **AF6** (datasheet Table 11).

| TAC5242 pin | STM32 pin (LQFP100) | SAI1 function | Direction |
|---|---|---|---|
| BCLK (2) | PE5 (pin 4) | `SAI1_SCK_A` | STM32 → TAC |
| FSYNC (3) | PE4 (pin 3) | `SAI1_FS_A` | STM32 → TAC |
| DIN (5) | PE6 (pin 5) | `SAI1_SD_A` | STM32 → TAC |
| DOUT (4) | PE3 (pin 2) | `SAI1_SD_B` | TAC → STM32 |
| — | PE2 (pin 1) | `SAI1_MCLK_A` | **not used** (TAC5242 has no MCLK pin) |

Notes:
- Block B is synchronous, so `SCK_B`, `FS_B`, `MCLK_B` are released and **not wired**.
- GPIO: mode alternate function, speed **Very High**, push-pull. Series resistors 22–47 Ω near the STM32 on BCLK, FSYNC, DIN. Optional ~100 kΩ pull-down on DOUT/PE3 (TAC DOUT is Hi-Z on unused BCLK cycles and before clocks start).
- Do **not** use PA9 (OTG_FS VBUS; also `SAI1_SD_B`/`I2S2_CK`), PA10 (OTG_FS ID), PA11/PA12 (OTG_FS DM/DP). No SAI1 Port-E pin conflicts with USB.

---

## 3. Clock configuration

HSE = 8 MHz.

### 3.1 PLLI2S → SAI1 kernel clock

$$f_{VCO}=\frac{8\ \text{MHz}}{\text{PLLI2SM}}\times\text{PLLI2SN}=\frac{8}{5}\times192=307.2\ \text{MHz}$$

$$f_{sai\_ker\_ck}=\frac{f_{VCO}/\text{PLLI2SQ}}{\text{PLLI2SDIVQ}}=\frac{307.2/5}{5}=12.288\ \text{MHz}$$

| Register / field | Value |
|---|---|
| `RCC_PLLI2SCFGR.PLLI2SM` | 5 (VCO input 1.6 MHz; allowed 1–2 MHz) |
| `RCC_PLLI2SCFGR.PLLI2SN` | 192 (VCO 307.2 MHz; allowed 100–432 MHz) |
| `RCC_PLLI2SCFGR.PLLI2SQ` | 5 |
| `RCC_PLLI2SCFGR.PLLI2SR` / `PLLI2SP` | unused by SAI1; keep valid (R ≥ 2, P any) |
| `RCC_DCKCFGR.SAI1SRC` | `01` = f(PLLI2S_Q) / PLLI2SDIVQ |
| `RCC_DCKCFGR.PLLI2SDIVQ` | `00100` (÷5) |

Hand-computed register words **(hand-computed, verify)**:
- `RCC_PLLI2SCFGR` = `0x25013005` (M=5, N=192, P=÷4, Q=5, R=2)
- `RCC_DCKCFGR` |= `0x00100004` (SAI1SRC=01, PLLI2SDIVQ=4)

Rules (RM0390 §6.3.23, §6.3.25): write PLLI2SCFGR / DCKCFGR fields only while PLLI2S (and PLLSAI for SAI1SRC) is **disabled**. A 2 MHz VCO input would reduce jitter, but cannot give an exact 48 kHz from 8 MHz in the available Q/DIVQ range.

### 3.2 SAI clock generator

With `NODIV = 0`, `MCKDIV = 0`: $MCLK = f_{sai\_ker\_ck} = 12.288$ MHz (ratio fixed at $MCLK/f_s = 256$).

$$f_{SCK}=MCLK\cdot\frac{FRL+1}{256}=12.288\ \text{MHz}\times\frac{64}{256}=3.072\ \text{MHz}$$

$$f_s=\frac{f_{SCK}}{64}=48\,000\ \text{Hz}$$

With `NODIV = 0` the frame length $FRL+1$ must be a power of two in master mode (64 satisfies this).

### 3.3 Main PLL / USB (reference, **unverified** against RM limits)

$M=4,\ N=168,\ P=2,\ Q=7$ → VCO 336 MHz, SYSCLK 168 MHz, USB 48 MHz. APB1 ÷4 (42 MHz), APB2 ÷2 (84 MHz).
SAI1 is on APB2; RM requires $f_{PCLK} > 2\times f_{SCK}$: $84\ \text{MHz} > 6.144\ \text{MHz}$ ✓.

---

## 4. SAI1 block configuration

`SAI1_Block_A` = transmitter, **master**, asynchronous. `SAI1_Block_B` = receiver, **slave**, synchronous to A (RM0390 §28.3.4: a synchronous block must be in slave mode).

### 4.1 `SAI_xCR1`

| Field | Block A | Block B | Note |
|---|---|---|---|
| `MODE[1:0]` | `00` master TX | `11` slave RX | |
| `PRTCFG[1:0]` | `00` free protocol | `00` | I2S emulated via FRCR/SLOTR |
| `DS[2:0]` | `111` (32-bit) | `111` | |
| `LSBFIRST` | 0 | 0 | MSB first |
| **`CKSTR`** | **1** | **1** | **Required.** `1` = outputs change on SCK falling edge, inputs sampled on rising edge. TAC5242 transmits on falling, receives on rising. Default `0` is wrong. |
| `SYNCEN[1:0]` | `00` async | `01` sync with A | |
| `MONO` | 0 | 0 | stereo |
| `NODIV` | 0 | — | |
| `MCKDIV[3:0]` | `0000` | — | slave: clock generator off |
| `OUTDRIV` | optional | optional | set after config, before SAIEN |
| `DMAEN` | 1 (after MODE set) | 1 (after MODE set) | set `MODE` before `DMAEN` |
| `SAIEN` | enable last (see §6) | enable first | |

Hand-computed values before enabling **(hand-computed, verify)**:
- `SAI1_ACR1` = `0x000002E0`
- `SAI1_BCR1` = `0x000006E3`
- (`DMAEN` = bit 17 = `0x00020000`, `SAIEN` = bit 16 = `0x00010000` added later)

### 4.2 `SAI_xFRCR` (I2S frame shape)

| Field | Value | Meaning |
|---|---|---|
| `FRL[7:0]` | 63 | 64 SCK cycles per frame |
| `FSALL[6:0]` | 31 | FS active level 32 SCK cycles (half frame) |
| `FSDEF` | 1 | FS = start of frame + channel side identification (I2S) |
| `FSPOL` | 0 | FS active low (left channel while FS low, as TAC5242) |
| `FSOFF` | 1 | FS asserted one bit before first bit of slot 0 (I2S) |

`SAI1_AFRCR` = `SAI1_BFRCR` = `0x00051F3F` **(hand-computed, verify)**.

### 4.3 `SAI_xSLOTR`

| Field | Value | Meaning |
|---|---|---|
| `NBSLOT[3:0]` | 1 | 2 slots (must be even when FSDEF = 1) |
| `SLOTEN[15:0]` | `0x0003` | slots 0 and 1 active |
| `SLOTSZ[1:0]` | `10` | 32-bit slots |
| `FBOFF[4:0]` | 0 | data starts at slot MSB |

`SAI1_ASLOTR` = `SAI1_BSLOTR` = `0x00030180` **(hand-computed, verify)**.
Constraints: `FBOFF ≤ SLOTSZ − DS`, `DS ≤ SLOTSZ`, `NBSLOT × SLOTSZ ≤ FRL`.

### 4.4 `SAI_xCR2`

`TRIS = 0`, `MUTE = 0`, no companding. Set `FTH` to match the DMA burst/FIFO strategy (choose in CubeMX; default is acceptable for a first bring-up).

### 4.5 `SAI_GCR`

`SYNCIN`/`SYNCOUT` = 0 (no inter-SAI sync; only internal A→B sync is used).

---

## 5. DMA (RM0390 Table 29)

| Direction | Block | Options (DMA2) |
|---|---|---|
| TX (to codec) | SAI1_A | Stream 1 Channel 0, **or** Stream 3 Channel 0 |
| RX (from codec) | SAI1_B | Stream 5 Channel 0, **or** Stream 4 Channel 1 |

Use **circular** mode, **32-bit** peripheral and memory data width, memory increment on. TX and RX use different streams, so there is no conflict. Buffer fill level of the TX ring is the natural input to the asynchronous USB feedback servo.

---

## 6. Initialisation and enable order

1. Enable HSE; configure main PLL (USB/SYSCLK) and PLLI2S per §3; wait for PLLI2S lock. Set `SAI1SRC`/`PLLI2SDIVQ` while PLLI2S disabled.
2. Enable GPIOE and SAI1 clocks; configure PE3–PE6 as AF6, Very High speed.
3. Wait ≥ 2 ms after TAC5242 supplies are stable; MD0–MD5 straps must already be settled.
4. Configure blocks A and B while disabled: `CR1`, `FRCR`, `SLOTR`, `CR2`.
5. Configure DMA streams; set `MODE` before `DMAEN`; prime the TX buffer with data (master TX only starts FS when the FIFO has data; otherwise it sends zeros with an underrun flag).
6. **Enable block B first** (`SAIEN`), then **block A** (RM: enable the slave before the master; master needs its kernel clock present before `SAIEN`).
7. To stop: clear `SAIEN` and poll until it reads back 0. Before changing TAC5242 MD1–MD5, stop clocks and wait ≥ 100 ms.

Monitor `SAI_xSR` for `OVRUDR`, `WCKCFG`, `AFSDET`/`LFSDET`, `CNRDY`; handle underrun/overrun in the USB feedback logic.

---

## 7. TAC5242 hardware strapping (target I2S, 32-bit, 48 kHz)

| Pin | Setting | Notes |
|---|---|---|
| MD0 | **GND** (no resistor) | Target I2S. Change requires power cycle |
| MD1, MD2 | **Low, Low** | AVDD 3.3 V, 32-bit word, linear-phase filter (MD2 H / MD1 L = 24-bit; H/H = low-latency) |
| MD3 | Low or High | I2S target: Low = 1 Hz HPF, 50 ms input-cap quick charge; High = 12 Hz HPF, 12.5 ms quick charge |
| MD5, MD4 | `00` | Differential AC-coupled input, differential line-out (best performance) |
| | `10` | Single-ended input on INxP (AC), single-ended line-out |
| | `01` | Differential, AC/DC coupled, high common-mode tolerance; receiver/headphone or line-out |
| | `11` | Single-ended AC/DC high CM tolerance; pseudo-differential headphone only |

- MD1–MD5: GND = low, IOVDD (3.3 V) = high. They must be stable before BCLK/FSYNC start.
- Mic vs line is **not** an MD setting: no gain control in pin mode. Full scale is fixed at 2 V<sub>RMS</sub> differential / 1 V<sub>RMS</sub> single-ended. Microphones need an external bias network from `MICBIAS` (2.75 V at 3.3 V AVDD, 5 mA max, 1 µF cap) and likely gain.
- `GPO` is a latched clock-error interrupt output; cleared only by power-cycling the supplies.
- Supported ratio: BCLK/FSYNC = 64 at 48 kHz (3.072 MHz, TAC5242 Table 7-5).

---

## 8. Timing sanity check

Half-period at 3.072 MHz ≈ 163 ns.

| Path | Spec | Margin |
|---|---|---|
| SAI master TX data valid (DS Table 67) | ≤ 15 ns | TAC5242 DIN setup 8 ns ✓ |
| SAI master FS valid | ≤ 14 ns | TAC5242 FSYNC setup 8 ns ✓ |
| TAC5242 DOUT valid after BCLK falling | ≤ 19 ns | SAI RX setup 1 ns, hold 5 ns ✓ |
| I2S/SAI max SCK | SAI ≤ 128·f<sub>s</sub> (32-bit) | 64·f<sub>s</sub> ✓ |

---

## 9. CubeMX / HAL notes (unverified)

- Select SAI1 block A "Master with Master Clock Out disabled", block B "Synchronous Slave"; protocol I2S, 32-bit data, 2 slots, full duplex.
- HAL names the strobe edge; the CubeMX value corresponding to `CKSTR = 1` is likely "Rising Edge". **After init, read back `SAI1_ACR1.CKSTR` and `SAI1_BCR1.CKSTR`** to be sure both are 1.
- Confirm in the generated code that block B has `MODE = 11` (slave RX) and `SYNCEN = 01`.

---

## 10. Bring-up checklist

- [ ] BCLK (PE5) measures 3.072 MHz, FSYNC (PE4) 48 kHz, 50 % duty.
- [ ] FSYNC falls one BCLK before the MSB of the left slot (I2S alignment).
- [ ] TAC5242 DOUT toggles on BCLK falling edges when an analog signal is applied.
- [ ] No underrun/overrun flags in steady state.
- [ ] GPO stays low after clocks start.
- [ ] USB audio feedback endpoint keeps the TX ring buffer near half full.
