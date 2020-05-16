
# Personal guide to QEMU/KVM

References:
- [Arch Wiki's PCI passthrough via OVMF](https://wiki.archlinux.org/index.php/PCI_passthrough_via_OVMF)
- [Optimus laptop dGPU passthrough](https://gist.github.com/Misairu-G/616f7b2756c488148b7309addc940b28)
- [SomeOrdinaryGamers's guide to QEME/KVM/Passthrough](https://www.youtube.com/watch?v=h7SG7ccjn-g)
- [Newbie Guide Migrating to Linux](https://www.reddit.com/r/linux_gaming/comments/edaq0s/guide_migrating_to_linux_in_2020/)
- [LEVEL1TECHS forum posts on passthrough](https://forum.level1techs.com/tags/passthrough)
- [Passthrough Helper Manjaro](https://github.com/pavolelsig/passthrough_helper_manjaro)
- [NVIDIA RTX 2060 Mobile successful passthrough](https://old.reddit.com/r/VFIO/comments/ebo2uk/nvidia_geforce_rtx_2060_mobile_success_qemu_ovmf/)
- [GPU Passthrough on trillian](https://gist.github.com/kaapstorm/4e51ff5f500eb7e93820d2e630dfdbbc)
- [Windows VM GPU Passthrough Ubuntu](https://mathiashueber.com/windows-virtual-machine-gpu-passthrough-ubuntu/)

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
- Regenerate the initramfs using (Change ```linux54``` to the kernel you're using):
- ```$ sudo mkinitcpio -p linux54```
- Reboot
- Check if the drivers in use for the dGPU are ```vfio-pci``` using:
- ```$ lspci -nnk```

## Creating and configuring the Virtual Machine

Steps:
- Install the required packages:
- ```$ sudo pacman -S qemu virt-manager ovmf dnsmasq ebtables iptables```
- Enable and start all the services:
- ```$ sudo systemctl enable libvirtd.service```
- ```$ sudo systemctl start libvirtd.service```
- ```$ sudo systemctl enable virtlogd.socket```
- ```$ sudo systemctl start virtlogd.socket```
- ```$ sudo virsh net-autostart default```
- Download the [Windows 10 ISO](https://www.microsoft.com/en-us/software-download/windows10ISO)
- Download the [VIRTIO drivers](https://docs.fedoraproject.org/en-US/quick-docs/creating-windows-virtual-machines-using-virtio-drivers/#virtio-win-direct-downloads)
- Open `virt-manager` and create a new virtual machine:
  - `Local install media`
  - `Browse`
  - Create new pool where you want your VM files stored at. Refer to [this guide](https://www.tecmint.com/manage-kvm-storage-volumes-and-pools/).
  - Copy the Windows 10 ISO and VIRTIO drivers you download to the pool you just created.
  - Select the ISO from the browse menu.
  - Choose the amount of RAM and cores you wish to give your VM.
  - Choose the size of your VM. If creating custom, do it in the pool you created before.
  - Give the VM a name and check `Customize configuration before install`.
  - In the settings of the machine, select `Overview` and set:
	  - `Chipset` to `Q35`
	  - `FIRMWARE` to `UEFI x86_64 .../OVMF_CODE.fd`
  - Now select `CPUs` and manually set the CPU topology according to your CPU.
  - Go to  `... Disk 1 > Advanced options` and set:
	  - `Disk bus` to `VirtIO`
  - Go to `... CDROM 1` and set `Disk bus` to `SATA`.
  - Click on `+ Add Hardware > Storage`:
	  - Check `Select or create custom storage`
	  - Click on `Manage`, go to the pool you created and select the VIRTIO drivers
	  - Make sure the `Device type` is set to `CDROM`
	  - And the `Bus type` is set to `VirtIO`
   - Click on `+ Add Hardware > PCI Host Device` and select everything that was in the same IOMMU group your dGPU was on. 
   - Click on `Begin installation`.

## Installing Windows 10

Steps:
- Install it normally until you get to the `Install` screen.
- Select `Custom install`. There will be no drives shown there, that's when the VIRTIO drivers we downloaded before come into play.
- Click `Load driver > Browse` and find the driver that says something like `...w10`. Refer to [this guide](https://linuxhint.com/install_virtio_drivers_kvm_qemu_windows_vm/) if you're having problems.
- Continue installing like normal.
- After finishing the installation you should be able to see the login screen. If you don't get any output on the external display but can still use the VM through window created by Virt-Manager then go to [Solving Error 43](#solving-error-43).

## Solving Error 43

Read [Arch Wiki Error 43](https://wiki.archlinux.org/index.php/PCI_passthrough_via_OVMF#%22Error_43:_Driver_failed_to_load%22_on_Nvidia_GPUs_passed_to_Windows_VMs) and, if you're on a laptop or ran out of things to try, [NVIDIA RTX 2060 Mobile successful passthrough](https://old.reddit.com/r/VFIO/comments/ebo2uk/nvidia_geforce_rtx_2060_mobile_success_qemu_ovmf/).

Steps:
- Copy the file `SSDT1.dat`, which you find in this repository in `qemu-kvm/SSDT1.dat` to the pool we created earlier.
- Go to your Virt-Manager and open your VM.
- Click on `Information > Overview > XML`. Your `XML` file should look something like this:
```
<domain xmlns:qemu="http://libvirt.org/schemas/domain/qemu/1.0" type="kvm">
  ...
  <features>
    ...
    <hyperv>
      ...
      <vendor_id state="on" value="fucknvidia"/>
      ...
    </hyperv>
    <kvm>
      <hidden state="on"/>
    </kvm>
    ...
  </features>
  ...
  <qemu:commandline>
    <qemu:arg value="-acpitable"/>
    <qemu:arg value="file=/path/to/your/SSDT1.dat"/>
  </qemu:commandline>
</domain>
```
- The error should be fixed now.
