# C2000-Bug-Test
Compiling a list of info I have found on the C2000 bug. Most notably how to determine if your CPU is affected.

According to the spec update from Intel, the newer C0 stepping is not affected by this bug. Identifying the stepping of your CPU is tricky. It looks like the only place where it is specifically called out is in the PCIe configuration space.

![image](https://github.com/user-attachments/assets/d5b49796-61d6-4277-b296-bba1aea7d0d6)
(Intel spec update doc, page 15)

I have one of these devices running a PFSense firewall. To read this value in BSD, I use pciconf:

`pciconf -r pci0:0:0:0 0x08`

This command can be put into the 'diagnostics->command prompt' section of the firewall.

![image](https://github.com/user-attachments/assets/43194218-d3aa-4e1b-a1a9-097e76377936)

The LSB (the number on the far right) should correspond to the 'PCI Rev ID' from table 8 above. In the above screenshot, it appears to indicate a B0 stepping CPU. 
