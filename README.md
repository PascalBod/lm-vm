# Overview

I create a distinct virtual machine for each of my embedded software projects. Thus I don't have to handle potential configuration conflicts between different development environments.

The virtual machine uses [*VirtualBox*](https://www.virtualbox.org/), and runs [*Linux Mint*](https://linuxmint.com/), a Linux distribution based on [*Ubuntu*](https://ubuntu.com/).

This short guide describes how I create the virtual machine (VM).

# Prerequisites

* Hardware: a 64-bit computer with enough memory so that the VM can be granted up to 16 GB (depending on the development environment that will be installed), with a few tens of GB available on the disk, and one free USB A (or C) port (for programming the microcontroller boards)
* Developer: 
  * Basic knowledge of Linux (knowing the most common commands...)
  * Basic knowledge of VirtualBox (knowing how to create a virtual machine...)

> [!NOTE]
> For many development environments, 4 GB or 8 GB of memory are enough. For [*STM32CubeIDE*](https://www.st.com/en/development-tools/stm32cubeide.html), 16 GB seems to be a minimum.

# VirtualBox installation

[Download the VirtualBox binary package for your platform and install it](https://www.virtualbox.org/wiki/Downloads). 

My host OS being Linux Mint, I use the Ubuntu version of VirtualBox.

At the time of writing, the latest version of VirtualBox is [7.1.8](https://www.virtualbox.org/wiki/Downloads).

# Creation of the VM

Download the [Linux Mint 21.3 Cinnamon ISO image](https://linuxmint.com/download_all.php).

Verify the integrity of the downloaded file.

Start VirtualBox, and create a new virtual machine, using the Linux Mint ISO file previously downloaded. Set configuration parameters to following values:
* **Name and operating system**:
  * **Name**: `LinuxMint` (or anything you prefer)
  * **ISO Image**: the Linux Mint ISO file
  * **Skip Unattended Installation**: check
* **Hardware**:
    * **Base Memory**: 4096 MB, 8192 MB, 16384 MB, or any other value, depending on the future use of the VM (see above comment about STM32CubeIDE)
    * **Processors**: depending on your host machine. I set it to **2**
* **Hard disk**:
    * Select **Create a Virtual Hard Disk Now**. I set size to 60 GB

Start the virtual machine.

When GNU GRUB menu is displayed, click the Enter key.

Wait for Linux Mint desktop to be displayed.

If your keyboard layout is not QWERTY, click on the main-menu icon ![icon](images/linuxMintMenuIcon.png) (in the lower left-hand corner). Move the mouse cursor over the **Preferences** category. Keeping it on the category icon, move it over the righ-hand side scrolling list. Scroll down and click the **Keyboard** item.

Select the **Layouts** tab. Add the layout for your keyboard, and remove the existing **English (US)** layout. Close the window.

Double click on the **Install Linux Mint** icon. Provide following information or selections, when required for:
* Language: English
* Keyboard layout: the one for your keyboard
* Install multimedia codecs
* Erase disk and install Linux Mint
* Name: Developer (or any other name)
* Computer's name: LMVM (or any other name)
* Username: developer (or any other username - the text below uses `developer`)
* Password: choose one

At the end of the installation, restart. When you get the message **Please remove the installation medium, then press ENTER:**, press the Enter key.

Log in as *developer* user. Close the **Welcome to Linux Mint** window.

In the VirtualBox menu, select **Devices > Insert Guest Additions CD image...**. A window asks you if you would like to run medium software. Click the **Run** button.

The password you are then asked for is the one you chose at installation time.

When execution is done, press the Return key (i.e. the Enter key).

If the VirtualBox window is too small, you can enlarge it.

Click on the system report icon, in the lower right-hand corner: ![icon](images/systemReportIcon.png). In the **System Reports** window which appears, click the **Ignore this report** button for the system restore utility. Close the system reports window.

Click the update manager icon, in the lower right-hand corner: ![icon](images/updateManagerIcon.png). In the welcome screen of the **Update Manager** window that appears, click the **OK** button. Click the **No** button of the **Do you want to switch to a local mirror?** banner (you'll be able to choose one later on). If you are told that a new version of the update manager is available, click the **Apply the Update** button. Again, the requested password is the one you chose above (I won't say it anymore :-) ). Then click the **Install Updates** button, and accept additional changes.

When the updates are done, close the update manager window, and shutdown the Linux Mint VM (**Linux Mint main menu**, shutdown icon: ![icon](images/shutdownIcon.png)).

With VirtualBox menu, set following settings parameters for the VM:
* **General > Advanced > Shared Clipboard**: **Bidirectional**
* **Display > Screen > Video Memory**: set it to 128 MB
* **USB**: select **USB 3.0**

Start the VM.

To grant access to the virtual serial link that will be used to program the microcontroller boards, open a terminal (main menu and the terminal, or the terminal icon on the lower left-hand corner), and add the user to the *dialout* group:

```shell
$ sudo adduser developer dialout
```

To interact with a board over the virtual serial link, install *miniterm*:
* From the main menu, click the Software Manager icon: ![icon](images/softwareManagerIcon.png)
* Search for `pyserial`
* Click the displayed **Python3-serial** box
* Click the **Install** button
* Provide your password
* Close the Software Manager window

If you want to share files with the host machine without file ownership trouble, add the user to the *vboxsf* group:

```shell
$ sudo adduser developer vboxsf
```

In the host machine, you also have to add the user to the *vboxusers* group, in order to let the virtual machine capture the various devices you'll connect to the host USB ports:

```shell
$ sudo adduser developer vboxusers
```

You can resize the VM window. If you want to make it full screen, press Right Control + F keys. Use the same keys to exit from the full-screen mode.

I don't like the default configuration of the terminal, so I modify it:
* Start a terminal
* Right-click in the terminal window
* **Edit > Preferences > General** and tick **Show menubar by default in new terminals**
* **Edit > Preferences > Unnamed** and set **columns** to 120
* Close the window

And I add the *System Monitor* applet to the Panel:
* Right-click on the Panel
* Select **Applets**
* Select the **Download** tab
* Select the **System Monitor** applet, and click its install button
* Go back to the **Manage** tab, select it and click the + button
* Close the applets window
* Right-click on the applet that was added to the Panel
* Select **Configure...**
* On the **Common** tab, enable **Draw border**
* Disable swap and load displays
* Close the window

To share files between the VM and the host computer, use VirtualBox menu:
* **Devices > Shared Folders > Shared Folders Settings...**
* Select **Machine Folders**
* Click the add button
* For **Folder Path**, select the path to the host directory that will be used to exchange files
* Tick **Auto-mount** and **Make Permanent**
* Click the **OK** button

For the file manager, I prefer to get a list of files instead of icons. So:
* **Edit > Preferences...**
* In the **Views** page, select **List View** for **View new folders using::**
* Close the window

# Update

When all packages are up-to-date, the update manager icon (see above) does not display the orange disk. If this disk appears, it means it's time to perform an update. To do this, click on the icon (see above).

# Keeping a copy of the virtual machine

If you plan to reuse the newly created Linux Mint virtual machine, shutdown it. Then, copy the directory containing the virtual machine files to a safe place.








