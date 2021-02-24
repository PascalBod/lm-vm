#### Table of contents

* [Overview](#overview)
* [Prerequisites](#prerequisites)
* [VirtualBox installation](#virtualboxInstallation)
* [Creation of the VM](#creationOfTheVm)
* [Exporting an appliance](#exportingAnAppliance)

<a name="overview"></a>
# Overview

I create a distinct virtual machine for each of my embedded software projects. Thus I don't have to handle potential configuration conflicts.

The virtual machine uses VirtualBox, and runs Linux Mint 20.1.

This short guide describes how I create it.

<a name="prerequisites"></a>
# Prerequisites

* Hardware: a 64-bit computer with enough memory so that the VM can be granted 16 GB, with a few tens of GB available on the disk, and one free USB A port (for programming the microcontroller board)
* Developer: 
  * basic knowledge of Linux (knowing the most common commands...)
  * basic knowledge of VirtualBox (knowing how to create a virtual machine...)

<a name="virtualboxInstallation"></a>
# VirtualBox installation

[Download the VirtualBox binary package for your platform and install it](https://www.virtualbox.org/wiki/Downloads). Version at time of writing is 6.1.18.

Install the Extension Pack: it provides support for USB 2.0 and 3.0 devices.

<a name="creationOfTheVm"></a>
# Creation of the VM

[Download Linux Mint 20.1, MATE edition](https://linuxmint.com/download.php).

[Verify the integrity of the downloaded file](https://linuxmint.com/verify.php).

Start VirtualBox, and create a new virtual machine, using the Linux Mint ISO file previously downloaded. Set memory size to 16 GB and file size to 30 GB.

Wait for Linux desktop to be displayed.

If your keyboard layout is not QWERTY, click on the main-menu icon (the small icon displaying Linux Mint logo, in the lower left-hand corner), select **Control Center**, click on **Keyboard** icon, select **Layouts** tab, add the layout for your keyboard, and remove the existing **English (US)** layout. Close the Control Center windows.

Double click on the **Install Linux Mint** icon. Following information or selections can be provided, when required for:
* Language: English
* Keyboard layout: the one for your keyboard
* Install multimedia codecs
* Erase disk and install Linux Mint
* Name: Developer
* Computer's name: LMVM
* Username: developer
* Password: choose one

At the end of the installation, restart. When you get the message **Please remove the installation medium, then press ENTER:**, click on Enter key.

Log in as *developer* user. Close the **Welcome to Linux Mint** window.

In the VirtualBox menu, select **Devices > Insert Guest Additions CD image...**. Double-click on the CD icon that appeared on the desktop. In the file explorer window, right-click on the **VBoxLinuxAdditions.run** file and select **Run as Administrator**. The password you are then asked for is the one you chose at installation time.

Once the guest additions are installed, you can right-click on the CD icon and select **Eject**.

Click on the system report icon, in the lower right-hand corner: ![icon](images/systemReportIcon.png). In the **System Reports** window that appears, select **System reports > Install language packs**, click on **Install the Language Packs** button, and accept the installation. The requested password is the one you chose at installation time.

For the system restore utility, click on **Ignore this report**. Close the window.

Click on the update manager icon, in the lower right-hand corner: ![icon](images/updateManagerIcon.png). In the welcome screen of the **Update Manager** window that appears, click on **OK** button. Click on the **No** button of the **Do you want to switch to a local mirror?** banner (you'll be able to choose one later on). If you are told that a new version of the update manager is available, click on **Apply the Update** button. Again, the requested password is the one you chose above (I won't say it anymore :-) ). Click on **Install Updates** button, and accept additional changes.

To grant access to the virtual serial link that will be used to program the microcontroller board, add the user to the dialout group:

```shell
$ sudo adduser developer dialout
```

Reboot: main menu and **Quit > Restart**.

You can resize the VirtualBox window: the Linux Mint desktop will resize accordingly.

<a name="exportingTheAppliance"></a>
# Exporting the appliance

Export a copy of the virtual machine as it is now, so that next time you want to set up an environment based on this type of VM, you don't have to go through all the steps above:
* shutdown Linux
* in VirtualBox Manager, select **File > Export Appliance...**
* select the virtual machine
* click on **Next >** button
* for **Format**, I use **Open Virtualization Format 2.0**
* click on **Next >** button
* click on **Export** button

Save the resulting `.ova` file in a safe place. Next time you need to create a new VM, import it from VirtualBox Manager with **File > Import Appliance...**.