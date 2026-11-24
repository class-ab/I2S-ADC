easyeda2kicad --full --lcsc_id=Cx --output  C:\Users\aiden\Documents\GitHub\kicad-parts\easyeda2kicad

# MCU

**U1: STM32F446VET6**

- JLC: https://jlcpcb.com/partdetail/STMicroelectronics-STM32F446VET6/C91929 

- Datasheet: https://www.st.com/content/ccc/resource/technical/document/datasheet/65/cb/75/50/53/d6/48/24/DM00141306.pdf/files/DM00141306.pdf/jcr:content/translations/en.DM00141306.pdf 

#### Power:

**C3-11, C19-21, C32, C39, C42, C49:** https://jlcpcb.com/partdetail/YAGEO-CC0603KRX7R9BB104/C14663 (100nF)

- decoupling caps for 9x VDD (+VBAT, VDDA, VREF+), 3x AVDD, DREG, IOVDD, 1x TLV9152

**C12-13, C17, C25-26:** https://jlcpcb.com/partdetail/16531-CL10A105KB8NNNC/C15849 (1uF)

- decoupling caps for STM32, LDL1117S33R and LDL1117S50R  

**C14-15:** https://jlcpcb.com/partdetail/24366-CL10A225KO8NNNC/C23630 (2.2uF)

- caps for STM32 internal regulator VCAP_1 and VCAP_2 

**C16, C18, C27:** https://jlcpcb.com/partdetail/20375-CL10A475KO8NNNC/C19666 (4.7uF)

- bulk decoupling caps for STM32, LDL1117S33R and LDL1117S50R

#### Crystal oscillator + capacitors

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

**U2:** TAC5242

- JLC: https://jlcpcb.com/partdetail/TexasInstruments-TAC5242IRGER/C47122155

- Datasheet: https://www.ti.com/lit/ds/symlink/tac5242.pdf

**C22-24, C30-31, C36-37, C43:** https://jlcpcb.com/partdetail/97651-CL10A106MA8NRNC/C96446 (10uF)

- decoupling caps for TAC5242, TPS63070 and TLV9152

# Audio buffer/filter

**U3:** TLV9152

- JLC: https://jlcpcb.com/partdetail/TexasInstruments-TLV9152IDR/C882649

- Datasheet: https://www.ti.com/lit/ds/symlink/tlv9152.pdf

**U5:** THS4521IDGKR

- JLC: https://jlcpcb.com/partdetail/TexasInstruments-THS4521IDGKR/C170157

- Datasheet: https://www.ti.com/lit/ds/symlink/ths4521.pdf

**R9:** https://jlcpcb.com/partdetail/YAGEO-RT0603BRD071ML/C326730 (1M)

**C38:** https://jlcpcb.com/partdetail/MurataElectronics-GRM31M5C1H473JA01L/C21812 (47nF 1206)

**R6, R10-12:** https://jlcpcb.com/partdetail/YAGEO-RT0603BRD074K99L/C723532 (4.99k)

**C40-41, C44-45:** https://jlcpcb.com/partdetail/KYET-MEB106K2A1501/C49215602 (10uF film)

**U8:** OPA1612AIDR

- JLC: https://jlcpcb.com/partdetail/TexasInstruments-OPA1612AIDR/C94590

- Datasheet: https://www.ti.com/lit/ds/symlink/opa1612.pdf

**FB1-2:** https://jlcpcb.com/partdetail/Sunlord-GZ2012D601TF/C1017

**D1-2:** https://jlcpcb.com/partdetail/onsemi-ESD5Z33T1G/C61148
**R16, R17:** https://jlcpcb.com/partdetail/YAGEO-RT0603BRD071KL/C110776 (1k)

**R13, R14:** https://jlcpcb.com/partdetail/3448513-TC0325B2001T5F/C2989090 (2k)

**R15:** https://jlcpcb.com/partdetail/YAGEO-RT0603BRD0764R9L/C861511 (64.9)

