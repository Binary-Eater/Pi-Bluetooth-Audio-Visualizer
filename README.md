# Setting Up a Raspberry Pi as a Bluetooth Audio Server Device

This guide will help you setup the Raspberry Pi to act as a Bluetooth Audio Server for making an speaker connected to the Pi's audio Jack or HDMI port serve as a Bluetooth audio device.

  - Prerequisites: An Bluetooth module (The Pi 3 has both built-in) (Preferably running Raspbian or Raspbian Lite)
  - Warning: On the Pi 3, turn off WiFi since the double module has issues streaming music over Bluetooth while having WiFi on (Noticed lag when having both on) This should not be an issue for a dedicated WiFi and Bluetooth module

## Needed Items To Download Before Proceeding

  - Before installing an updates, it is recommended to first update your Raspberry Pi firmware
```sh
sudo rpi-update
```
  - Run this
```sh
sudo apt-get update && sudo apt-get install bluez pulseaudio-module-bluetooth python-gobject python-gobject-2 bluez-tools udev
```

## Configuring PulseAudio

Add your user to the PulseAudio group. The default on Raspbian and Raspbian Lite is pi.

```sh
sudo usermod -a -G lp pi
```

Now, create the file /etc/bluetooth/audio.conf and edit it with a text editor of your choice.

```
[General]:
Enable=Source,Sink,Media,Socket
```

Then, start the PulseAudio service with the following command

```sh
pulseaudio -D
```

Now, you need to pair your phone with the Raspberry Pi
Run the following commands.
```sh
bluetoothctl
[bluetooth] list
# An output should appear representing your bluetooth dongle or the bluetooth module on the Pi 3
[bluetooth] agent on
[bluetooth] default-agent
[bluetooth] discoverable on
[bluetooth] scan on
# The MAC address of your device that you want to pair might be listed. If so, note down the MAC address that is associated with the name of the device you want to pair
[bluetooth] pair XX:XX:XX:XX:XX:XX
[bluetooth] trust XX:XX:XX:XX:XX:XX
```

Now, you should be able to connect to the Pi from your phone and stream audio to the Raspberry Pi.

You may need to force audio through the audio jack by changing the setting by running the following command.

```sh
sudo raspi-config
```

# Setting Up Adafruit DotStar LED library for Python
Download the Python libraries for the DotStar (APA102) LEDS at the following link.

https://github.com/adafruit/Adafruit_DotStar_Pi/archive/master.zip

Go into the folder and run the following commands.

```sh
make
sudo make install
```

You can now use the babumusicsync.py script in the repository to do music visualization using the Raspberry Pi.
Just be sure to set your SINK_NAME correctly based on the output of the following command.

```sh
pactl list
```

In my code, I set the SINK_NAME using the following line.
SINK_NAME = 'alsa_output.0.analog-stereo'

### Version
0.0.1

Copyright Babu

### Tech

For resources on the Bluetooth Audio Sink with the Pi:
http://raspberrypi.stackexchange.com/questions/47708/setup-raspberry-pi-3-as-bluetooth-speaker

For Adafruit DotStar Information:
https://rbgeek.wordpress.com/2012/05/14/ubuntu-as-a-firewallgateway-router/
