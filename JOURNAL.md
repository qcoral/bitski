# Journal!

Migrating this over from Blueprint and also discontinuing the whole NOTES.md file thing. I really like storytelling instead.

## March 4th, 2026

introducing bitski!!

![bitski](https://blueprint.hackclub.com/user-attachments/blobs/proxy/eyJfcmFpbHMiOnsiZGF0YSI6MTE2MTU2LCJwdXIiOiJibG9iX2lkIn19--1ba8fe03591f4fada5bc2feec5595a4bccbf7262/bitski.png)

concept above ^

anyhow. I'm really rusty at making projects so I figured this would be a kick in the ass to actually design something real. Started designing the project today

Here's what I have so far:

![image](https://blueprint.hackclub.com/user-attachments/blobs/proxy/eyJfcmFpbHMiOnsiZGF0YSI6MTE2MTU4LCJwdXIiOiJibG9iX2lkIn19--1e7a7de594678901722256c6bd6914342e39aa3e/image.png)

Highlights:

- Made my own footprint for the BQ25185 b/c kicad doesn't have it
    - I settled on this thing in the first place because Adafruit has a charger module for it already. I trust their judgement that it's a good IC to use.
- Settled on the esp32-s3-mini-1-n8 because it has bluetooth + native USB support
- decided on 0402 LEDs because apparently [this](https://hackaday.io/project/19292-redotsmart) guy already tried the whole 0201 LED setup and it did NOT work well.

Trying to figure out what the best power setup for this thing is - I think I might want to fix the CC pins to draw more than 500mA but we'll see

anyways, signing off!

## March 15, 2026

![image](https://blueprint.hackclub.com/user-attachments/blobs/proxy/eyJfcmFpbHMiOnsiZGF0YSI6MTIyMTM0LCJwdXIiOiJibG9iX2lkIn19--07a529d2ad8bc11ccdf28ea869390c6614cbfe25/image.png)

Ok wow it's been a few days!

I copied some stuff over from the orpheus pico's power circuitry, but obviously I wanted to make sure I properly understood it first.

Main thing was figuring out impedance matching and what resistors to use on the USB circuitry. Espressif's [design guide](https://docs.espressif.com/projects/esp-hardware-design-guidelines/en/latest/esp32s3/schematic-checklist.html#usb) recommended 22/33 ohms, but the datasheet had 0 as a placeholder. I decided to be safe and go with the design guide as I'm presuming the datasheet is a bare minimum implementation.

Interestingly it's recommended to add some unpopulated capacitors just in case you need to add some later to filter out noise. Did so accordingly

![image](https://blueprint.hackclub.com/user-attachments/blobs/proxy/eyJfcmFpbHMiOnsiZGF0YSI6MTIyMTM1LCJwdXIiOiJibG9iX2lkIn19--c979a0c0510f29a16fd7fb41795fcf992c9fddf3/image.png)

Claude was surprisingly good at teaching me this. I think I have a decent grip on impedance now and how that whole thing works?

Also read a bit on different types of diodes and why I might want to use a schottky instead of a regular diode. That one was relatively straightforward, but still pretty fun!

![image](https://blueprint.hackclub.com/user-attachments/blobs/proxy/eyJfcmFpbHMiOnsiZGF0YSI6MTIyMTQxLCJwdXIiOiJibG9iX2lkIn19--de17d4db8457cb5ea35387378fc809655dbb506a/image.png)

I also started to read a bit into charlieplexing the LEDs, but my brain's a little tired right now. Honestly seems really cool though.

Signing off!

## Migration from my NOTES.md file

\*this is info I would rather have in my journal. I think this was done sometime in early march?

Impedance is really cool! Turns out the reason why those 33ohm resistors are there is so that the impedance is matched.

My intuition told me that logically the resistors should be placed close to the chip then. Claude affirmed this - hope I'm right?

Apparently this thing has a touch sensor! wow.

Claude dug the internet and found this. lmao
https://cadlab.io/project/2492/master/files

Chose BMP580 because it's pretty commonly used, well documented, and decently cheap? I think barometric pressure sensors are just expensive

Found this lovely sht40 clone:
https://www.lcsc.com/product-detail/C48887140.html

Seems OK to use

## April 4th, 2026.

Trying to figure out what IMU to use. Turns out these are very hard!

Found this page, which was extremely useful for picking out parts - even though I ended up not using any of it it helps to set a reference point of what the "norm" in a community looks like so to speak.

https://betaflight.com/docs/development/manufacturer/manufacturer-design-guidelines#312-inertial-measurement-unit-imu-selection

I think I'm going to run with the [LSM6DSOXTR](https://jlcpcb.com/partdetail/STMicroelectronics-LSM6DSOXTR/C481766) accelerometer/gyroscope + [LIS3MDLTR](https://jlcpcb.com/partdetail/STMicroelectronics-LIS3MDLTR/C478483) magnometer.

I strongly considered the BNO055, but I think it'll be much more interesting to figure out some of the math myself + this combo is cheaper.

Anyhow, I decided to play a bit more with the layout. Turns out the ISFL3741 is massive! I think I'll have to go double sided PCBA for this which sucks, but I really want to lean into the compact device fantasy and that feels kind of impossible without doing that

Here's where I ended up landing with the layout:

![image](https://user-cdn.hackclub-assets.com/019d5fbc-3233-71bd-b04f-fbbbef446ac2/paste-1775427661527.png)

and here's a paper printout!

![image](https://user-cdn.hackclub-assets.com/019d5fbc-f75a-7e06-9288-20334e1ecd66/paste-1775427712134.png)

## April 28th, 2026

Sorry this ended up taking awhile to get back to! Was really busy travelling. Gave a talk at RMRRF!

Anyhow, back to where we landed. Updated the schematic

Only spent ~30 min on this today? Just did some basic schematic stuff in order to fill more in

Turns out the IS31FL3741, which I was originally planning to use, was not actually in stock! Instead we are going to chain two IS31Fl3731's instead and then bang them together in firmware. Hopefully the lookup table works.

A little worried ab i2c sync, but we'll see how that goes. I really need to stop getting lost in a sea of component choosing and actually stick to something. I think part selection ends here, i've cut down on a bunch of stuff for now.

Anyhow, finished the part selection for the sht40 clone! this one was pretty simple.

also updated my resources list :)

## April 29th, 2026

Not too much done today. Just spent a tiny bit of time combing through datasheets and trying to figure out IOMUX functionality. Found [this table](https://documentation.espressif.com/esp32-s3_datasheet_en.pdf#cd-append-consolid-pin-overview)

![image](https://cdn.hackclub.com/019dd81d-86d5-76ae-8b59-dae42fedaf70/paste-1777447305533.png)

That's a lot!

need to go to sleep. insane things coming up.

## July 24th, 2026

HOLY so much has happened. I really need to actually finish this thing!

## July 27th, 2026

Finished the BMP580 schematic part!
![image](https://cdn.hackclub.com/019fa66e-14d5-7f38-bb8d-25e644711473/paste-1785203659284.png)

I'm basing it off of the Adafruit BMP580 schematic implementation [here](https://learn.adafruit.com/assets/139375). Seems relatively straightforward? I might have to mess around with the i2c config but it should be fine
w

## July 28th, 2026

Ok so turns out I did not finish the battery wiring I think (?) so I am now looking at that

ALSO! The datasheet page to reference is page 41 for the esp32-s3-mini-1u:

![image](https://cdn.hackclub.com/019faaf9-d256-7694-a45d-a3d47434bdcc/paste-1785279926553.png)

I should check this thing often ^^

## August 2nd, 2026

Finishing the battery stuff today! Nifty table:

![image](https://cdn.hackclub.com/019fc41d-a0a7-76d0-a4cd-235633e38d49/paste-1785701702610.png)

Overall, I think I spent roughly 2 hours of solid work time? I worked from 4-9 but a lot of it was staggered.

Learned SO SO much:

- PD triggers are mainly for higher voltages! If a device is capable of 5v3A it'll just dispense that anyways if the slave device doesn't have any PD negotiation circuitry
- Bosch uses Verilog for the BMP580 when describing the i2c address config options! i.e 7'h47 = 7-bit hex, 0x47. This was really interesting to know.

Also putting solder jumpers everywhere so that I can kind of "assemble" the board piece by piece once i actually get it!

![image](https://cdn.hackclub.com/019fc53f-1243-796e-8105-2eefb8eec807/paste-1785720672706.png)

A lot of today was just checking over to make sure that the components I currently have were actually wired up! I.e the configurations were correct, all the pins that needed to be wired were wired, etc.

There's still some more to go through and notably I still need to set up the config resistors on the bq25185, but generally speaking things should be OK (?)

God this is fun.

## August 15 2026.

Once again it's been a minute. I really need to get better at this.

Well I guess update #1: I found a battery at the office to use for this thing! It's 1000 mAh, should be pretty good

(note to self: take pic of battery)

Time to figure out the power requirements!

One really interesting thing to note is that the IS31FL3731 wires everything internally as 2 9-pin charlieplexed blocks of 72 LEDs, not a block of 18 pins! That's why the max is 144 LEDs instead of n \* (n - 1) = 240 LEDs.

other interesting things I learned about:

- [The green gap!](https://hmntl.illinois.edu/news/63802)
- Turns out white LEDs are just blue LEDs covered in phosphor!

## August 30th, 2026

OK I REALLY NEED TO GET BACK ON THIS WOW

First things first - need to get a 5v boost for the LEDs! I didn't realize white LEDs have such a high voltage drop (almost 3.3v), which causes issues if I wire it up to that directly!

For the math on the pixels, here's what I got:

ILED = 64.7 / REXT

if REXT = 20k ohm as recommended, then ILED (average LED current) = 3.2mA

IOUT = ILED / 10.5

subbing in the terms

IOUT = 34mA peaks

If each column has up to 16 LEDs, that could get us to 544 mA tops per IC if all are lit at 100% brightness. That's a lot!
