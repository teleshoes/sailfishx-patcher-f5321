sailfishx-patcher-f5321
=======================

This repository contains a tool that permits to apply the [Xperia X Compact compatibility layer](https://github.com/g7/droid-compat-f5321)
on top of official Sailfish X images.

To keep things simple and clean, this guide will show how to patch inside a Vagrant environment.  
You can run it as well without vagrant, but is not recommended. Please read anyway the Vagrantfile to
get the required dependencies.

The following may also work on Windows and macOS machines, but it hasn't been tested (testers welcome!).

Frequently Asked Questions
--------------------------

#### Q: How does this thing work?

A: [droid-compat-f5321](https://github.com/g7/droid-compat-f5321) is a compatibility layer that I've developed
in order to use official Sailfish X images on the Xperia X Compact.

This layer works by overriding some hardware specific parts of the Xperia X adaptation with the ones of the
Xperia X Compact, and where the override operation is not applicable, the patched files are applied
via a diversion, using a rudimentary tool called [rpm-divert](https://github.com/g7/rpm-divert).

The kernel image is patched, so the whole official adaptation is working on the X Compact.

The kernel patch is done with another tool that I've developed, [yabit](https://github.com/g7/yabit).

#### Q: What Sailfish X images are supported?

A: Sailfish X for Xperia X Single Sim (F5121). Currently the following images have been patched successfully:

* Sailfish X F5121 2.1.3.7
* Sailfish X F5121 2.1.4.14
* Sailfish X F5121 2.2.0.29

#### Q: What doesn't work?

A: The only things that don't work are:

  * USB OTG (the Xperia X kernel does not supply the driver for the USB Type-C controller of the X Compact)
  * Some secondary sensors (magnetometer, gyroscope, pressure, step counter). This is temporary until I narrow down a battery drain issue I experienced with those.

#### Q: Is it stable?

A: I'm using a patched image on my daily driver since April 2018. Before I was running a custom-built
2.1.2 image and I've yet to notice differences stability-wise.

#### Q: Do I have access to the Sailfish X licensed content (aliendalvik, text prediction, etc)?

A: If your Sailfish X license is valid, yes. Jolla will see your device as a standard, single-sim, Xperia X.

Alien Dalvik and the other 3rd-party content work fine.

#### Q: Are Sailfish Over The Air (OTA) updates safe?

A: They should be. The compatibility layer has been developed with OTAs in mind.

So far I haven't had any issues with upgrades. My daily driver has had the following updates since April:

2.1.3 (start) -> 2.1.4 -> 2.2.0 -> 2.2.1

#### Q: Does this mean that I can run official Sailfish X on other Xperia devices?

A: No. The Xperia X and the Xperia X Compact are so similar that an approach similar to mine
can work. Other Xperias (X Performance, XZ*) do not share anything about the hardware so unfortunately
things can't work.

But I expect that a similar approach can be taken to patch the Sailfish X images for the XA2 for the XA2 Plus and Ultra.

#### Q: Hey, I like what you've done! Can I buy you a beer?

A: Sure! Head up [here](https://www.paypal.com/cgi-bin/webscr?cmd=_s-xclick&hosted_button_id=HVAFQSLM94DLW&lc=en_XC). Thanks!

Requirements
------------

* An official Sailfish X image, for the Xperia X Single Sim (F5121)
* [vagrant](https://www.vagrantup.com)
* [virtualbox](https://www.virtualbox.org)
* ~15 GB of disk space

How to patch
------------

First of all, clone this repository and its submodules:

	git clone https://github.com/g7/sailfishx-patcher-f5321.git
	cd sailfishx-patcher-f5321
	git submodule init
	git submodule update

Then, install (if you haven't) the vagrant-vbguest plugin:

	vagrant plugin install vagrant-vbguest

Bring up the vagrant environment. This might take a while:

	vagrant up

If everything went well, copy the official Sailfish X image to the `sailfishx-patcher-f5321` directory,
then start the actual patching process:

	vagrant ssh -c "/vagrant/patch.sh -a f5321 -i /vagrant/Sailfish*.zip"

Note: **Do not** change the "`/vagrant`" directory! It already references the `sailfishx-patcher-f5321`
directory where you copied the zip file.

If everything is successful, you should get a patched zipfile in the very same directory.

You can use the official Sailfish X installation instructions to flash to your device. Enjoy!

To destroy the vagrant image, simply execute

	vagrant destroy
