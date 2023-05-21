# Restoring a Commodore 1084s

A few weeks ago a friend was nice enough to bring me a Commodore 1084s monitor he found in an e-waste center. 
The 1084 was in working condition, but pretty beaten and with a few issues:

1. scratches/scuffs on the surface (but not on the tube, yay!)
2. the power button was sticky (common failures on these monitors)
3. the RGB input port was not working
4. signs of rust on the back connectors

A few images:
![1084_front](game_yoshii.jpeg)

This monitor is a dream for whomever, like me, that grew up with a Commodore64 or Amiga, so I was super excited to restore it and to play with it!

For the RGB port, we tried connecting both a regular rgb source (a PSX) and an Amiga 1200 (kindly borrowed from) and and the symptom was the same: no image at all, with some faint noise. We need to trace the signal and see where it gets lost...

After opening the monitor I realized that the poor thing probably went through some very rough time, likely stored in a very humid place. There was extensive rust on almost all the metal parts, and corrosion on the main board:

![rust1](rust1.jpeg)
![rust2](rust2.jpeg)
![rust4](rust4.jpeg)

The corrosion could also explain the RGB input port not working. 
So, challenge accepted! Let's bring this piece of history back to his full potential!

The first step was to perform some good cleaning, and get rid of the rust on the metal structure and on the shields. The shields are soldered on the main chassis, so you need to carefully desolder them using a soldering iron and a pump/desoldering gun. I decided to use a bath of white vinegar to remove rust, instead of more aggressive chemical solutions:

![rust7](rust7.jpeg)


While the metal parts were taking a bath, I gently cleaned the main chassis with a brush, cotton swabs and using 99% isopropyl alcohol; with the board not cleaned I started looking for damages, especially on the area around the RGB connnector.

One thing that I found suspicious was that the csync input pin was _bridged_ with hsync, but a couple of experts in a crt forum confirmed that it is normal, and the 1084 can detect composite sync in the hsync line; also the symptoms (black screen) pointed to something not sync related.

So I first tried injecting directly the RGB signals on the chroma IC, passing sync through the connector, and... ...it worked! Great, so now it was clear that the problem is between the rgb input pins and the chroma.
Since I did not have a service manual with schematics (u/FerrisonHorde has a 30 years old _paper_ version of a PAL 1084, that was unfortunately different), I traced the signal backwards from the Chroma, and found a few places that needed reflowing and a trace that needed to be rebuilt. Also the RGB switch was in need of some care, so I desoldered it, cleaned and resoldered it back in.

Result? We have an image in RGB now!

But passing in a grid showed some wiggly vertical lines: ![grid_wiggly](grid_wiggly.jpeg)

That's usually produced by a voltage regulation problem, so I checked with an ESR meter the capacitors in the deflection and power circuits and found a few that were borderline or bad. Replacing them fixed the problem: ![grid_good](grid_good.jpeg)

Now back to the rusty metal parts... 
After several hours in the solution, and gently scraping with a brush, the result was quite good:

![final5](final5.jpeg)
![final2](final2.jpeg)
![final3](final3.jpeg)

The last thing to do was to replace the power button. Another person in the forum recommended a compatible part (4$ instead of 30$ for a stock replacement!) and swapping them was a pretty easy job, reusing the mounting bracket from the original push button:
![power_button](power_button.jpeg)

Time to reassemble the monitor, clean and polish all the scratches and enjoy it in all its glory!
![final7](final7.jpeg)
![sfa3](game_sfa3.jpeg)

Some of the scratches were too deep to be completely removed, but I'm pretty happy with the result! Also the image quality is incredible for a monitor that is 34 years old!
And it will make the perfect partner for an Amiga or C64!
