# VM Preparation
We will create 2 VMs by importing the OVA file provided in the lab

## Prerequisites Software
### Virtualbox
1. Install Oracle Virtualbox by downloading the latest Virtualbox from here: https://www.virtualbox.org/wiki/Downloads
2. Install the Virtualbox Extenstion Pack

### Putty (for Windows)
1. Install putty from here: https://www.chiark.greenend.org.uk/~sgtatham/putty/latest.html

## Create VMs
1. Once Virtualbox is installed, import the Virtualbox OVA file provided (202002-MySQLWorkshop8019.ova)

![Import OVA](img/OVA2.png)

2. Change the following:
Name: MySQLnode1
Mac Address Policy: Generate new MAC addresses for all network adapters

![Import OVA4](img/OVA4.png)

3. Start importing by clicking on "Import"

![Import OVA5](img/OVA5.png)

4. Once the import completes, you should have the VM imported in Virtualbox in "Powered Off" mode

5. Repeat the same "Import" process to create the second VM and renamed to "MySQLnode2"

