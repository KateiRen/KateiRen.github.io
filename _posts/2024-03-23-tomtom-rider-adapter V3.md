---
layout: post
title:  "Tomtom Rider Adapter V3"
author: karsten
categories: [ 3D-Printing, motorbike ]
image: assets/images/2024-03-23/20240322_125245.jpg
beforetoc: "Get juice for your mobile instead"
---

# Aiming for the max (V3 =  USB-C Socket)

With USB-A and 5V, the max charging current seems to be 1A.
With USB-C and 5V it seems that 1,5A are possible. So lets do another (last?) version.

At least according to this [source](https://forum.digikey.com/t/simple-way-to-use-usb-type-c-to-get-5v-at-up-to-3a-15w/7016/33) higher currenty on USB-C with 5V should be easy. According to the headline, up to 3A should be working.

I ordered SMD type USB-C sockets which I found out without even trying that these small pins will not become friends of my loose-in-the-air resistor cabling.

Oh, there are pretty nice small boards with soldered USB-C sockets availabel as well.
Let's order these and wait again.

When the mini boards arrived, I just had to adjust the 3D model again, making it a nice housing for the small board instead.

## Putting it together

![The board fits nicely into the bottom part](/assets/images/2024-03-23/20240321_231059.jpg)

![I decreased the height of the top part to save on printing time](/assets/images/2024-03-23/20240321_231104.jpg)

![The USB-C port seen from the side](/assets/images/2024-03-23/20240322_091409.jpg)

![Fixing everything with some two-component glue](/assets/images/2024-03-23/20240322_103936.jpg)


## Result

Measurements done on the laboratory power supply show that my Galaxy S21 draws around 1,5A from the socket when connected to 5V. Not quite the 3A that seem to be possible as well, but better than the 1A from previous model.

![All three versions of the design next to each other](/assets/images/2024-03-23/20240322_125245.jpg)

3D Models can be found (here)[https://www.printables.com/de/model/817171-tomtom-rider-mount-adaptor-remix-v3]