easyeda2kicad --full --lcsc_id=Cxxxxxxxx --output  C:\Users\aiden\Documents\GitHub\kicad-parts\easyeda2kicad

# Components:

### MCU: STM32F446VET6

- JLC: https://jlcpcb.com/partdetail/STMicroelectronics-STM32F446VET6/C91929 

- Datasheet: https://www.st.com/content/ccc/resource/technical/document/datasheet/65/cb/75/50/53/d6/48/24/DM00141306.pdf/files/DM00141306.pdf/jcr:content/translations/en.DM00141306.pdf 

### Audio codec: TAC5242

- JLC: https://jlcpcb.com/partdetail/TexasInstruments-TAC5242IRGER/C47122155

- Datasheet: https://www.ti.com/lit/ds/symlink/tac5242.pdf

### Audio buffer/filter: TLV9152

- JLC: https://jlcpcb.com/partdetail/TexasInstruments-TLV9152IDR/C882649

- Datasheet: https://www.ti.com/lit/ds/symlink/tlv9152.pdf

### Crystal oscillator + capacitor

**Crystal:** CF4008M00012001

- JLC: https://jlcpcb.com/partdetail/huaxindianzi-CF4008M00012001/C7294546

- 8 MHz frequency

- 12pF load capacitance

- 40 ohm ESR

**Capacitors:** CC0603FRNPO9BN120

- JLC: https://jlcpcb.com/partdetail/YAGEO-CC0603FRNPO9BN120/C519482 

- 12pF capacitance

- NP0 rating, plus/minus 1%

- 0603 footprint

Total Gmcrit: ~0.11 mA/V

### 

# Research:

**Audio buffer:**

- OPA1652
  
  - JLC: https://jlcpcb.com/partdetail/TexasInstruments-OPA1652AIDR/C30025
  
  - Datasheet: https://www.ti.com/lit/ds/symlink/opa1652.pdf
  
  - NOT rail-to-rail
  
  - NOT easy to use with single-supply
  
  - Excellent performance etc, but hard to achieve anyway?

- LMC6482
  
  - JLC: https://jlcpcb.com/partdetail/TexasInstruments-LMC6482AIMXNOPB/C12620
  
  - Datasheet: https://www.ti.com/lit/ds/symlink/lmc6482.pdf?
  
  - IS rail-to-rail
  
  - IS easy to use with single supply
  
  - Mediocre performance, older design

- TLV9152
  
  - JLC: https://jlcpcb.com/partdetail/TexasInstruments-TLV9152IDR/C882649
  
  - Datasheet: https://www.ti.com/lit/ds/symlink/tlv9152.pdf
  
  - IS rail-to-rail (etc)
  
  - Newer design, good performance
