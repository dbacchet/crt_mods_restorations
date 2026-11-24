Repair Log
==========

Sharp C1 "Connor"
-----------------

## Symptoms 

- does not power on. power supply pops scr used in power regulation
- flyback does not oscillate
- missing one color (green)

## Work done

**Power Supply**

- troubleshooting on power supply
- desoldered and checked all component
- traced the circuit
- identified and replaced bad/borderline components:
   - zener diode ZD707
   - VR712 potentiomenter (common failure)
   - IC701 (NTE1751) was fried
   - SCR D701 (ECG/NTE5424)

**chassis**

- horizontal driver transistor was missing - added one
- cold joints and broken traces
- NES ("B") power connector joints were cracked (common failure)
- replaced deflection IC801 (Sharp X0212CE) with the one from the other TV
   - whomever worked on it before replaced with the wrong IC: NTE1547 instead of NTE1671
- recapped deflection section (~15 caps)
- audio jack board had a broken leg on the jack - fixed
   - this board is needed for the main audio to work

**neck board**

- Q851 amp transistor was missing. Replaced with a modern C3503

**NES board**

- controller connectors had bad solder work done and bad traces (previous fixes?) - resoldered and added jumpers
- reflown all cable connectors


Sharp C1 "Ryan"
---------------

## Symptoms

- does not power on. power supply pops scr used in power regulation
- solid white raster (when bypassing internal power supply)
- OSD size is too wide
- Audio volume does not go down to zero

## Work done 

**Power Supply**

- troubleshooting on power supply
- tested all component
- identified bad/borderline components:
   - VR712 potentiomenter (common failure)
   - SCR D701 (ECG/NTE5424)
- secondary transformer used for the 12V rail was bad
   - secondary winding was open - tried to fix but not possible
   - adapted a new transformer (had to adjust the pins to fit the board)

**chassis**

- cold joints and broken traces
- NES ("B") power connector joints were cracked (common failure)
- replaced deflection IC801 (Sharp X0212CE) with the correct replacement: NTE1671
- recapped deflection section (~15 caps)
- MICOM IC301 (Sharp X0371CE) troubleshooting: it's the reason for the OSD and audio volume problems
- replaced R622 that was feeding the high voltage rail to the neckboard (reason for the solid white raster)

**NES board**

- controller connectors had bad solder work done and bad traces (previous fixes?) - resoldered and added jumpers
- reflown all cable connectors

**Other**

- replaced power cord: the original plug had poor/bad contact
   - 3d printed the strain relief

Controller
----------

- does not register buttons correctly
- identified problem in the cable: bad cross talk
- replaced cable with a new one
- 3d printed strain relief

