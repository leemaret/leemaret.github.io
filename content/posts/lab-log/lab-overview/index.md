---
title: "Lab Overview"
date: 2023-04-20T16:51:51-05:00
summary: "An overview of the tools I use in my work as a developer and industrial designer."
---

An overview of the tools I use in my work as a developer and industrial designer.

## computers

### primary local

I use an old (2016) tower server that was certified to run Centos 5, a Lenovo ThinkServer TS104. I've replaced all the drives with mostly SSDs and a single large HDD and maxed out the RAM to 32GB. My primary os is Manjaro on top of Arch, usually with Gnome for the desktop.

I love this computer because it is perfect for my needs. It will probably still be usable and pretty functional off the network in 5 years. It even has an RS-232 port, originally for printers, but also extremely useful for vintage and complex hardware nerds and people with plotters, like me. It can do virtualization, run Windows if necessary, and after I switched to mostly SSDs, runs quiet and fast enough.

Since I mostly develop for system environments and servers, I don't worry much about software since this guy doesn't typically need to interface with any non-OSS stuff. It works well as a host for Android devices and small electronics. 

### secondary local

It is an unfortunate fact of the work that one requires a laptop for when on site or in data center/noc. I use a 2017 Dell XPS developer edition laptop running Fedora + LXQT with an extremely well-tuned terminal emulator and Neovim/Tmux setup because most of my work on the laptop is at the command line. This is probably the best Linux laptop I've ever encountered and am going to try to hold onto it for as long as it keeps going. Unfortunately the RAM is soldered into the mobo on these bad lads (terrible design decision, stop that!) and can't be upgraded; one stick has already died in mine, so I'm living on the edge. 

In case you're wondering what makes a great laptop for this kind of work, it is onboard ethernet jack and SD card reader with a normal power supply input that is a standard shape used in other electronics (easy to replace the power charger if lost). Also it is very light. Also it is that in-between generation with USB-A 3.0 AND USB-C jacks. Supremely useful layout.

I don't use this laptop for software tools or IDE's. It is really just for network troubleshooting and implementation and working with text files. 

### local backup

One of those SATA toasters that allows you to plug in two additional drives, either SSD or HDD, and provides power so they can be run over USB3. I need some big SSD because two HDD running in it is loud as hell. I can walk around with it to connect it to another computer or the network hub depending on what I am doing.

I also carry an SSD drive around with me to sites and plotters etc in one of those sata-to-usb hard cases. Way more useful than SD cards because it is harder to lose and doesn't require an additional card reader. 

### hypervisor

HPE Proliant running Proxmox VE. I need to upgrade the drives to SSD but the HPE is a fancy lad and requires fancy drives that I am not ready to splash out for. I've had no problems with it but I think I will eventually unless I get the iLo dongle since I eventually plan to move it to a colo facility and without the dongle I'm not sure I'll be able to upgrade Proxmox itself without the iLo piece. 

Proxmox VE is great and I cannot recommend it enough if you have the hardware for a separate virtualization instance.

### network

pfSense running on a dedicated network appliance box I got from [mitxpc](https://mitxpc.com/collections/networking) sometime in 2015 which so far has been the best networking decision of my entire professional life. I also have a separate wify router for better coverage although I could mod my pfbox to do the same.

### windows

Unfortunately Windows is still a necessary evil for a lot of the work I do requiring special software or printers or some hardware. I use a 2020 Dell XPS with a lot of horsepower and the WSL. 

I don't do much if any development work on the Windows machine because I love my local machine so much but I can manage with using IntelliJ IDEA or VS Code connecting to a virtual machine on the hypervisor. Otherwise it is primarily used for running software like Solidworks and Affinity, using the plotter, and writing/testing documentation that requires testing against Windows.

I don't really use the WSL interface much except for simple reproductions (it is running on Docker). I don't really know much powershell though so it can be useful for a functional command line interface on Windows.

Periodically I am forced to use the Windows machine for debugging some weird hardware/device problem that Linux is having problems opening a connection to. 

### android

Samsung A4, a cheap 7" tablet. I do all the development for it on the host, the primary local computer. 

## software

### solidworks

I'm currently getting skilled up to be certified for Solidworks, which is a 2D drafting (CAD) program. I run this on the Windows machine and use a Wacom Intuos Pro tablet to draw.

### affinity

I prefer Affinity to Adobe since I don't use it for specialized work and have no opinion on things like brushes or anything serious power users might need. I mostly use it to process files between source applications and the plotter, and to layout documents with images that aren't going to be used on the web.

## special hardware

### plotter

A plotter is a type of specialty printing device that uses a pen or knife to draw lines on a roll of paper or other substrate. I use mine to print plans and patterns of materials to be cut. 

Plotters are extremely cool and useful if you are working with big things and materials that need to have precise placement, because they can accept CAD and flat vector files with edges and paths. They're what you need if you want to cut vinyl to a specific font and size to apply at an exact angle around a corner, for example, because they can be calibrated for more specific clearances than a paper printer. They can also accept a variety of substrates. I mostly use paper and the pen and cut my pattern pieces by hand.

### 3d printer

TBD. Probably a Halot Mage 8k. I only use resin/stereolithography 3d printers because of the type of work I do (moldmaking and prototypes) requires more detail and structural capabilities than the filament machines can provide.

### tablet

Wacom Intuos Pro. I prefer these to digitizers because I find the second screen distracting. You do need to train yourself to look at the screen and not your hand while drawing which I don't find to be a problem any more than typing. I have one of those half-gloves to keep your hand from interfering with the tablet but I don't usually need to use it. The Intuos has good ability to map the physical buttons to application hotkeys but I only really use it for zooming and panning in the document. 

### CNC mill

TBD. This is a bit further away from my area of focus but I am very interested in learning how to program the mills. 