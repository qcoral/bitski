# Notes

## Parts list:

- esp32-s3-1-n8 (8mb ram)
- bq25185 charging
- voltage regulator??
- usb c part??

- ???

## Datasheets:

- https://docs.espressif.com/projects/esp-faq/en/latest/software-framework/peripherals/usb.html
- https://documentation.espressif.com/esp32-s3-mini-1_mini-1u_datasheet_en.pdf
- https://www.ti.com/lit/ds/symlink/bq25185.pdf?ts=1772714566377
- https://docs.espressif.com/projects/esp-hardware-design-guidelines/en/latest/esp32s3/schematic-checklist.html waow this is cool
- https://jlcpcb.com/partdetail/BoschSensortec-BMP580/C22391138

## Other notes

Impedance is really cool! Turns out the reason why those 33ohm resistors are there is so that the impedance is matched.

My intuition told me that logically the resistors should be placed close to the chip then. Claude affirmed this - hope I'm right?

Apparently this thing has a touch sensor! wow.

Claude dug the internet and found this. lmao
https://cadlab.io/project/2492/master/files

Chose BMP580 because it's pretty commonly used, well documented, and decently cheap? I think barometric pressure sensors are just expensive

Found this lovely sht40 clone:
https://www.lcsc.com/product-detail/C48887140.html

Seems OK to use
