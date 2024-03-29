---
layout: post
title:  "Tomtom Rider Adapter V2"
author: karsten
categories: [ 3D-Printing, motorbike ]
image: assets/images/2024-03-20/20240321_221408.jpg
beforetoc: "Get juice for your mobile instead"
---

# Going for a second edition (with USB-A Socket)

Its the time of the year again. The weather gets warmer and the next trips on the bike 🏍️ are getting closer. [The adaptor from last year](https://kateiren.github.io/tomtom-rider-adapter/) didn't hold to its promises. My phone ran out of battery while driving more than once.
I wasn't able to get the power out of the charger to keep the deivce juiced up during navigation.

Thats why I started tinkering on the adaptor again. Last time I wasn't 100% following the instructions as the required resistors were out of the usual-to-be-found-steps in my resistor library. Combining some ordinary resistors in a way to get to the same voltage divider was not possible due to the lack of space in the design. 

This time I was ordering exactly whats needed plus a USB A socket to stay flexible with the cabling.
By the way, the information behind tweaking the USB charging port can be found on this website [https://learn.adafruit.com/minty-boost/icharging](https://learn.adafruit.com/minty-boost/icharging).

So modifying the 3D model again and a few tests later assembling could start.

## Putting it together

![The new bottom part, still on the printing bed](/assets/images/2024-03-20/20240321_150601.jpg){: width="500" }

![The bottom part and top part next to each other](/assets/images/2024-03-20/20240321_221418-2.jpg){: width="500" }

![zooming in on the wiring inside the bottom part](/assets/images/2024-03-20/20240321_221408-2.jpg){: width="500" }

![Side view of the USN Socket](/assets/images/2024-03-20/20240322_091342-2.jpg){: width="500" }

![Upside down view, showing the contact surfaces](/assets/images/2024-03-20/20240322_114911-2.jpg){: width="500" }

![Sealing the adaptor after checking that there is no short circuit](/assets/images/2024-03-20/20240322_103716-2.jpg){: width="500" }


## Result

Measurements done on the laboratory power supply show that my Galaxy S21 draws around 1A from the socket when connected to 5V.

Hooked up with the bike, the phone tells me at 58% that it needs 1 hour and 1 minute until fully charged. Let's see if that gives enough juice when the display, LTE and GPS is switched on...

![Final result of the adaptor](/assets/images/2024-03-20/20240322_125221.jpg){: width="500" }
