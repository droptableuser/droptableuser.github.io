---
title:  "34C3 Junior CTF - information_society"
date:   2018-01-03 21:45
---
The challenge consisted of an [audio file](../files/information_society_bonustrac.flac)

The name was already a hint. The band [Information Society](https://en.wikipedia.org/wiki/Information_Society_(band)) released on their 3rd album a [hidden track](http://www.eeggs.com/items/3840.html). This track could only be accessed through playing it into a modem. 

Upon listening to the sound file it became clear, that it had to be a modem sound, so a program to reverse the data into its original form was required. Intro [minimodem](https://github.com/kamalmostafa/minimodem). 
It can be installed in Kali via:
{% highlight bash %}
apt-get install minimodem 
{% endhighlight %}
The hints from the hidden track proved quite invaluable as 8 data bits and 300 baud were the same values required to resemble this audio file.
{% highlight bash %}
minimodem --read 300 -a -8 -f information_society_bonustrack.flac > information_society_bonus
{% endhighlight %}
The resulting [file](../files/information_society_bonus.png) was:
{% highlight bash %}
file information_society_bonus
information_society_bonus: PNG image data, 657 x 100, 8-bit/color RGBA, non-interlaced
{% endhighlight %}
