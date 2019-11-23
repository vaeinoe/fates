# Fates DAC board for Raspberry Pi

Fates is an audio and UI platform for Raspberry Pi 3 Model B+ and Pi 4 Model B that provides a stereo audio codec with headphone driver, 3 (or 4) encoders, 3 buttons, a 128x64 OLED display, 2x audio ins, and 2x audio outs. 

Fates is fully compatible with the [monome norns](<https://github.com/monome/norns>) software ecosystem. A “DIY norns” if you like. Once installed, Fates can be used with the primary norns codebase, without any branches or mods.

Fates can also be used directly with Orac 2.0 (puredata) and also with other Raspberry Pi audio projects like PatchboxOS, etc.

NOTE - this install and norns software has been tested mostly on the Pi3b+ and Pi4b. It may not work perfectly on older/slower Pi models (3b, etc.) and may need different configuration. 

### Specs:

- WM8731 stereo audio codec with headphone driver
- NHD-2.7-12864WDW3 128x64 grayscale OLED display
- 3 pushbuttons
- 3 rotary encoders (optional 4th encoder for Orac or other software)
- 2x 1/8in inputs
- 2x 1/8in outputs
- 1/8in stereo headphone out

Fates was designed for Raspberry Pi 3 Model B+ and Raspberry Pi 4 Model B. It’s not been performance tested with earlier Raspberry PI models.

For Raspberry Pi 4, Fates includes a USB-C power jack which fixes the Pi4 issue with "e-marked" USB-C cables not powering the device.

Fates can be powered either from it's own USB-C power jack, or the Raspberry Pi power jack (but NOT both).

![<fates pcb top>](<hardware/fates1.8.1_top.png>)

## BOM and Build 

[BOM](hardware/BOM.md)  

[BOM - Thru-hole only](hardware/BOM-thruhole.md)  (for SMD assembled boards)

[Build Guide](hardware/Build.md)

[Acrylic Case Assembly](hardware/AcrylicCase.md)



## Install instructions

- [Installing Fates 1.8+ from disk image](https://github.com/okyeron/fates/blob/master/install/norns/Norns_disk_image_install.md) **(recommended)**  


- [Installing Orac](https://github.com/okyeron/fates/blob/master/install/orac/README.md) (BETA)

## Troubleshooting

- Be sure you are using a good power supply. The Pi 3b+ needs a 5V 2.5A power supply. The Pi 4 requires a 5V 3A power supply. Buy one of the official Raspberry Pi power supplies if you're not sure.

- DO NOT TRY TO POWER FATES FROM YOUR LAPTOP USB-C Ports. Use a dedicated 3A power supply.  

- [Check your input voltages](hardware/Build.md#tip---test-voltage)

- `SUPERCOLLIDER FAIL` error on boot: This happens because the Jack Audio system is not starting properly. A number of things can cause this. There is a information and support thread on the "lines" forum [here](https://llllllll.co/t/fates-a-diy-norns-dac-board-for-raspberry-pi/22999?u=okyeron)

### Audio tests

SSH to the pi to conduct these tests. Have your audio outputs and inputs connected to speakers/mixer and a sound source.  

A low level test…  
Firstst stop jack, so we can test the DAC directly with ALSA

`sudo systemctl stop norns-jack.service`  

Now use `aplay` to play a wave file.

`aplay ~/dust/audio/common/waves/01.wav`
this should play a simple clean bell tone

An alternate test is speaker-test  
`speaker-test -t wav -c 2 -l 3 -D hw:0,0`

This will play a female voice saying "front right" and "front left" in each channel 3 times. 

Record 15 seconds of audio from the inputs and save to a .wav file 
`arecord -f dat -vv -V stereo -d 15 ~/audio-test.wav`

Play back the same audio file  
`aplay -vv -V stereo ~/audio-test.wav`

`sudo reboot` to get things back to normal. 

## UART

Fates includes UART pins broken out for a serial connection to another computer using a UART-USB cable.

For example - the [Adafruit 954 cable](https://www.adafruit.com/product/954):
- white lead TX
- green lead RX
- black lead GND
- ***red lead (5v) from the UART cable is not connected on Fates***

Then connect using `screen`

  `screen /dev/cu.usbserial* 115200`

If this does not work, try swapping TX and RX
