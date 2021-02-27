# online-exam-loopholes
This repository compiles a couple of methods to cheat in an online exam, which involves a Personal Phone/Computer, preferably not inside a campus/college. 
Note: I DO NOT condone cheating as a valid option to pass in exams, but my college usually gives out all the questions w/ answers in advance, so I can develop this repository guilt-free.

It is basically impractical for colleges to mitigate all the loopholes simultaneously, which is why making these open source won't really hurt the ones who wanna cheat.

Feel free to contribute.

# 1. Camera method

## Setting up a virtual camera in which a short video with your face in it will be looped through for as long as the exam lasts.

This method is tested under Linux and Windows. Should work just fine with MacOS. 

Requirements :

- Ubuntu 18.04 or newer (Kernel 4.19+)

  install `ffmpeg and v4l2loopback-dkms`

  type `sudo modprobe v4l2loopback` before launching OBS Studio.

- MacOS Mojave or newer
- Windows 10 v1803 or newer
- OBS Studio Latest (prefferably the Git version)

1. Open OBS Studio
