# Raspberry Pi cam example
This is a small showcase of a small device with a small camera.  
In case you are trying to set up a custom-made small security cam or a fun livestream only for your housemates, this may help you.

## Hardware Setup
The following hardware is used: 
* Raspberry pi 2 Model B V1.1
* SanDisk Ultra 16 GB micro SD HC1 10 A1
* Raspberry Pi camera Model 3 wide NoIR with 12-megapixel Sony IMX708 image sensor
* That one is buyable at [Raspberry.com](https://www.raspberrypi.com) -> look for Camera module 3
* Ethernet network setup for Raspberry Pi

## How to install
I refer to the micro SD card as SD card. It is simpler to write for me.
Abbreviation for Raspberry Pi is Rpi

First flash the SD card with Raspberry Pi OS - my hardware has with the specification of Raspberry Pi 2 Model B V1.1 a 32-bit architecture. So I take the Raspberry Pi 32-bit lite Version [(Download OS)](https://www.raspberrypi.com/software/operating-systems/) [(Link to the reference, it is open source)](#References).  
Connect the camera cable to the camera slot on the Raspberry Pi. This is a three step process. As background info remember: The colored part of the camera cable faces towards the movable part of the camera port on the Rpi, which can be locked.
1. Move the securing part for the camera slot up. Depending on the version of Rpi board it has different colors.
2. Gently slide the cable for the camera in the newly opened space between the securing part and the - in the usual cases - black part. The colored part of the cable has no electrical contacts and faces the movable part, which is only for mechanical purposes. The movable part of the camera slot just secures the camera to be set in there. The cable should slide with little resistance. Sometimes the movable part goes down automatically and makes the sliding hard, so keep it up. And in my case the cable does not completely slide in, so a little of the electric contacts of the cable is still visible.
3. Move the securing part for the camera slot down until it is at the bottom. The camera cable should be unremovable.
Now slide the SD card in the Raspberry Pi.
Connect the Raspberry Pi to the network by Ethernet cable.
Have the usual peripherals like a screen and a keyboard. The lite-version of Raspberry Pi OS does not include a desktop environment.
Have some power source for the Raspberry Pi and turn it on.

In my case the Raspberry Pi OS included already the necessary camera tools. My research gave me the info, that the camera tools uses a C++ library "libcamera" with a port to Raspberry Pi. The github for the port will be linked below [Link to References](#References)

To have a clean system after you set up your users and your favorite admin-tools or -editors run the following to have the newest updates: `sudo apt update && sudo apt upgrade`

## Running the camera
You only need two commands. The 1st command is to make a good picture. Type or copy the following command into a shell of your raspberry pi  
```
rpicam-still -o default.jpg
```
That command above is the basic command to check, if your camera works at all and Rpi writes you the picture called "default.jpg".

The following is the optimized command.
```
rpicam-still -q 100 --mode 4608:2592:12:P --autofocus-range full -o default.jpg
```  
If you tried making pictures with the same hardware setup, it should auto-detect your camera, find some configuration for your camera and set up many parameters for your camera.

A few options are in the command `rpicam-still`: 
* The **o**utput file is behind the `-o` option.
* The `-q 100` sets the output **q**uality for jpg to **100%**
* The mode is a possibility to enhance the picture a little and is specific for your camera. So the following is optimized for my hardware and the option `--mode 4608:2592:12:P` means: 1) width at **4608** pixels, 2) height at **2592** pixels, 3) color depth at **12** bits 4) with **p**acking
* The `--autofocus-range` just gives a hint which the command uses to find the best focus for the picture.

The 2nd command will run a stream. The following needs to be typed or copied into your raspberry pi shell    
```
rpicam-vid -t 30000 --inline -o - | cvlc stream:///dev/stdin --sout '#rtp{sdp=rtsp://:8000/}' :demux=h264
```

It creates a video stream (with no audio). The stream can be opened with VLC -> media -> networkstream  -> stream URL `rtsp://IP_or_hostname_of_raspberry_pi:8000` .   
The `-t` option is the timeout in milliseconds. So **30000** millisconds means the stream will run 30 seconds.  
With the result you can choose to simply view it, or save it with VLC in a video or do with the result something like streaming.

## Linux help
More options for the commands `rpicam-still` and `rpicam-vid` can be accessed with the following commands:
* `rpicam-still --help`
* `rpicam-vid --help`

General examples can be found online.

## Example pictures and videos
Example picture:  
![Simple Picture from Raspberry Pi camera](/examples/default_start.jpg)  
Example video [download / show example video](/examples/documents.mp4)


## References
[\[1\]: linux / Raspberry Pi OS on github.com](https://github.com/raspberrypi/linux)  
[\[2\]: libcamera on github.com/raspberrypi](https://github.com/raspberrypi/libcamera) 

