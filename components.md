easyeda2kicad --full --lcsc_id=Cxxxxxxxx --output  C:\Users\aiden\Documents\GitHub\kicad-parts\easyeda2kicad

# MCU

**U1: STM32F446VET6**

- JLC: https://jlcpcb.com/partdetail/STMicroelectronics-STM32F446VET6/C91929 

- Datasheet: https://www.st.com/content/ccc/resource/technical/document/datasheet/65/cb/75/50/53/d6/48/24/DM00141306.pdf/files/DM00141306.pdf/jcr:content/translations/en.DM00141306.pdf 

### Power:

**C3-11:** https://jlcpcb.com/partdetail/YAGEO-CC0603KRX7R9BB104/C14663 (100nF)

- decoupling caps for 6x VDD (+VBAT, VDDA, VREF+)

**C12, C13:** https://jlcpcb.com/partdetail/16531-CL10A105KB8NNNC/C15849 (1uF)

- decoupling caps for VDDA and VREF+

**C14, C15:** https://jlcpcb.com/partdetail/24366-CL10A225KO8NNNC/C23630 (2.2uF)

- caps for STM32 internal regulator VCAP_1 and VCAP_2 

**C16:** https://jlcpcb.com/partdetail/20375-CL10A475KO8NNNC/C19666 (4.7uF)

- bulk decoupling cap for VDD

### Crystal oscillator + capacitors

**Y1:** CF4008M00012001

- JLC: https://jlcpcb.com/partdetail/huaxindianzi-CF4008M00012001/C7294546

- 8 MHz frequency

- 12pF load capacitance

- 40 ohm ESR

**C1, C2:** CC0603FRNPO9BN120

- JLC: https://jlcpcb.com/partdetail/YAGEO-CC0603FRNPO9BN120/C519482

- 12pF capacitance

- NP0 rating, plus/minus 1%

- 0603 footprint

# Audio codec

**U2: TAC5242**

- JLC: https://jlcpcb.com/partdetail/TexasInstruments-TAC5242IRGER/C47122155

- Datasheet: https://www.ti.com/lit/ds/symlink/tac5242.pdf

# Audio buffer/filter

**U3: TLV9152**

- JLC: https://jlcpcb.com/partdetail/TexasInstruments-TLV9152IDR/C882649

- Datasheet: https://www.ti.com/lit/ds/symlink/tlv9152.pdf

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
