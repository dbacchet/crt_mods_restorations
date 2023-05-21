KV-20M40 - Dual mod RGB + Component
===================================

![crt_front](crt_front.jpeg)
A friend brought me a very nice Sony Trinitron KV-20M40, that he wanted to mod. This set has a BA4 chassis that is _very_ flexible so we decided to perform a dual mod, and add _both_ RGB and Component support to this nice 20".

To make things a little more interesting, I wanted to be able to select RGB and Component using the remote, and not a physical switch on the back of the TV. Luckily that's possible with this TV. The result is that at the end we have a TV with:

* RGB or Composite on Video1
* Component on Video2

Let's get started!

![chroma](chroma.png)
Looking at the Chroma/Jungle chip in the schematics, there are 2 sets of pins that are relevant:

* pins 32,31,30 will be used to inject the RGB signals, and pin 29 to switch to RGB mode. This chroma has an additional set of RGB inputs, but I decided to perform a RGB-mux mod, mainly for 2 reasons: 1) it is more convenient since R2/G2/B2 are exposed through jumper wires on the top of the chassi board, and 2) to use R1/G1/B1 I needed to either lift the pins or cut some traces and I wanted the mod to be reversible.
* pins 37,38,39 will be used to inject Y, Pr, Pb, and we will also need to use pin 41 for the sync signal and pin 36 to switch to component.

# RGB Mod
The RGB mod is fairly classical: 

1. remove the grounding resistors R087, R088, R089 ![grounding resistors](grounding_resistors.png)
2. create a mux circuit with 75 Ohm termination resistors and 680 Ohm inline resistors for R, G, B, like shown in the picture. ![circuit](20m40-osd-mux-noscart.jpg) I used a custom small board I designed to simplify the work, since it's the same for all rgb-mux mods.
3. connect the R,G,B signals to one leg of the resistors R025, R026, R027 and the blanking signal to one leg of L001. ![chassis mod](mod_top_large_zoom.jpeg) Get the 5V for blanking from one leg of R009, and ground from JW045.

# Component Mod
Adding component on this set is also pretty easy:

1. Terminate the Y, Pr, Pb signals to ground with 75 Ohm resistors and connect them to pins 37, 38, 39 through a 0.1 uF ceramic capacitor
2. Connect the Y signal to pin 41 through a 0.1 uF capacitor, to pass the sync signal

The result looks like the following picture: ![ypbpr_bottom](mod_bottom1_large_zoom.jpeg). There is an unpopulated header on the right that I used to bring the signals on the top of the chassis, for the grounding resistors and for the audio signal from Video2.

One thing worth mentioning here is that in my particular case I decided to _disconnect_ the Video2 RCA (in the front of the tv) to be able to use that channel for component with the remote. In case you want to preserve that functionality (passing the sync on the original RCA connector and use a physical switch for enabling component) _do not_ terminate the Y signal, since it's already terminated in the CVBS2/Y2 line. If you want to understand better why, [this thread](https://shmups.system11.org/viewtopic.php?f=6&t=68134) on shmups is a gold mine.

The final result looks like this:
![final_mod_top](mod_top3_large.jpeg)
Note the pin headers for RGB (on the small PCB) and Component, that will be connected directly to the RCA connectors in the back of the TV:
![mod_rca](mod_rca.jpeg)
![back_rca_labels](back_rca_labels.jpeg)
The switch is used to commute between RGB and composite on Video1.

# Using the remote to switch between TV, RGB and Component

This is the part of the mod that is a little more interesting. Instead of having a switch on the back that forces on RGB and/or Component on every channel, I wanted to be able to use the remote for selecting the input; so it's possible to keep all systems connected (i.e. a PS2 in component and a MiSTer in RGB) and just commute between them with the remote without having to get to the back of the TV every time.

On the BA4 chassis, the Micom has a very nice feature: pins 10 and 11 put out 5V when Video1 and Video2 are selected. So we can use them to enable RGB and Component by just tuning to the right input with the remote, how convenient!
![micom_chsel](micom_chsel.png)
Probing the values it turs out that they produce 6.3V, and that is good because there is some margin to still have ~5V on those lines after we connect them to the RGB and Component switch pins in the chroma, since that will drain more current and slightly drop the voltage.

So what we need to do is:

1. connect pin10 to pin 29 (YS2) in the chroma chip, through a 2.1K Ohm resistor. This will make the produced voltage drop from 6.3V to 5.1V, that is still within specs. I also used a switch to still be able to use composite, in this particular mod.
2. connect pin11 to pin 36 (YUVSW) in the chroma, through a 2.1K Ohm resistor.

So now we can use the remote to switch!
![video1_rgb](video1_rgb_small.png)
![video2_component](video2_component_small.png)

Quality is very good on both RGB and component, pretty happy on how it turned out!!
![crt_grid](crt_grid.jpeg)
![crt_img_ff](crt_img_ff.png)
![crt_img_hp](crt_img_hp.png)
