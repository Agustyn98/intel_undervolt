# Undervolt intel processors on Linux, easier than on Windows

In 2018, [This guide](https://github.com/mihic/linux-intel-undervolt) was made on how to undervolt your processor, while it is a great foundation, it doesn't provide all the information needed to get undervolting working. This short guide will show you how to succesfully lower voltages on your CPU and iGPU on linux.


## Guide:
First of all, it is still possible to undervolt intel's proccesors regardless of your BIOS version and microcode, unlike what the [Arch wiki](https://wiki.archlinux.org/title/Undervolting_CPU) says.

These are the components of your proccesor you can mess with:

1. CPU Cores
2. Cache
3. iGPU
4. iGPU unslice
5. Analog I/O


### Undervolting the CPU Cores
-- Cores and cache:
To undervolt your CPU cores, you need to also undervolt your cache at the same time. The more you lower cache offset, the more you can undervolt your cores.
For example, on my machine, with a -50mV offset on cache, I can undervolt the cores up to -90mV, setting values lower than -90 will have no further effect.

### Undervolting the integrated graphics
To undervolt your integrated graphics, you neeed to undervolt both iGPU and iGPU unslice values equally, otherwise it won't work. What the original guide refers to as System Agent, is actually what Throttlestop refers as iGPU Unslice


### Analog I/O and Digital I/O
Undervolting the Analog I/O works just fine, while the Digital I/O mentioned on the original guide never worked. It was never recommended to modify any of these values anyway.


### Overclocking
Just like you can add negative offsets, you can add positive ones. Overclocking is also a posibility.


## My utility 

Based on this information, you can use any tool mentioned [here](original-github.com), just remember that older utilities call the iGPU unslice as System Agent

I've also provided my own script which is less than 100 LOC and written in Python.



## Script installation 
Clone this repository and move undervolt to your binaries folder
Modify the sample config file undervolt.conf and copy it to /etc/undervolf.conf

$ git clone http...
# cp undervolt /usr/local/bin
# cp undervolt.conf /etc/

### Usage

undervolt apply
Undervolt your processor according to the offset values indicated in /etc/uv-intel.conf

undervolt read
Get the offsets currently set on your computer

### Persist changes
The systemd unit uv.service will re-apply your settings after boot, suspend and hibernation.

To activate it:
Move uv.service to /etc/systemd/system
Enable the service

# cp uv.service /etc/systemd/system
# systemctl enable uv.service

### Config file
Follow the sample config format. Positive values in this file translate to negative offsets in the script