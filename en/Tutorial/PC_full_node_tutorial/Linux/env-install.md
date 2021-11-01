# Build a Linux environment


## Make U disk boot disk (installation disk)

1. Use the UltraISO tool: File -> Open and select the ISO file (centos iso installation file)
   ![usb1](./images/usb1.png)

2. Click the menu in turn: Start -> Write hard disk image

   ![usb2](./images/usb2.png)
   

   ![usb3](./images/usb3.png)
   
3. Select the corresponding U disk and click Write to complete the U disk installation disk.
   

## Install Linux system

Insert the prepared U disk into the computer where the system is to be installed, and set the BIOS startup item to boot from the U disk. After starting, you enter the installation interface.

1. After booting from the U disk, the following interface will appear:

   - Install CentOS 7 Install CentOS 7
   - Test this media & install CentOS 7 test the installation file and install CentOS 7
   - Troubleshooting

   Select the first item, install CentOS 7 directly, press Enter, and enter the following interface
   
2. Select the first item, install CentOS 7 directly, press Enter, and enter the following interface

   ![installpage1](./images/installpage1.png)
   

3. Select the language used during the installation process, here select English, keyboard select American keyboard. Click Continue
   ![installpage2](./images/installpage2.png)

4. First set the time
   ![installpage4](./images/installpage3.png)

5. Select Shanghai as the time zone and check whether the time is correct. Then click Done
   ![installpage4](./images/installpage4.png)

6. Select the software to be installed
   ![installpage5](./images/installpage5.png)

7. Select Minimal Install, then click Done
   ![installpage3](./images/installpage6.png)

8. Select the installation location, where you can partition the disk.
   ![installpage7](./images/installpage7.png)

9. Select i wil configure partitioning (I will configure partitioning), and then click done
   ![installpage8](./images/installpage8.png)

10. As shown in the figure below, click the plus sign, select /boot, and divide the boot partition into 400M. Finally click Add
    ![installpage9](./images/installpage9.png)

11. Then use the same method to allocate space to the other three areas and click Done
    ![installpage10](./images/installpage10.png)

12. Then the summary information will pop up, click AcceptChanges (accept changes)
    ![installpage11](./images/installpage11.png)

::: tip
<pre>

Suggestions on how to set the partition size:
Generally speaking, there are at least two mount points in a linux system, namely / (root directory) and swap (swap partition), among which / is required;
Suggest several directories and sizes for general mounting:
   /swap directory 32G # If the memory is less than 4G, it is twice the memory. If the memory is greater than 4G, the size of the memory can be the same.
   /boot directory 400M # The static link file of boot loader, which stores the programs related to Linux startup
   / (Root) directory # all remaining
</pre>
:::

13. Set the host name and network card information
    ![installpage12](./images/installpage12.png)

14. First, turn on the network card, and then check whether the IP address can be obtained, and then click Done after changing the host name.
    ![installpage13](./images/installpage13.png)

15. Finally select Begin Installation
    ![installpage14](./images/installpage14.png)

16. Set the root password
    ![installpage15](./images/installpage15.png)

17. Click Done after setting the root password
    ![installpage16](./images/installpage16.png)

18. Wait for the system to be installed and restart the system
    ![installpage17](./images/installpage17.png)