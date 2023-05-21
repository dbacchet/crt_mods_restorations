Philips 13PT30 L601
===================

**Specifications**

**Manufacturer:** [Philips](https://crtdatabase.com/crt-discovery-tool?manufacturer=Philips)

**Model:** 13PT30 L601

**Size (Viewable):** [13"](https://crtdatabase.com/crt-discovery-tool?size=13%22)

**Inputs:** [Composite](https://crtdatabase.com/crt-discovery-tool?inputs=Composite),  [RF](https://crtdatabase.com/crt-discovery-tool?inputs=RF)

**Resolutions:** [240p](https://crtdatabase.com/crt-discovery-tool?resolutions=240p),  [480i](https://crtdatabase.com/crt-discovery-tool?resolutions=480i)

**Horizontal Max:** [15 kHz](https://crtdatabase.com/crt-discovery-tool?horizontal=15%20kHz)

**Vertical Max:** [60 Hz](https://crtdatabase.com/crt-discovery-tool?vertical=60%20Hz),  [50 Hz](https://crtdatabase.com/crt-discovery-tool?vertical=50%20Hz)

**Formats:** [NTSC](https://crtdatabase.com/crt-discovery-tool?formats=NTSC)

**Aspect:** [4:3](https://crtdatabase.com/crt-discovery-tool?aspect=4%3A3)

**Adjustments:** [OSD Customer Controls](https://crtdatabase.com/crt-discovery-tool?adjustments=OSD%20Customer%20Controls)

**Jungle:** TDA8846

**Tube:** [Chunghwa](https://crtdatabase.com/crt-discovery-tool?tube=Chunghwa),  A34AGT13X95 E

**Mask:** [Slot](https://crtdatabase.com/crt-discovery-tool?mask=Slot)

**Speakers:** [Stereo](https://crtdatabase.com/crt-discovery-tool?speakers=Stereo)

**Chassis:** 13E800 Chassis

**Launched:** [1999](https://crtdatabase.com/crt-discovery-tool?launched=1999)

**Country of Manufacture:** Singapore 

**Mods:** [RGB via Mux](https://crtdatabase.com/crt-discovery-tool?mods=RGB%20via%20Mux)

**Degaussing:** [Automatic on Power-on](https://crtdatabase.com/crt-discovery-tool?degaussing=Automatic%20on%20Power-on)

# Summary

13" curved screen TV from Philips, with a peculiar analog clock in the front.

# Literature

-   [Service Manual](13pt30l1.pdf)
-   [Chroma Datasheet](TDA-884.pdf)

# Notes
This small Philips TV has a Chunghwa slot mask tube, that is not very high TVL, but very sharp.  The clock is controlled by the TV menu, and can be used as an alarm with different animal sounds.

Like many Philips TVs, this model is capable of syncing to 50hz. Pal video will still display in black and white because the TDA8846 chroma chip only supports NTSC.

This model, as with all the models that use a TDA884X chroma, can be modded via RGB Mux, and the results are very good.

# RGB Mod

The service manual is very difficult to read, but tracing back the RGB input pins in the TDA-8846 (pin 23,24,25) you can find the grounding resistors 3647, 3648, 3649, that are 82 Ohm SMD resistors. Remove them as shown in the following picture:
![ground resistors](mod_ground_resistors.jpeg)
Since 82 Ohm is very close to the standard 75 Ohm termination value in the external RGM lines, there is no need to add any inline resistors in the  mux board (see how the inline resistors have been replaced by jumper wires in the small mux board):
![mod_wiring](mod_wiring.jpeg)
In the picture above you can also see how the external RGB lines have been terminated with a starndard 75 Ohm resistors.

The blanking signal on this chroma is a little delicate, as it only accept a very limited range of voltages around 1.5V: too low and it will not trigger and too high it will produce a black screen. Depending on where you get your source voltage from you might need to experiment with an inlne resistor, or a small pot. In the picture above we got 5V from a voltage regulator.

The signals can be injected to the jumpers 9258 (R), 9259 (G), 9260 (B) and 9261 (Blank) as shown in the following picture:
![mod_wiring_conn](mod_wiring_conn.jpeg)

The result is a very nice and sharp image!

# Gallery
![](tv_front.jpeg)
![](tv_back.jpeg)
![](tv_open.jpeg)
![](tv_grid.jpeg)
![tube](tube.jpeg)
![img1](img_sfa3_1.jpeg)
![img2](img_sfa3_2.jpeg)
![img3](img_sfa3_3.jpeg)