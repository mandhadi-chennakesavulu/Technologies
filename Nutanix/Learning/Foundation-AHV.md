**Foundations has 3 Steps**
```
ESXI Based + AHV Based + 
```
**AHV Cluster Istallations Steps**
```
Step 1 : Check the IPMI ip is set or not
Bios -> ipv4 enable -> assaign static ip

Create a VM Outside the Server orelse use pc
if you are using your pc - Install a sandbox
than create a vm.

Step 2: Download the Foundation image from Nutanix portal:
https://portal.nutanix.com/page/downloads?product=foundation

![alt text](image.png)

untar the file

Deploy a Virtual Box for deploying Foundation VM
https://download.virtualbox.org/virtualbox/7.1.6/VirtualBox-7.1.6-167084-Win.exe (This is
for windows)

After Untar the folder folder use the ovf file use it and import in virtual box

```