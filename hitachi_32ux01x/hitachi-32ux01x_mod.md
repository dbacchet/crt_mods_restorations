RGB-Mod + G1-Mod on HITACHI UltraVision 32UX01S
===============================

![image](comparison_sonic_logo.jpg)
The Hitachi 32UX01S is a fantastic 32"; it has the same tube of several JVC D-series 32", with a beautiful slot mask and very deep blacks.
It already has composite, s-video and component inputs on the back, but the owner wanted to have it RGB-modded with SCART and also add a G1-mod.

![image](hitachi_full_mvc.jpeg)

## RGB Mod

This Hitachi uses a Toshiba TA1310N, that has 2 sets of analog RGB inputs; one is used by OSD, and one by the Picture-in-picture (PIP) function. For this mod we will reuse the PIP, muxing the signal coming from the SCART there.

![image](chroma_schematics.png)

As usual we need to do 3 steps:

1. connect the R,G,B, signals with the right termination and right voltage
2. provide a blanking signal to commute to RGB
3. connect the csync signal

### RGB signals

The TA1310N analog RGB inputs (pin 3, 4, 5) but they expect a signal that is 0.5Vpp, sligthly different than the standard 0.7Vpp that is coming from the video source (console, MiSTer, etc).
![image](TA1310N_rgb_inputs.png)
To convert the 0.7Vpp signal to 0.5Vpp we will use a clever trick with the termination resistors:
- each signal must be terminated to ground with 75 Ohm
- we can create a voltage divider using 56 + 22 Ohm resistors

the schematic will look like the following:
![image](input_voltage_divider.png)
The output signal will be 56/(56+22)*0.7 = 0.5 Vpp which is perfect, and the total termination 78 Ohm instead of 75, which is absolutely acceptable.

Since the TV already has the 0.1 uF decoupling capacitors, the only thing we need to do is to build the circuit above, and connect it to the chassis. There are very convenient injection points in the signal board, shown in the following picture:
![image](chassis_rgb_injection_point.jpeg)

### Blanking

The blanking signal has to be injected to pin6, and the best point is to use the anode of D502 or pin6 of the PSM1 connector to the main chassis:
![image](chassis_blanking_injection_point.jpeg)

I took the voltage for blanking from a 5V rail, using a 150 Ohm resistor to bring it down a little bit.

### Using component for the sync signal

Since this TV has component, it's convenient to use the Y input for the sync signal: this way we will minimize or completely avoid the horizontal shift of the image when feeding RGB.

The input2 has both composite and component, and the TV switches to component if connected. The following image shows the circuit used for the selection: there is a mechanical switch connected to the Cr plug that commutes the input to pin12 of I301 high (7.5V) or low (0.5V).
![image](component_input_selector.png)
To use component then we can either keep a plug connected to Cr or force pin12 to be high.
I decided to have input2 always in component, since there are 2 more composite inputs in this TV.

To do that we can simply remove R352 and connect pin12 to a 9V rail using a 22k Ohm resistor (same as R351). Very conveniently there is a 9V rail just beside R352, and we can install the pull up resistor like in the following picture:
![image](component_input_selector_pullup.jpeg)


### Preparing the Signal Input Board

With all the above done, I first created the needed circuit on a breadboard and connected all the signal to confirm it was working as expected:
![image](chassis_signal_board_breadboard.jpeg)
Then I used my RGBmux board to implement the final version of the circuit:
![image](rgb_mux_board.jpeg)
and finally connected the remaining audio and sync pins to the component AudioL, AudioR and Y RCA pins that are exposed from the bottom of the signal board:
![image](chassis_signal_board_final_scart.jpeg)



## G1 Mod

The mod works by increasing the negative voltage bias in the G1 grid of the picture tube, that has the effect of creating a thinner, sharper stream of electrons from the cathode.
This will produce a smaller spot size on the screen, with increased sharpness and thicker scanlines.

The picture tube has 5 major components:

