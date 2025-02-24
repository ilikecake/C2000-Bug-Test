# C2000 Bug Info
Compiling a list of info I have found on the C2000 bug. Most notably how to determine if your CPU is affected.

According to the spec update from Intel, the newer C0 stepping is not affected by this bug. Identifying the stepping of your CPU is tricky. It looks like the only place where it is specifically called out is in the PCIe configuration space.

![image](https://github.com/user-attachments/assets/d5b49796-61d6-4277-b296-bba1aea7d0d6)
(Intel spec update doc, page 15)

I have one of these devices running a PFSense firewall. To read this value in BSD, I use pciconf:

`pciconf -r pci0:0:0:0 0x08`

This command can be put into the 'diagnostics->command prompt' section of the firewall.

![image](https://github.com/user-attachments/assets/43194218-d3aa-4e1b-a1a9-097e76377936)

The LSB (the number on the far right) should correspond to the 'PCI Rev ID' from table 8 above. In the above screenshot, it appears to indicate a B0 stepping CPU. 


## Other notes
From what I can tell, the older B0 stepping can still be okay if they apply a hardware fix. Basically, they solder some pull up resistors on some of the clock lines on the motherboard. Check the links below for more info and pictures.

Additionally, this bug will not cause a failure (they say) when the hardware is running. It will manifest as a failure to boot after a restart.

The spec document also talks about the CPUID and MSR (model specific registers). These can also be read from BSD, but I don't think they are actually relevant for determining the stepping of the CPU.

To read the CPUID in BSD:
`cpucontrol -v -i 0x01 /dev/cpuctl0`

To read MSR in BSD:
`cpucontrol -v -m 0x17 /dev/cpuctl0` (this is reading register at address 0x17)

## Links
For more info
* [servethehome forum post](https://forums.servethehome.com/index.php?threads/bug-in-intel-atom-c2000-series-processors.13173/)
* [EEVBlog forum](https://www.eevblog.com/forum/microcontrollers/intel-atom-c2000-failures/)
