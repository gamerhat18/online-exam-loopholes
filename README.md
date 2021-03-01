# online-exam-loopholes
This repository compiles a couple of methods to cheat in an online exam, which involve a Personal Phone/Computer, preferably not inside a campus/college.

Note: I DO NOT condone cheating as a valid option to pass in exams, but my college blatantly told us to copy for our final exams, so I developed this repository guilt-free.

It is basically impractical for colleges to mitigate all the loopholes simultaneously. They can try, but they'll probably just conducting exams offline if this ends up making them really paranoid.


Feel free to contribute btw.

# A. Camera method

## Setting up a virtual camera in which a short video with your face in it will be looped through for as long as the exam lasts.

***This method is tested under Linux and Windows. Should work just fine with MacOS.*** 

### Requirements :

- **Ubuntu 18.04** or newer **(Linux Kernel 4.19+)**

  - install `v4l2loopback-dkms`
  - type `sudo modprobe v4l2loopback` right before launching OBS Studio.

- **MacOS Mojave** or newer

  - install homebrew and type `brew install gstreamer`

- **Windows 10 v1803** or newer

- **OBS Studio Latest** (preferably the latest Git version)


### 1. Disable your current primary camera 

  You don't strictly need to do this, but some exam softwares/websites might get a suspicion about the fact that you have (possibly) more than one camera, if you already have one inbuilt or you have an external camera plugged in. You can disconnect it or you can disable it from the terminal.
  
  To disable the camera, type `sudo modprobe uvcvideo` in your terminal


### 2. Open OBS Studio and add media sources

  You have to add a media source by clicking the '+' icon in the **Sources** section.
  Ideally, you'd want a short video of yours (about 1 minute) where you are sitting and staring at the screen naturally without much movement. The video will be looped through at the frame-rate that you select in the below section.
  
![](img/obs-1.png)


### 3. Change settings for optimal performance (OPTIONAL)

  If you experience bad performance or your computer gets slow, you can change some settings to make it run smoother. Just copy these settings as shown in these images and you won't suffer from bad performace anymore.

| Output Settings |
| --- |
| ![](img/obs-settings-1.png) |

| Video Settings |
| --- |
| ![](img/obs-settings-2.png) |

| Sound Settings |
| --- |
| ![](img/obs-4.3.png) |


