# Adam's Hexapod

This is the Go program which powers my hexapod. It's not terribly impressive
compared to some of the hexapods out there, but it's my first real hardware
project, and I've learned a lot while putting it all together.

It's a 3d-printed skeleton bolted to a bunch of Dynamixel servos, powered by a
[Raspberry Pi](http://www.raspberrypi.org) and manually controlled by a Sony
Sixaxis controller. Each leg has 4DOF, which makes the gait very flexible.

Here's the SketchUp model it's printed from:

![hexapod sketchup model](http://cl.ly/image/0c3p3h2R0N1R/hexapod-sketchup-20141125.png)


## Hardware

* [Printrbot Simple Metal](http://printrbot.com/shop/assembled-simple-metal/) 3d printer
* [Dynamixel AX-12A](http://www.trossenrobotics.com/dynamixel-ax-12-robot-actuator.aspx) servos
* [Raspberry Pi, Model B+](http://www.raspberrypi.org/products/model-b-plus/) computer
* [Sony Sixaxis](https://en.wikipedia.org/wiki/Sixaxis) controller
* [USB2AX](http://www.xevelabs.com/doku.php?id=product:usb2ax:usb2ax) dynamixel interface
* [SparkFun USB MicroB Breakout](https://www.sparkfun.com/products/10031)
* [RioRand LM2596](http://amzn.com/B008BHAOQO) voltage regulator
* [Tiger 11.1v 2200mAh LiPo](http://www.trossenrobotics.com/3s-11v-2200mah-25c-lipo-battery) battery
* [Mean Well GS90A12-P1M](http://www.jameco.com/webapp/wcs/stores/servlet/Product_10001_10001_2078291_-1) power supply
* [Edimax EW-7811Un](http://amzn.com/B003MTTJOY) wifi adapter
* [Medialink MUA-BA3](http://amzn.com/B004LNXO28) bluetooth adapter


## Usage

1. Spend countless hours and dollars printing and assembling the hexapod. Be
   sure to blow up RPi and trap fingers between moving parts for authentic
   experience.

2. Provision the RPi using [adammck/hexapod-infra](https://github.com/adammck/hexapod-infra).
   It runs Pidora 2014 with [QtSixa](http://qtsixa.sourceforge.net), the control
   program (this repo), and a few systemd services to glue everything together.

3. Flip the power switch to boot the hexapod. If you're running tethered with an
   external power brick, make sure that the power switch is off to isolate the
   LiPo before plugging it in.

4. Plug the Sixaxis controller in with a USB cable. You only have to do this
   once, to pair it with the Bluetooth adaptor. Give it a few seconds (to run
   sixpair), then unplug it.

5. Build and deploy:

        bin/pi-deploy main/bot.go

   This requires Go to be installed with cross-compilation support for
   Linux/ARM. That's outside of the scope of this document, but it's easy.

6. Press the PS button to pair. The controller should rumble and flash its
   lights. The control program will now start, and the hexapod will initialize
   and stand up.

7. Use the left stick to translate, and the right stick to rotate. Use L2 and R2
   to adjust the ground clearance and step height. Wheee, this is fun!

8. Press Select and Start to shut down the servos and the RPi. Note that this
   doesn't entirely kill the power, so don't forget to disconnect the LiPo to
   avoid damaging it.

   Shut down remotely by running:

        bin/pi-poweroff

    Shutdown will automatically occur (with no warning) when the battery drops
    below 9.6 volts. This is to protect the LiPo. My 2200mAh battery usually
    lasts about 15 minutes on a full charge.


## TODOs

It's a miracle this damn thing works at all. Here are some problems which I'm
currently aware of, which may or may not ever be fixed.

* The ground is assumed to be an infinite plane, which is (apparently?!) not in
  fact true. I'd like to add pressure sensors to the feet, to make the gait more
  flexible.

* Inside the chassis is *very* cramped and probably a fire hazard. Neater wiring
  would be nice.

* Lots of my toy 3d math code remains. Now that I've got the hang of that stuff,
  it should all be removed in favor of a more comprehensive (and better tested)
  third-party library.

* We're not using the Sixaxis' gyroscope or accelerometer yet, which seems like
  an awful shame.

* Enthusiastic dancing (i.e. heavy load) while tethered sometimes causes reboots
  because my power brick can't supply enough power, so cuts out. Should probably
  upgrade.


## License

MIT
