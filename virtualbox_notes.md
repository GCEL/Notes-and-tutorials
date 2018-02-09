# Virtual Box Troubleshooting

These notes are to supplement the standard VirtualBox installation guides found on the internet.

## Virtualisation issues

Virtualisation tools (such as VirtualBox) allow you to use your computer/laptop to emulate another computer, letting you run different operating systems without having to have lots of separate physical computers or set up dual-boot systems. The virtualization technology can simulate different types of CPU such as 32-bit and 64-bit CPUs, but it requires your computer hardware to be configured properly.

Virtualisation needs both software (i.e. VirtualBox) and hardware support (i.e. the right settings on you computer's hardware to be activated.)

### Operating System 'bitness' (64 bit vs 32 bit)

Most modern operating systems (Linux, Windows, MacOS etc) are designed to run on what is called '64-bit' computer architecture. The number refers to the amount of data your computer's processor can process in one go. So a 64-bit processor/CPU will be able to read and 'process' 64 bits of data at a time. Pretty much every computer made in the last decade will be '64 bit'. Older computers were 32 bit or even 16 bit. VirtualBox can emulate 32-bit or 64-bit computers, but special support is required for 64-bit.

### How do I check if 64-bit virtualisation is supported?

The easiest way if you are reading this, is to try and run or install a 64-bit operating system on VirtualBox. If you get an error when trying to start the virtual machine like: "This virtual machine is configured for 64-bit guest operating systems. However, 64-bit operation is not possible.", then this is your answer.

In Windows, you can also open Task Manager and go to the 'Performance' settings. There will be some text here that says "Virtualization: Enabled" if there is support for 64 bit virtualisation.

In Linux, you can either type:

```
lscpu | grep "CPU op-mode"
```

and it will hopefully say '64-bit'...or you can do:

```
cat /proc/cpuinfo | grep 'vmx\|svm'
```

and there will be some output to screen. (If there is no output from this command, then it is not supported.

### How do enable 64-bit virtualization support in the BIOS?

If you have a 64-bit CPU, you will probably be running, or want to run, a 64-bit version operating system on it. (e.g. Ubuntu 64-bit.)

To use modern (64-bit) operating systems on virtualbox, you must have something called 'Virtualization Supoprt' switched on in your computer's BIOS. BIOS is a set of low-level hardware settings that are activated a few seconds after your computer switches on. (If you are curious, BIOS stands for "Basic Input/Output System".)

You can normally access the BIOS settings by quickly pressing 'ESC', 'DEL' or 'F1' (or some other function key) just after pressing the power button on your computer. It is difficult to give specific advice here, as this will vary between computer manufacturer, but once you have accessed the BIOS settings menu on your laptop, you will be looking for a setting called 'Virtualization Support' or 'Enable Virtualization'. Make sure this is enabled, then save the BIOS settings and restart your computer. Sometimes it can be under the 'Security' tab in the BIOS settings, other times it will be under a different tab. Sometimes it is also called "VT-x", "AMD-V", or "Intel VT-x"...

### What if I can't enable Virtualization Support?

Sometimes, the BIOS settings can be password protected or encrypted by the organisation that provides your computer or laptop (if it is a company- or university provided computer). You can ask your local IT support if they will enable hardware virtualisation for you, but they may not allow it. If this is the case, you can usually still use VirtualBox, but you will be limited to using a 32-bit version of the operating system. Most versions of the Linux operating system are still available in a 32-bit version for this reason. You will need to look for the '32-bit' version of Linux when you are downloading the installation image if this applies to you.

## Network issues

The easiest way to give your virtual machine an internet connection is to share the internet connection of your real, physical machine with it (The 'host' machine or 'host operating system'). This is usually automatic if you have just installed VirtualBox and used the default settings, but sometimes you will need to change some settings if you are having problems.

1. Try checking the Network Settings in your virtual machine. With VirtualBox open, right-click on the name of the virtual machine and click 'Settings'. Go to the 'Network' settings group, and look for a drop down menu called 'Attached to'. The most commonly used setting is 'NAT', which just gives your virtual machine access to whatever internet connection the host machine happens to be using at the time. If it is not set to NAT, try changing this and seeing if this fixes your network connection problems.

2. Make sure the network adaptor is enabled *within the Virtual Machine window*. When you have started up the Virtual Machine you want to use, look at the edge of the window in the bottom right. There are some little blue icons and one of these is the network adaptor. Make sure it is active, it should be blue or lit up in some way instead of being greyed-out. Right-click on this and click 'enable network adaptor' if it is not already activated. 

3. Make sure your *host machine* is actually connected to the internet. Before starting any virtual machines, check that your physical, real computer is connected to the internet and you can browse to a website for example.  
