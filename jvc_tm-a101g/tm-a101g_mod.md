JVC TM-A101G RGB Mod and front button selector
==============================================

## Intro

I was planning to use this monitor in my setup that has my consoles connected to several monitors through an Extron MVX matrix switcher, and all monitors use RGB as input; this small JVC has a great image and I decided to mod it. 
To make it a bit more interesting I wanted to use one of the 2 buttons in the front to commute between composite and RGB: "A" will be composite and "B" will use RGB.

Info on this monitor can be found in the CRTDatabase page: [TM-A101G](https://crtdatabase.com/crts/jvc/jvc-tm-a101g)

Service manual can be found here: [manual](https://archive.org/download/jvc-tm-a-101-g-sm/JVC%20TM-A101G%20SM.pdf) and [schematics](https://dn760001.eu.archive.org/0/items/jvc-tm-a-101-g-sch/JVC%20TM-A101G%20Sch.pdf)

For the RGB modding part credits go to Immerhax that developed the original I2C override logic: [link](https://immerhax.com/?p=305)

## Theory

There are a few steps we need to make:

1. prepare the signal input board
2. adjust the input signal to be at the right level
3. override the I2C info to properly enable RGB
4. use the input selector signal to enable/disable RGB

### Preparing the Signal Input Board

This monitor uses a Toshiba TA1226EN Chroma chip. This chip has 2 sets of analog RGB inputs: the digital is used for the OSD menu, and the analog is grounded through capacitors C716, C717, C718. 
![image](schematics_chroma.png)

For the mod we will use pins 23, 24, 25 for R, G, B signals and pin 22 for blanking.

On the "Signal PWB" board we need to:

- remove C716, C717, C718
- connect the R, G, B input wires to the holes directly connected to the chroma chip
- connect the blanking cable to the leg of pin22 (there is already a 10k Ohm resistor to ground there that we will keep)
- connect the ground wire to the other side of one of C716/C718/C718
- connect the 5V wire to the jumper W726 (that is the output of the IC954 regulator)

The sync signal can be connected directly to thet "VIDEO_A" input pin, and this way you will reuse all the internal sync extraction logic independently of the type of signal you are feeding in (composite, luma, pure csync). The only side effect is that the image will be slightly shifted left when using RGB, that you can correct in the service menu. A good injection point is the negative leg of C101; there are several places near that that can be used to connect the corresponding ground as well. 

The reason of that shift is the delay introduced by the comb filter and the overall signal processing before the sync gets injected into the chroma chip. To avoid the shift we might inject sync downstream of the comb filter, but this monitor does some processing of the signal and reconstructing the right level might take some experimentation; I will probably do that in the future; for now I will adjust the shift, that is minimal anyway.

All the modifications above have to be done on the part of the PCB that is behind a metal shield, that has to be removed by desoldering 4 tabs. 

All the signals needed for RGB can be injected from the front of the signal board; I chose to do this so I can more cleanly use the back for the I2C spoofing we will do later.

The following picture shows the board before/after:

![image](signal_board_front.jpeg)
![image](signal_board_after.jpeg)

### Circuit to inject RGB signal

The chroma chip does not use the standard 0.7 Vpp voltage, but instead expects 0.5 Vpp, as can be seen in the datasheet:
![image](specs_chroma_rgb.jpeg)

So we need to adjust the input voltage, and at the same time keep the input termination as close as possible to the standard 75 Ohm. The following circuit shows the voltage divider circuit I used: 

![image](schematics_input.jpeg)

As you can see the termination is 78 Ohm (56 + 22), and the voltage divider will bring down the input signal to 56/(56+22)*0.7 = 0.5 V which is exactly what is expected by the Chroma.

I created the circuit above using my rgb-mux board:
![image](rgb_mux_board.jpeg)

I use this board with SCART mods, but the signals are exactly the same so it was very convenient. 

On the right there are 2 voltage dividers (all four resistors are 10k Ohm) used for blanking: they will bring down the 5V voltage form the board or from the VGA (pin 9) to ~2.5V that is enough to trigger blanking in the Chroma.
I connected those on a DPST switch with 3 positions:

- **On**: RGB input is forced On
- **On**: RGB input is disabled
- **Auto**: RGB input is triggered when the device connected to VGA is on

The two sources used for 5V are the jumper W034 (5V rail) and pin 9 of the VGA connector.

### Using the front buttons to enable/disable RGB

With the changes made so far the monitor is fully working and can be fed with a RGB signal: the switch will force RGB on/off independently from the channel that is used, but sync will only work when tuned to inputA.
Would be great to have RGB automatically selected when we use inputA and disabled when we use inputB. This can be done quite easily by using a transistor as a switch, and adding that to our mux board.

Conveniently the monitor has a signal that shows which input is selected: the collector of Q220 will go high (~3V) when inputA is selected and low (GND) when inputB is selected. This is ideal to be fed to the base of a 2N2222 NPN transistor, to switch it on and off.

So in our case the transistor will have:

- base: connected to the collector of Q220 through a 10k Ohm resistor
- collector: connected to the 2.5V rail in the board
- emitter: connected to the blanking pin in the board

We don't need any other pull up/down resistors because the signal to the base is never floating and the collector is connected to a high impedence input in the chroma chip.

The final board is like this:

![image](rgb_mux_board.jpeg)

Now with the external physical switch we can turn the mod on/off and when the mod is on it's only active when inputA is selected. Exactly what we wanted.

### Input connector on the back

The input connector can be whatever we like, for example SCART or RCA; I chose to use a VGA connector to make it easier to connect directly to my Extron MVX.

When using VGA, one thing to keep in mind is the level of the sync signal: VGA uses TTL level sync that is usually between 3.3 and 5 Vpp; the NTSC standard for composite is 1 Vpp, and sync is the negative part of the signal, approximately 0.3 Vpp. So we will need to attenuate sync to bring it within spec and that can be done with a 470 Ohm resistor inline to the sync line before it's injected in the monitor. The picture above shows that resistor as well.

I 3D printed a small adapter to host the VGA and the switch, and make it fit nicely on the back of the monitor:

![image](rgb_mux_board.jpeg)
![image](rgb_mux_board.jpeg)



![image](front_sfa3.jpeg)

And with this we are done! 

## Pictures

Let's conclude with a few pictures showing the TV and the image quality:

![image](240p_zoom.jpeg)
![image](sfa3_1.jpeg)
![image](sfa3_2.jpeg)
![image](sfa3_zoom2.jpeg)
![image](front_240p.jpeg)
![image](front_grid.jpeg)
![image](back_inside.jpeg)
![image](back_inside_top.jpeg)
![image](rear2.jpeg)
