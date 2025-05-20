### Note: 
- I'm currently stuck in windows hell so you'll have to do with raw markdown, sorry :(
- Trust me its worse for me :|

# GRUB
- I installed grub but dont have a desktop manager (or smt) so no gui for me. here's how i load the kernel
- plz dont hack me im just a boy (*^.^*)

## Load Linux kernel in GRUB Shell
- start by setting the root, in this case the partition where the kernel is stored, it'll probably in /dev/sda1
> set root=(hd0,gpt1)
if its in /dev/sdaY then:
> set root=(hd0,gptY)
IDK how to do it for MBR or /dev/sdb so read the manual ig

- Next to load the kernel:
	- we specify the "kernel image" (or smt) 
	- and the dst location of the root partition
> Linux /vmlinux root=/dev/sda6

your root parition number depends on ur setup :) 

- Finally we need load the initial ram
This can be a bit confusing since older systems have it under initrd.img while new systems have it under
initramfs-linux.img, either which:
> initrd initramfs-linux.img

- Then just boot
> boot

[Good refernece](https://www.linuxfoundation.org/blog/blog/classic-sysadmin-how-to-rescue-a-non-booting-grub-2-on-linux)
