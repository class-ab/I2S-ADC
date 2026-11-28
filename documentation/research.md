# STM32 reference designs:

- https://andybrown.me.uk/2015/10/31/stm32f042dev/

- https://andybrown.me.uk/2021/03/13/usb-microphone/

- https://github.com/WeActStudio/WeActStudio.STM32F4_64Pin_CoreBoard

- https://github.com/bartinstruments/open-source/blob/main/AUDIO%20CORE/STM32H723ZGT6.pdf

# Audio design references:

- https://daisy.nyc3.cdn.digitaloceanspaces.com/products/seed3/Daisy_Seed3_datasheet.pdf

- https://github.com/bastl-instruments/citadel/blob/main/schematics/Citadel_1.0_schematic.pdf

- https://github.com/bastl-instruments/kastle2/blob/main/schematics/Kastle_2_1.2_schematic.pdf

- https://github.com/bartinstruments/open-source

- https://github.com/bartinstruments/open-source/blob/main/BOUNCE%20MULTIEFFECT/(300)%20MULTIEFFECT%20STM32H7.pdf

# USB noise filter references:

- https://andybrown.me.uk/2015/07/24/usb-filtering/

- https://www.ftdichip.com/Documents/AppNotes/AN_146_USB_Hardware_Design_Guidelines_for_FTDI_ICs.pdf

- https://andybrown.me.uk/wp-content/images/stm32f042dev/schematic.pdf

- https://andybrown.me.uk/wp-content/images//usb-microphone/schematic.pdf

- https://electronics.stackexchange.com/questions/648984/usb-2-0-vbus-filter

- https://github.com/wagiminator/Power-Boards/blob/master/USB_Power_Filter/USB_Power_Filter_schematic.pdf

- https://www.we-online.com/components/media/o150133v410%20ANP007d%20EN.pdf

- https://daisy.nyc3.cdn.digitaloceanspaces.com/products/seed3/Daisy_Seed3_datasheet.pdf

# Audio buffer ICs:

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

# 3.3V LDO ICs:

AMS1117, AP2210, AP2112, ME6231, LM3940, MCP1827, ADM7154, LDL1117

Selected LDL1117 (S33R version = 3.3V output) for:

- Datasheet recommends good ESR ceramic capacitors, compared to other devices asking for specific and expensive tantalum caps etc

- Ultra low noise operation, power supply (input) noise rejection ratio of 87dB

- Cheap and easy to use part
