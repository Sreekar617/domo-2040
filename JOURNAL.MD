Using [Kai Pereira's devboard tutorial](https://blueprint.hackclub.com/starter-projects/devboard), I was able to get a schematic together relatively quickly. The decoupling capacitors were kinda confusing at first, but then I checked the datasheet and it made sense that since there were 8 3v3 outputs you'd use 8 capacitors for 3v3
![](https://cdn.hackclub.com/019ed780-8f91-724d-be31-46112cdec84a/image.png)

## USB
Starting at the top left, USB is the USB-C connection because wireless power isn't good enough yet and micro usb is trash. The CC lines are pulled to GND via 5.1k resistors as specified by the datasheet.  Data is transmitted via D- and D+, and there's two identical outputs for each line. I just have them connected to a single line and connected to the MCU via 27 ohm resistors. The power given by the USB C receptacle is 5V, so i use an LDO to step it down to 3V3 which is much tastier and can efffectively juice up not just the components on the pcb but also any external components i may want to connect to the devboard. both the 5V and 3.3V lines have bulk caps to smooth out large voltage ripples caused by the LDO not being a massless frictionless ideal gas. 
![](https://cdn.hackclub.com/019f1111-25ae-7d10-80a4-8326d9c93bc9/image.png)
## DECOUPLING
This is pretty simple, there's eight small 3V3 capacitors and two 1V1 capacitors to filter out high frequency noise, one per output on the chip, and then a larger decoupling capacitor for each of the lines.
![](https://cdn.hackclub.com/019f1112-0d9a-73a1-ab25-8946107415c6/image.png)

## CRYSTAL
This is a super tuff crystal that oscillates at a known frequency to help the MCU time stuff. It needs two 33pF (tiny!) external load capacitors to work for some super cool math reasons i don't really understand. It's connected to the MCU through a 1k damping resistor that protects it.
![](https://cdn.hackclub.com/019f1112-9a29-7f77-a18c-52ed279e9eae/image.png)

## FLASH
I deviated from the tutorial a bit here and used a 16MB flash chip instead of the 2MB that the tutorial used, so hopefully that'll be fine. It communicates with the MCU using quad SPI for fast data transfer. Side note, the widely used terms to talk about SPI are master and slave, but the other (and imo better) way to say it is controller and peripheral. However, it's labeled both ways on my schematic so bear with me a little. When the Slave Select (or Chip Select) pin on the flash is pulled to ground, it acts as a reset signal, so that's what the 1k, SW, and GND are for. The rest of the time, it has to be pulled to HIGH, so we have a 10k resistor connected to 3V3 to pull up the CS pin the rest of the time. There's a 0.1uF decoupling capacitor next to the power, and there's four data lines going back to the MCU (hence quad spi)
![](https://cdn.hackclub.com/019f1137-5443-7d62-b098-9f877f06ae4d/image.png)

## RP2040/OUTPUT
There's nothing super crazy going on here, from my side I just connect all the peripherals back to this, and also make sure to connect the GPIO outputs and whatnot to the header pins.
![](https://cdn.hackclub.com/019f1138-92c6-79fe-9154-fb81f88c7263/image.png)

# PCB -- Edge.Cuts, Placement, and Caps (~3 hours)
The first thing I did on the PCB was make the Edge.Cuts, since I want this to be able to match the footprint of a normal Pico I matched the dimensions. I then placed down all the big components, and placed the decoupling capacitors as close as I could to the things they were supposed to decouple. Following the advice of the guide, I tried to wire the high speed components first, without using any vias. Using the 16MB chip made it kinda hard since it's just so bulky, but eventually I got it wired pretty good. The USB-C data lines needed to be differential pairs, and they needed to be exactly the same length, but I tried for a while and I could not figure out how to use the length tuning tool, so I ended up just adding a little kink by hand.i was confused about some simple things (like "will the usb c port fit underneath the board") so i tried asking the big claude and the big gpt but they were completely useless so i had to actually lock in and solve my problems on my own.
![](https://cdn.hackclub.com/019f1024-7cbe-77a9-b25c-4a494c3a1b63/image.png)

# PCB -- Routing  (~3 hours)
This was definitely the hardest part. I just sat in front of the Kicad and tried different variations on routing until I found one that was satisfactory. I had to move some of the capacitors out of the way to make space, and I ended up putting some of them underneath the chip, on the back side. The bulky 16MB chip really overcomplicated this, but at this point I was reluctant to change it out so I tried my best to work around it. Once I had all the resistors and stuff down, I had to route the 40ish header pins, which were less strict about speed and impedance and stuff so I could use vias for those. 
![](https://cdn.hackclub.com/019f1028-1386-7b13-80ba-858c57b73492/image.png)

# PCB -- Debug (~4 hours)
This was a lot of running DRC, fixing the error that came up, then running DRC and fixing the two more that pop up in its place. One thing I encoutered a lot is incomplete thermal relief where there was only one spoke instead of two spokes: ![](https://cdn.hackclub.com/019f0c8f-a7ed-7984-9624-ab36b4b5fd06/image.png)
I fixed this by just extending a small trace outwards from the pad, which is a solution i read somewhere on Slack and seemed to appease DRC. ![](https://cdn.hackclub.com/019f0f70-41c3-7c36-a63b-0f7ccdcccc25/image.png)
Also, there were sections of the ground pour that were kinda just floating and not actually connected to ground, so I added some more vias to fix that. I also cleaned up some of the suboptimal routing, and fixed some of the text being too small to print properly. Also there was some basic stuff like a couple traces/vias accidentally placed too close to each other but that was pretty simple to fix.
# Back to the schematic (~2 hours)
I decided it was too cringe to not deviate from the original Pico design, so I decided to make use of every pin on the rp2040, and also use every single header pin on the 1x20 connectors. My original design had GP25 marked as N/C, and both the original Pico and the Orpheus Pico have two N/C pins on the headers (well technically theyre not NC on the pico but they're basically useless to normal people). After some tweaks to the schematic, I added a user LED so it can look cool and light up, and I added another button for user input, using all the GPIO pins on the RP2040. Then, I shuffled around the header pins a bit and added one GP pin so all the slots are used, but I did my best to make it still compatible with normal Pico uses so it won't explode or something.
![tuff schematic](https://cdn.hackclub.com/019f102a-e839-76a1-9213-b51d48cd5e41/image.png)

# low cortisol decorating

# oops there's a catastrophic error (~2 hours)
it turns out all of the 3v3 traces need to be connected to **each other** and not just to their own capacitor so i had to reposition a bunch of the capacitors to make it possible to connect them together. This was pretty hard because at this stage of the design, a lot of stuff was already placed down, and i kinda had to place capacitors around the stuff.
Here's a picture of the error (notice the thin cyan lines showing it's unconnected):
![](https://cdn.hackclub.com/019f102d-b7f0-76a7-b792-78830b94dac2/image.png)
and here's my solution 😭:
![](https://cdn.hackclub.com/019f102f-87d7-79fc-af27-670d384ae5c2/image.png)

# ship it

bom draft thingie
blue led: C2288
diode thing: C191023
77
f
