Title: Computer algebra system on a cell phone
Date: 2013-09-29 03:51
Author: Peter
Category: SymPy
Slug: computer-algebra-system-on-a-cell-phone

Around early 2008, I became obsessed with the idea of not having a
laptop: a phone could work fine. Years passed, and Linux-based phones
never caught on. I gave up waiting and bought an Android phone. After a
year of tinkering, I finally have a graphical computer algebra system
running on it. It is an important step toward dodging my laptop
entirely, although more work lies ahead.

[caption id="attachment\_546" align="alignnone" width="620"][![Phone
calculating the symbolic derivative of an implicit
function.](http://peterwittek.com/wp-content/uploads/2013/09/spyder_on_phone-1024x538.jpg)](http://peterwittek.com/wp-content/uploads/2013/09/spyder_on_phone.jpg)
Phone calculating the symbolic derivative of an implicit
function.[/caption]

Hardware
========

The phone is a Sony Xperia Arc S from 2011. It has a 1.4 GHz Scorpion
Snapdragon CPU, which is a single-core ARM Cortex-A8 unit with ARMv7
instruction set. A quick comparison with hardinfo showed that it was
about ten times slower than the single-core performance of the i7-620M
processor in my laptop, which peaks at 3.33 GHz. The FPU is much slower
than that.

[caption id="attachment\_570" align="alignnone" width="600"]![Comparing
an ARMv7 and an x86\_64
CPU](http://peterwittek.com/wp-content/uploads/2013/09/benchmark.png) A
quick performance comparison between an i7-620M and a Cortex-A8
Snapdragon CPU. The benchmarks were performed in hardinfo. Lower values
are better.[/caption]

A more limiting constraint is the RAM, which is only 512 MBytes. Bash
reports even less than that, about 360 MBytes. Large-scale symbolic
calculations will not happen on this hardware.

The screen is a 4.2-inch, true-colour TFT, with 854x480 pixels.

Software
========

The phone was rooted through official Sony channels, and it runs
Cyanogenmod 10 (Android 4.1.2). Proprietary software was not
installed.[F-Droid](https://f-droid.org/ "F-Droid")replaces Google Play
as the software market. The kernel version is 2.6.32.

You will need
a[terminal](https://f-droid.org/repository/browse/?fdfilter=terminal&fdid=jackpal.androidterm "Terminal Emulator"),
a [VNC
viewer](https://f-droid.org/repository/browse/?fdfilter=vnc&fdid=android.androidVNC "androidVNC"),
and [Complete Linux
Installer](http://sourceforge.net/projects/linuxonandroid/files/App/ "Complete Linux Installer")
to proceed.

Get Linux Running
=================

Complete Linux Installer needs you to store the image file in an
appropriate folder in the root of the sdcard, e.g. archlinux for Arch,
ubuntu, and so on. Arch Linux does not work well, the X server produces
garbled screens. I recommend the [small
image](http://sourceforge.net/projects/linuxonandroid/files/Ubuntu/13.04/Small/ "Ubuntu ARM small")
of Ubuntu 13.04. It includes the LXDE desktop.

There is no actual installation taking place. The \`\`installer" is a
bash script that runs in a terminal and chroots into the image you
downloaded. When you run the script for the first time, you will have to
answer a few questions. Choose a passsword for your user. Then say yes
to both ssh and VNC, and set resolution to 854x480. You can change these
latter options in /root/cfg/ubuntu.img.cfg.

Configure Linux
===============

Connect to the wireless network. The terminal should be logged in as
root in your chrooted environment. Find out your IP address:

<div class="highlight">

    # ifconfig

</div>

Typing commands on your screen is excruciating, so log in to your phone
from your computer;

<div class="highlight">

    $ ssh ubuntu@ip_address

</div>

Install screen to avoid problems with dropping wifi signals:

<div class="highlight">

    $ sudo apt-get update
    $ sudo apt-get install screen
    $ screen

</div>

Install Spyder and Sympy:

<div class="highlight">

    $ sudo apt-get install python-sympy spyder

</div>

This will take hundreds of megabytes to download and more to install.
The image file will be nearly full, so delete all the cached files after
the operation completes:

<div class="highlight">

    $ rm -fr /var/cache/apt/archives/*

</div>

You are ready to access the X client through VNC. The solution is
inefficient. X could run in the framebuffer. Android and X cannot share
the same framebuffer, so unless you are ready to work with Linux alone,
you must use a virtual screen and access it through VNC.

The settings should be ubuntu for the password, localhost for the
address, and 5900 for the port. Set colour format to 24-bit. The desktop
should show if you press connect.

LXDE is not a touch-friendly interface. Pan in to click more precisely.
Disable the screensaver.

Running the Computer Algebra System
===================================

Launch Spyder from the run menu. It will take a while. If the CPU load
diagram does not show high-CPU load, then the launch failed due to
memory problems. Restart the phone, and try again.

Start an Ipython interpreter in Spyder. The Ipython configuration file
does not work correctly. To have pretty printing, you have to initialize
Sympy's appropriate function:

<div class="cell border-box-sizing code_cell rendered">

<div class="input">

<div class="prompt input_prompt">

In [1]:

</div>

<div class="inner_cell">

<div class="input_area">

<div class="highlight">

    from sympy import *
    init_printing()

</div>

</div>

</div>

</div>

</div>

You are ready to test symbolic computations:

<div class="cell border-box-sizing code_cell rendered">

<div class="input">

<div class="prompt input_prompt">

In [2]:

</div>

<div class="inner_cell">

<div class="input_area">

<div class="highlight">

    x, y = symbols('x y')
    diff(sin(x*y)**x, x)

</div>

</div>

</div>

</div>

<div class="output_wrapper">

<div class="output">

<div class="output_area">

<div class="prompt output_prompt">

Out[2]:

</div>

<div class="output_latex output_subarea output_pyout">

\$\$\\left(\\frac{x y \\cos{\\left (x y \\right )}}{\\sin{\\left (x y
\\right )}} + \\log{\\left (\\sin{\\left (x y \\right )} \\right
)}\\right) \\sin\^{x}{\\left (x y \\right )}\$\$

</div>

</div>

</div>

</div>

</div>

Future Work
===========

Using VNC is an inefficient solution. Apart from the memory requirements
of the client, rendering is slow. The X server should run in the
framebuffer. Since this disables the Android graphics stack, phone
functionality must be replaced by X-friendly solutions. [Firefox
OS](https://www.mozilla.org/en-US/firefox/os/ "Firefox OS") or [Sailfish
OS](https://sailfishos.org/ "Sailfish OS") might be a long-term
solution. The former already
[works](http://neuralassembly.blogspot.jp/2013/06/xperia-arcxperia-rayfirefox-os.html "Xperia Arc running Firefox OS")
on an Arc S. The software side is nearing completion.

If you want a phone-powered laptop, the hardware side looks bleak.
Linking up the phone with a Bluetooth keyboard is easy. Programming the
phone's screen to function as a touchpad should not be difficult
either -- some [remote touchpad
solutions](http://www.remotedroid.net/ "RemoteDroid") work this way.

The component that is missing is a portable, battery-powered screen that
takes HDMI input. [Casetop](http://livi-design.com/ "Casetop")adds a
tiny 11-inch screen a keyboard to your phone. GeChic gives you a
[15.6-inch portable
screen](http://www.gechic.com/product_help_en.asp?s=6 "On-Lap 2501M"),
but the resolution is only 1366x768, and it weights a kilo.

You could salvage a full-HD LED panel from a laptop, or order a
replacement screen. The panels use a low-level interface called LVDS.
This is specific to each panel, and converting an HDMI input to this
signal is not trivial. Chalkboard Electronics
[sells](http://www.chalk-elec.com/?page_id=1280 "LVDS LCD interface board")
a programmable converter for \$39, but it gives you little information
on how to programme the interface for an arbitrary panel. Also, the
board includes a power output for LCD screens, but it is superfluous for
LED-lit panels.

If you select your components carefully, you should be able to construct
a 17-inch laptop that weights about one kilogram, excluding the
processing unit, the phone. Perhaps we will get there in the next year
or two.
