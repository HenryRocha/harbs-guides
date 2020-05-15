# Personal guide to QEMU/KVM

## IOMMU
References:
[Arch Wiki](https://wiki.archlinux.org/index.php/PCI_passthrough_via_OVMF)

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
- Check the groups, [Setting up IOMMU](https://wiki.archlinux.org/index.php/PCI_passthrough_via_OVMF#Setting_up_IOMMU) covers how to do that.