1. cathode K: that emits the ray of electrons
2. G1: control grid to shape the ray
3. G2: acceleration grid
4. G3: focus grid
5. G4: the actual screen with the phosphor coating

![image](elements-of-a-monochrome-picture-tube.jpg)

On consumer TVs G1 is usually set to ground to reduce complexity and cut cost. What this mod does is simply unlocking a functionality that is there and used in high-end monitors or even in much older TVs, before manifacturers started simplifying the design.

For some more theory on what this mod does you can read the [reddit post](https://www.reddit.com/r/crtgaming/comments/r51z1n/before_vs_after_mod_for_controlling_cathode_ray/?share_id=kyCU5UDmGy1ul9YzNcriI&utm_content=1&utm_medium=ios_app&utm_name=ioscss&utm_source=share&utm_term=1) from u/LukeEvansSimon, who came up with the original idea.

For this particular mod I used the board that was given to me by the owner of the TV, but the circuit is actually very simple: a rectifier with a diode and a cap. 

If you want a good read about the full story about how this mod came to be and a few alternative board designs, you can read the (long!!) [shmups thread](https://shmups.system11.org/viewtopic.php?t=67124&hilit=arcing&sid=14b0c93b8b19490ed52e1cb8de223102) from the original author.

Back to the Hitachi, what we need to do is to identify a source of negative 200V or bigger, and the flyback has pins with exactly that.
Looking at the chassis, there are pins labeled GND, 200V, 26V, 56V and we have to use the one labeled **26V** that has a positive 26V ceiling but big negative (>200V) peaks.

![image](flyback_pins.jpeg)
![image](flyback_pins_wired.jpeg)

Since we are talking about some serious voltage here, I used 16AWG wire. The following picture shows input and output from the G1 mod board:

![image](g1_board_io.jpeg)

the input (yellow) shows the 200V negative peaks, and the output (magenta) the stable negative voltage that will be applied to the G1 pin in the neckboard. In this case the voltage was set to -100V, but the specific value can be changed with the potentiometer in the G1 board.

### Neckboard modification

The last step is to isolate the G1 pin from the ground plane in the neckboard.
In this case we have to cut the ground plane near the G1 pin. 
![image](neckboard_cut_trace.jpeg)

The last thing to do is to wire it and fire it up!
![image](neckboard_wired.jpeg)

After modding the TV needs to be tuned, to find the right balance of G1 and G2. The way I did it was:

1. adjust the pot in the G1 board until I get the level of sharpness I like
2. adjust the G2 (screen pot) in the flyback to rebalance image brightness
3. repeat steps 1 and 2 until happy

In my case I ended up with -120V for G1.

One important note is not to overdo it: the risk is to have arcing between the cathode and G1, that will shorten the life of the tube. Stay below 150-200V and it should be safe.


## Final assembly

The pictures below show the final assembly, with the G1-mod board installed beside the chassis, and the RGB-mod board installed in the back shell:

![image](chassis_final.jpeg)
![image](back_inside_scart_board.jpeg)
![image](back_outside_scart.jpeg))



## Pictures

Let's conclude with a few pictures showing the TV and the image quality.

First a comparison between component / RGB / RGB+G1:
![image](comparison_sonic_logo.jpg)
![image](comparison_sonic_logo_zoom.jpg)

![image](comparison_mvc_zoom.jpg)
![image](comparison_mvc_zoom2.jpg)

And a few examples from nice games:
![image](comp_sonic_rgb_zoom2.jpeg)
![image](game_mvc_characters_g1.jpeg)
![image](game_hp_rgb.JPG)
![image](game_zoom_hp.jpeg)
![image](game_sfa3_1.jpeg)
![image](game_sfa3_4.jpeg)
![image](game_sfa3_5_g1.jpeg)
![image](input_voltage_divider.png)
![image](pattern_color_bars.JPG)
![image](pattern_monoscope_zoom_g1.jpeg)