**C46-47:** https://jlcpcb.com/partdetail/546277-GCM1885C2A101FA16D/C527967 (100pF)

**C48:** https://jlcpcb.com/partdetail/YAGEO-CC0603FRNPO9BN102/C309468 (1nF)

# Connectors

**J1:** 6.35mm stereo jack right-angled (PJ-609BA)

- JLC: https://jlcpcb.com/partdetail/HOOYA-PJ609BA/C309277

**J2:** USB-C SMD connector (1054500101)

- JLC: https://jlcpcb.com/partdetail/MOLEX-1054500101/C134092

- Datasheet: https://www.molex.com/content/dam/molex/molex-dot-com/products/automated/en-us/salesdrawingpdf/105/105450/1054500101_sd.pdf?inline\

**J3:** https://jlcpcb.com/partdetail/Samtec-FTSH_107_01_L_DVK/C5307809 SWD - STM32

**J4:** https://jlcpcb.com/partdetail/Neutrik-NC3FAAH2/C368522 XLR

# Power

**U4:** LDL1117S33R

- JLC: https://jlcpcb.com/partdetail/STMicroelectronics-LDL1117S33R/C435835

- Datasheet: https://www.farnell.com/datasheets/2259188.pdf

**U6:** LDL1117S0R 

- JLC: https://jlcpcb.com/partdetail/STMicroelectronics-LDL1117S50R/C970558

- Datasheet: https://www.st.com/content/ccc/resource/technical/document/datasheet/group3/0e/5a/00/ca/10/1a/4f/a5/DM00366442/files/DM00366442.pdf/jcr:content/translations/en.DM00366442.pdf

**U7:** TPS63070RNMR

- JLC: https://jlcpcb.com/partdetail/TexasInstruments-TPS63070RNMR/C109322

- Datasheet: https://www.ti.com/lit/ds/symlink/tps63070.pdf

**L1:** XFL4020-152MEC

- JLC: https://jlcpcb.com/partdetail/Coilcraft-XFL4020152MEC/C3033018 

**C29:** https://jlcpcb.com/partdetail/16532-CL21A106KAYNNNE/C15850 (10uF 0805)

**C33-34:** https://jlcpcb.com/partdetail/46786-CL21A226MAQNNNE/C45783 (22uF)

**C35:** https://jlcpcb.com/partdetail/1877-CL05B104KO5NNNC/C1525 (100nF 0402)

- decoupling caps for TPS63070 (x3 above)

**R1:** https://jlcpcb.com/partdetail/YAGEO-RT0603BRD07300KL/C705762 (300k)

**R2:** https://jlcpcb.com/partdetail/YAGEO-RT0603BRD0751KL/C861458 (51k)

**R3, R5:** https://jlcpcb.com/partdetail/YAGEO-RT0603BRD0710KL/C95204 (10k)

**R4, R6-8:** https://jlcpcb.com/partdetail/YAGEO-RT0603BRD07100KL/C122538 (100k)

**R18-19:** https://jlcpcb.com/partdetail/23913-0603WAF5101T5E/C23186 (5.1k)

**U9:** https://jlcpcb.com/partdetail/Stmicroelectronics-USBLC62SC6/C7519

**D3:** https://jlcpcb.com/partdetail/onsemi-ESD5Z50T1G/C82044

**FB3:** https://jlcpcb.com/partdetail/MurataElectronics-BLM18PG121SN1D/C14709

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

**3.3V LDO:**

AMS1117, AP2210, AP2112, ME6231, LM3940, MCP1827, ADM7154, LDL1117

Selected LDL1117 (S33R version = 3.3V output) for:

- Datasheet recommends good ESR ceramic capacitors, compared to other devices asking for specific and expensive tantalum caps etc

- Ultra low noise operation, power supply (input) noise rejection ratio of 87dB

- Cheap and easy to use part
