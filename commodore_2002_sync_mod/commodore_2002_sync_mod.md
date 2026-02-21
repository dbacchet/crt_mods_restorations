Sync Mod on a Commodore 2002 CRT Monitor
===========================

![image](game_crtdb.JPG)
The commodore 2002 is a great monitor, functionally identical to the more famous 1084-D. 
The CRT tube is the same (see picture with the tube number and reference to the 1084-D page on [CRTDatabase](https://crtdatabase.com/crts/commodore/commodore-1084-d)); interestingly enough, even if the tube number is the same the tint is slightly lighter compared to the 1084-D, probably manufacturing differences since these monitors were produced a few years apart.

The chassis design is very similar as well, with one significant (to me...) difference: for Analog RGB, the 2002 _only_ accepts TTL sync, and it will not work natively with 75 Ohm sync coming from consoles, etc.

![image](2002_sm_inputs.png)

Well, let's fix this! 

We can modify the monitor such that it will accept _both_ TTL and 75 Ohm signals; it will require "normalizing" the signal level before it's processed by IC7203 at pin 9.

![image](2002_sm_schematics.png)

Very conveniently there is R7206 that connect the csync input to the IC, and can be used to inject the normalized signal.

![image](chassis_1_orig.jpeg)
![image](chassis_2_top.jpeg)

## Processing the sync signal

How can we take a generic sync signal and convert it into a clean TTL sync?
Luckily there is an IC that does exactly that, the LM1881!

The datasheet shows the minimal circuit needed to achieve our goal:

![image](lm1881_circuit.png)

- input sync signal at pin2, through a 0.1 uF capacitor
- output at pin1, already at TTL level
- RC circuit connected at pin6
- ground and Vcc at pins 4 and 8

I experimented with a breadboard, taking the input from one side of R7506 and injecting the TTL sync on the other side. 

![image](chassis_3_removed_resistor.jpeg)
![image](chassis_4_breadboard.jpeg)

Worked really well, so I converted that into a small circuit and installed it in the commodore!

![image](mod_circuit_v1.jpeg)
![image](chassis_5_modchip_installed.jpeg)

## Oscilloscope signals

### Before the mod

If we input a TTL sync signal (yellow), the monitor can correctly generate a good sync (purple).

![image](sync_1_orig_TTL_in.jpeg)

If we input a 75 Ohm sync signal (yellow), the monitor is not able to generate proper sync (flat purple line).

![image](sync_2_orig_75ohm_in.jpeg)

### After the mod

If we input a TTL sync signal (yellow), the monitor can correctly generate a good sync (purple).

![image](sync_3_mod_ttl_in.jpeg)

If we input a 75 Ohm sync signal, now the monitor can properly generate a good sync!

![image](sync_4_mod_75ohm_in.jpeg)

## Improved design

Since the experiment went well I created a more integrated version of the same circuit, with also the option to define the sync output level, for cases where we want to have a specific peak to peak value.

Installed this on another Commodore 2002, see the following pictures:

![image](mod_circuit_v2.jpeg)
![image](mod_installed.jpeg)

## Other images

![image](crt_tube.jpeg)
![image](game_240p.JPG)
![image](game_smw.JPG)
