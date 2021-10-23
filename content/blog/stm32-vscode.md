---
title: "STM32 environment on Visual Studio Code"
date: 2019-11-02
draft: false
categories: ["blog"]
tags: ["dev", "micro", "stm32"]
---

> Originally published on [GitHub Gists](https://gist.github.com/Bonnee/393c4be25d2e8620d9ec406073940d3a)

This page will help you setup Visual Studio Code for programming and debugging STM32 boards using STM32CubeMX projects.

I tested this guide under Arch Linux and Ubuntu 18.04. If you get it to work under other setups please let me know so that I can update the steps with more info.

## Warning
If you were using STM32CubeIDE or SystemWorkbench before, you need to convert your projects in order for them to work. The conversion procedure is fully reversible.<br/>
Up until the time of writing this guide, it is not possible to use STM32CubeIDE and Visual Studio Code on the same project unless some configuration changes are made on CubeIDE. 
Besides that, it is recommended that every person that works on a project runs the same working environment.

# Installation

## Steps
1. [STM32CubeMX](#1-stm32cubemx)
2. [GNU ARM Embedded Toolchain](#2-gnu-arm-embedded-toolchain)
3. [Make](#3-make)
4. [OpenOCD](#4-openocd)
5. [Visual Studio Code](#5-visual-studio-code)
    1. [C/C++ Extension](#51-cc-extension)
    2. [stm32-for-vscode Extension](#52-stm32-for-vscode-extension)

## 1. STM32CubeMX
If you already have CubeMX installed, you can skip this step.
#### Arch Linux
Install `stm32cubemx` from the [AUR](https://wiki.archlinux.org/index.php/Arch_User_Repository). If you don't have access to the AUR or you just don't want to use it, follow the step below.
#### Inferior operating systems (Windows, Ubuntu, Temple OS...)
Download the .zip from [here](https://www.st.com/en/development-tools/stm32cubemx.html) (you have to sign up to ST's website) and install the right version for your OS.

---

## 2. GNU ARM Embedded Toolchain
These are the tools needed to compile and debug the code.
### Arch Linux
Install `arm-none-eabi-gcc` `arm-none-eabi-gdb` `arm-none-eabi-newlib`.
### Debian/Ubuntu
Install `gcc-arm-none-eabi` `gdb-multiarch` `libnewlib-arm-none-eabi`.
### Windows
Install the toolchain from [ARM's website](https://developer.arm.com/tools-and-software/open-source-software/developer-tools/gnu-toolchain/gnu-rm/downloads).

---

## 3. Make
### Linux
Install `make` from your package manager.
### Windows
Install make from this [this](http://gnuwin32.sourceforge.net/packages/make.htm) page (updated in 2006 though...). 
It should be possible to get the current version of make through [WSL](https://en.wikipedia.org/wiki/Windows_Subsystem_for_Linux), but I don't have experience with it.

---

## 4. OpenOCD
Open On-Chip Debugger is the interface between your computer and the programmer. It will take care of uploading the compiled software to the STM32 and during debug it will open the connection between the computer and the STM32.<br/>
### Linux
Install `openocd` from your package manager.

**Warning**: for some newer microcontrollers and programmers you might need to build OpenOCD from sources, especially if your distro doesn't ship the latest version.
### Windows
I received feedback about [xPack OpenOCD](https://xpack.github.io/openocd/install/) working fine with this setup.

---

## 5. Visual Studio Code
The next steps aren't really necessary to get the thing working. You could just use a shell and your favorite editor and you would have (almost) all the functionality of the complete setup. Visual Studio Code is just a pretty front-end.
### Linux
[The official linux page](https://code.visualstudio.com/docs/setup/linux) contains the instructions to install the latest version of VSCode on many distributions.
### Windows
Download and install the program from the [official website](https://code.visualstudio.com/).

---

### 5.1. C/C++ Extension
This extension will take care of intellisense, syntax highlighting and more.<br/>
Install [this](https://marketplace.visualstudio.com/items?itemName=ms-vscode.cpptools) extension in Visual Studio Code.
### 5.2. stm32-for-vscode Extension
Same as above, but with [this](https://marketplace.visualstudio.com/items?itemName=bmd.stm32-for-vscode) extension. 

# Configuration
## 1. STM32CubeMX
By default CubeMX generates projects in a format called EWARM. Unfortunately, EWARM is currently not supported by the VSCode extension, but the more generic Makefile structure is. In order to configure CubeMX to support VSCode you have to navigate to `Project Manager->Project->Toolchain/IDE` and set it to Makefile.

If you don't want your STM32Cube library path being spammed all over the makefile (if you need to share the project with someone else for example) you can tell CubeMX to copy the necessary files to the project's directory and link the makefile to them. The option is located in `Project Manger->Code Generator->STM32Cube MCU packages and embedded software packs`.

After CubeMX setup you can click on `GENERATE CODE`; the new Makefile project structure should get created.

---

## 2. Visual Studio
Using the `Open folder` menu in Visual Studio Code, navigate to your project's root folder and open it.
Now open the command palette (`Ctrl+Shift+P` or `F1`) and run `Build clean STM32 project`. A terminal should appear and you will see gcc (hopefully) building your project.

---

# Usage
## Compiling & Flashing
After you call `Build STM32 Project` for the first time, stm32-for-vscode will create two custom tasks (that can be accessed by typing `Ctrl+Shift+B`) to build your code and to flash it into the STM32 board.

## Debugging
To debug the code just press `F5` inside VSCode and the debugger will start automatically. Remember to stop the debugger before flashing new code.

# Troubleshooting
### `Error: libusb_open() failed with LIBUSB_ERROR_ACCESS` during upload
This happnes because you don't have permission to open the serial device. In order to fix this you need to add your user to the group that owns the serial interface. To get the serial interface name you can run `dmesg | grep tty` after plugging the STM32 in. Read the last line, you should see something like:
```
cdc_acm 2-2:1.2: ttyACM0: USB ACM device
```
In this example `ttyACM0` is the device name.<br/>
Now that you know the name you can get the owner by doing `ls -l /dev/ttyACM0`. The output looks like this:
```
crw-rw----  1 user   group   4, 166 set 11 08:45 /dev/ttyACM0
```
`group` indicates the group name.

Now you just need to add your user to the group by doing `sudo usermod -aG group $USER`, substituting `group` with the group name you have discovered above.

You are done. Log out of your account and log back in to apply the change.
