RGB-Mod on Sony Trinitron KV-27XBR25
===============================

![image](img/crtdb_screen.JPG)
The Sony KV-27XBR25 is a high end consumer set from 1992. It notably has the same tube of the PVM-2950Q, and a very similar internal chassis. Dark tinted tube, 600TVL on a big 29" screen, with a visible area of 27".
It comes stock with component and s-video inputs, and in this article we will show how to mod it to add RGB as well.

Service Manual: [link](https://archive.org/details/kv32xbr25/mode/2up)  
Chassis: Sony, FN  
Tube: Sony Trinitron, M68KUZ10X

In this specific mod the owner wanted to have a VGA connector to be easier to use on their setup; at the end we will have:

- _VGA connector with RGB and c-sync signals._  
C-sync is assumed to be TTL level, passed in the h-sync vga pin. Devices like MiSTer support this natively, and alder monitors like the Commodore 1084 or 2002 had the same levels in the input.
- _switch that can be used to select ON/OFF/Auto._  
When in Auto, the set uses the voltage from the VGA connector to enable RGB only when the source (i.e. MiSTer) is turned on.

We are doing a RGB-mux mod, injecting the signal into the connections used by the OSD text.
![image](img/240p_zoom.JPG)

![image](img/chassis_stock_open.jpeg)

## RGB Mod

This Sony has a CXA-1465AS Jungle chip, that supports 2 different sets of rgb inputs: digital on pins 10..12 and analog on pins 16..18. For some strange reason Sony decided to use _both_ for OSD... some menus in the OSD are going through the digital interface, while others through the analog.

![image](img/schematics_jungle.png)

As usual we need to do 3 steps:

1. connect the R,G,B, signals with the right termination and right voltage
2. provide a blanking signal to commute to RGB
3. connect the csync signal


### RGB signals
For this mod we will inject the R,G,B signals on pins 16, 17 and 18 respectively, and blanking to pin 15.

As I mentioned the TV has 2 sets of OSD inputs; the analog one is generated in the E2 board, and sent to the E1 board that is the one with the Jungle chip.

The entire mod can be done in the E1 board; unfortunately that board is soldered in the main chassis so it will have to be unsoldered and extracted first; the following pictures shows the connector that has to be unsoldered from the bottom of the main chassis A board:
![image](img/chassis_bottom.jpeg)
![image](img/e1_board_stock.jpeg)

I decided to remove the decoupling caps C330/C329/C328 and inject the signal to their positive pins, that are connected to the R,G,B inputs of the Jungle; for blanking I soldered the wire to the R377 SMD resistor.
The other two wires are ground and 9V, and there are several good points on the bottom side of the E1 board. 

The modified board look like this:
![image](img/e1_board_mod_0.jpeg)

And when installed back in the chassis this is the result:

![image](img/e1_board_mod_installed.jpeg)
![image](img/e1_board_mod_installed_zoom.jpeg)

### RGB mux circuit

We now need to create a circuit that muxes the inputs from the E2 board and those from the VGA connector properly terminating them to 75 Ohm.

The challenge here is that in the E2 board there isn't the usual voltage divider found in almost all the consumer sets where we are doing a RGB-mux mod; and to make things more interesting, the voltage level from E2 is pretty high, in the order of several volts, and doing a naive mux before the 220 Ohm input resistors will result in a level where contrast is crushed by the jungle. This was the main reason to inject directly to the jungle inputs and recreate the relevant circuit for muxing in an external circuit.

The schematic of that external circuit is the following:
![image](img/rgb_mod_circuit.jpeg)

I experimented using a breadboard first:
![image](img/breadboard_v1.jpeg)
![image](img/breadboard_v1_zoom.jpeg)

And then implemented in a smaller board. The nice thing about this design is that in the bottom there are the connectors used to pass the remaining original signals between E2 and E1; this is needed to properly maintain the TV OSD as well.
![image](img/rgb_mod_board.jpeg)
![image](img/rgb_mod_board_installed.jpeg)

### C-sync, blanking and VGA connector

The last thing we need to do is to properly inject c-sync, and we can use the luma input of the s-video connector, to minimize the horizontal image shift when using RGB.
Since we want to use TTL levels for sync (to be compliant to VGA standard levels), I added a 470 Ohm resistor between VGA pin 13 and the s-video luma input.

Also to properly use s-video as input even if there is no s-video cable plugged in we need to short to ground the selecton pin in the connector as well. This can be easily achieved by using a DPDT switch so we commute both blanking and s-video selection at the same time.
![image](img/rgb_mod_sync_and_selector_wiring.jpeg)

For blanking the feed the 9V signal from the rgb-mod board through a 2.2k Ohm resistor on one pin of the switch, and the 5V signal from VGA through a 1.5K Ohm resistor; the common pin is connected back to the rgb mod board and injected into R377 as explained above. 
The following image shows the back of the input panel with the switch and VGA connector:

![image](img/rear_panel_inside.jpeg)

And we are done!

![image](img/chassis_mod_final.jpeg)





## Final assembly

The picture below shows the final assembly, with the VGA connector and the RGB-mod board installed:

![image](img/rear_view_open.jpeg)

## Pictures

Let's conclude with a few pictures in RGB:

![image](img/game_kirby_1.JPG)
![image](img/game_kirby_2.JPG)
![image](img/game_sfa3_0.JPG)
![image](img/game_sfa3_1.JPG)
![image](img/game_sfa3_2.JPG)
![image](img/game_sfa3_3.jpeg)
