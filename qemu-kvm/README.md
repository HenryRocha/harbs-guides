# Personal guide to QEMU/KVM

References:
- [Arch Wiki](https://wiki.archlinux.org/index.php/PCI_passthrough_via_OVMF)
- [Optimus laptop dGPU passthrough](https://gist.github.com/Misairu-G/616f7b2756c488148b7309addc940b28)
- [SomeOrdinaryGamers's guide to QEME/KVM/Passthrough](https://www.youtube.com/watch?v=h7SG7ccjn-g)
- [Newbie Guide Migrating to Linux](https://www.reddit.com/r/linux_gaming/comments/edaq0s/guide_migrating_to_linux_in_2020/)
- [LEVEL1TECHS forum posts on passthrough](https://forum.level1techs.com/tags/passthrough)

## IOMMU

Steps:
- Read the [Prerequisites](https://wiki.archlinux.org/index.php/PCI_passthrough_via_OVMF#Prerequisites) on the ArchWiki.
- Read [Setting up IOMMU](https://wiki.archlinux.org/index.php/PCI_passthrough_via_OVMF#Setting_up_IOMMU) from the ArchWiki.
- To add the kernel parameters do:
- ```$ sudo nvim /etc/default/grub```
- Add ```intel_iommu=on``` and ```iommu=pt``` to ```GRUB_CMDLINE_LINUX_DEFAULT```.
- To regenerate Grub's configs do:
- ```$ sudo grub-mkconfig -o /boot/grub/grub.cfg```
- Reboot
- Check if IOMMU was enabled correctly, do:
- ```$ sudo dmesg | grep -i -e DMAR -e IOMMU```
- The output should contain:
- ```DMAR: IOMMU enabled```
- If it does, keep going. If it doesn't, you did something wrong.
- Check the groups, [Ensuring that the groups are valid](https://wiki.archlinux.org/index.php/PCI_passthrough_via_OVMF#Ensuring_that_the_groups_are_valid) covers how to do that.

## Isolating the dGPU

Steps:
- Read [Isolating the GPU](https://wiki.archlinux.org/index.php/PCI_passthrough_via_OVMF#Isolating_the_GPU).
- Watch [SomeOrdinaryGamers's guide to QEME/KVM/Passthrough](https://youtu.be/h7SG7ccjn-g?t=904)
- Create and edit the file ```/etc/modprobe.d/vfio.conf``` with:
- ```$ sudo nvim /etc/modprobe.d/vfio.conf```
- The file should be empty at first and you will need to fill it out, using the IOMMU groups you got from the last step on [IOMMU](#iommu).
- Add ```options vfio-pci ids=XXXX``` to the file, using the correct ID's for your group. Should look something like this:
- ```options vfio-pci ids=10de:13c2,10de:0fbb```
- Edit ```/etc/default/grub```, using:
- ```$ sudo nvim /etc/default/grub```
- Add ```igfx_off kvm.ignore_msrs=1``` to ```GRUB_CMDLINE_LINUX_DEFAULT```, then update Grub using:
- ```$ sudo grub-mkconfig -o /boot/grub/grub.cfg```
- Edit ```/etc/mkinitcpio.conf```, using:
- ```$ sudo nvim /etc/mkinitcpio.conf```
- Add ```vfio_pci vfio vfio_iommu_type1 vfio_virqfd``` to ```MODULES```.
- Make sure that ```HOOKS``` contains ```modconf```.
- Regenerate the initramfs using(Change ```linux54``` to the kernel you're using):
- ```$ sudo mkinitcpio -p linux54```
- Reboot
- Check if the drivers in use for the dGPU are ```vfio-pci`` using:
- ```lspci -nnk``