### 4. Start the Virtual Camera

  Click on "**Start Virtual Camera**" and enter your password if asked for.
  Now the virtual camera is created by the name **OBS Virtual Camera** and you can check if it is working correctly by going to [https://webcammictest.com/](https://webcammictest.com/) and selecting "OBS Virtual Camera" on the website.
  
![](img/obs-test.gif)

# B. Remote Assistant method

## Asking a friend to send answers on your phone via text, while sharing your screen via Discord/Teamviewer so that your friend can see the questions. 

![](img/remote.png)


### Requirements :
 
- **Ubuntu 18.04** or newer **(Linux Kernel 4.19+)**
  
- **MacOS Mojave** or newer
 
- **Windows 10 v1803** or newer

- **Discord** or **TeamViewer** (both will work fine, but Teamviewer is usually more reliable)

- **A friend indeed** (smart enough to search exactly what you need)

### 1. Initialise

  Make sure you and your friend have a decent internet connection on both of your computers, and install Discord/TeamViewer on them.

### 2. Sharing is Caring
  
  Start Screen Sharing your screen to your friend's computer via Discord or let your friend connect to your PC via TeamViewer.

### 3. 



# C. Virtual Machine method

## Running the browser/software in a VM, and looking for answers in the host OS. 

Using a VM to run a software is probably the safest way to do it without compromising your security. 
You can use the OBS Studio method inside the VM as well, while you search for answers on the host machine or on your phone.

<p>
<details>
<summary>Setup KVM on Ubuntu based Distros (Almost same in other Distros as well)</summary>


You can make a Linux VM or a Windows VM. However, this guide focuses on making a Windows VM, as the process is a relatively easier for Linux VM because you don't have to download or install any drivers through an iso.

### Requirements:

  - **Ubuntu 18.04** or newer **(Kernel 4.19+)**
 
Note that any Linux distribution will work just fine as long as it is somewhat recent, and can install `virt-manager` and `qemu`.

This guide uses **Ubuntu 20.04** for the demo.


# Creating a Virtual Machine in KVM
This step-by-step guide will take you through setting up a CPU and memory efficient virtual machine to use OBS Studio and give exam at the same time.

## Install KVM
First up, you must install KVM and the Virtual Machine Manager. By installing `virt-manager`, you will get everything you need for your distribution:
```bash
sudo apt-get install -y virt-manager
```

## Download the Windows Professional and KVM VirtIO drivers
You will need the Windows 10 ISO. You will also need drivers for VirtIO to ensure the best performance and lowest overhead for your system. You can download these at the following links.

Windows 10 ISO: https://www.microsoft.com/en-us/software-download/windows10ISO

KVM VirtIO drivers (for all distros): https://fedorapeople.org/groups/virt/virtio-win/direct-downloads/stable-virtio/virtio-win.iso

## Create your virtual machine

Open `virt-manager` (Virtual Machines).

![](kvm/00.png)

Next, go to `Edit`->`Preferences`, and check `Enable XML editing`, then click the `Close` button.

![](img/01.png)

Now it is time to add a new VM by clicking the `+` button.

![](img/02.png)

Choose `Local install media` and click `Forward`.

![](img/03.png)

Now select the location of your Windows 10 ISO, and `Automatically detect` the installation.

![](img/04.png)

Set your memory and CPUs. We recommend `2` CPUs and `4096MB` for memory. We will be using a Memory Ballooning service, meaning 4096 is the maximum amount of memory the VM will ever use, but will not use this amount except when it is needed.

![](img/05.png)

Choose your virtual disk size, keep in mind this is the maximum size the disk will grow to, but it will not take up this space until it needs it.

![](img/06.png)

Next, name your machine `RDPWindows` so that WinApps can detect it, and choose to `Customize configuration before install`.

![](img/07.png)

After clicking `Finish`, ensure under CPU that `Copy host CPU configuration` is selected, and `Apply`.

**NOTE:** Sometimes this gets turned off after Windows is installed. You should check this option after install as well.

![](img/08.png)

Next, go to the `XML` tab, and edit the `<clock>` section to contain:
```xml
<clock offset='localtime'>
  <timer name='hpet' present='yes'/>
  <timer name='hypervclock' present='yes'/>
</clock>
```
Then `Apply`. This will drastically reduce idle CPU usage (from ~25% to ~3%).

![](img/09.png)

Next, under Memory, lower the `Current allocation` to the minimum memory the VM should use. We recommend `1024MB`.

![](img/10.png)

Under `Boot options`, check `Start virtual machine on host boot up`.

![](img/11.png)

For SATA Disk 1, set the `Disk bus` to `VirtIO`.

![](img/12.png)

For the NIC, set the `Device model` to `virtio`.

![](img/13.png)

Click the `Add Hardware` button in the lower right, and choose `Storage`. For `Device type`, select `CDROM device` and choose the VirtIO driver ISO you downloaded earlier. This will give the Windows 10 Installer access to drivers during the install process. Now click `Finish` to add the new CDROM device.

![](img/14.png)

You are now ready to click `Begin Installation`

![](img/15.png)

Now move on to installing the virtual machine.

## Install the virtual machine
From here out you will install Windows 10 Professional as you would on any other machine.

![](img/16.png)

Once you get to the point of selecting the location for installation, you will see there are no disks available. This is because we need to load the VirtIO driver. Select `Load driver`.

![](img/17.png)

The installer will then ask you to specify where the driver is located. Select the `E:\` drive or whichever drive the VirtIO driver ISO is located on.

![](img/18.png)

Choose the appropriate driver for the OS you have selected, which is most likely the `w10` driver for Windows 10.

![](img/19.png)

You will now see a disk you can select for the installation.

![](img/20.png)

Windows will begin to install, and you will likely need to reboot the VM a number times during this process.

![](img/21.png)

At some point, you will come to a network screen. This is because the VirtIO drivers for the network have not yet been loaded. Simply click `I don't have internet`.

![](img/22.png)

It will confirm your choice, so just choose `Continue with limited setup`.

![](img/23.png)

After you get into Windows and login with the user you created during the install. Open up `Explorer` and navigate the `E:\` drive or wherever the VirtIO driver ISO is mounted. Double click the `virt-win-gt-64.exe` file to launch the VirtIO driver installer.

![](img/24.png)

Leave everything as default and click `Next` through the installer. This will install device drivers as well as the Memory Ballooning service.

***Thanks to this [amazing project](https://github.com/Fmstrat/winapps/) for this guide :)***

</details>
</p>

<p>
<details>
<summary>Setup QEMU Virtualization on MacOS</summary>

### Requirements: 

- **MacOS Mojave** or newer


Follow these Links:

> **M1 and other Apple Silicon Macs:** 
>
> [QEMU_ON_M1](https://gist.github.com/citruz/9896cd6fb63288ac95f81716756cb9aa) by _Citruz_
>
>[MacRumors](https://forums.macrumors.com/threads/success-virtualize-windows-10-for-arm-on-m1-with-alexander-grafs-qemu-hypervisor-patch.2272354/) by _1958llakin_


> **Intel Mac/ Hackintosh:** [Parallels](https://www.parallels.com/)


 </details>
</p>


### ❌ Windows virtualisation sucks. Just use Linux if you wanna go this route.



