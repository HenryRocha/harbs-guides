# Useful outputs to keep logged:

## Lenovo Legion Y740
- Output for ```sudo dmesg | grep -i -e DMAR -e IOMMU```:
```
henryrocha@legionY740> sudo dmesg | grep -i -e DMAR -e IOMMU                                                                                                                                ~
[    0.000000] Command line: BOOT_IMAGE=/boot/vmlinuz-5.4-x86_64 root=UUID=aaa94180-5af0-48ef-a650-50a03ba9b3d1 rw quiet intel_iommu=on iommu=pt udev.log_priority=3
[    0.009492] ACPI: DMAR 0x000000004EBA6000 000070 (v01 LENOVO CB-01    00000001 ACPI 00040000)
[    0.168924] Kernel command line: BOOT_IMAGE=/boot/vmlinuz-5.4-x86_64 root=UUID=aaa94180-5af0-48ef-a650-50a03ba9b3d1 rw quiet intel_iommu=on iommu=pt udev.log_priority=3
[    0.168980] DMAR: IOMMU enabled
[    0.222342] DMAR: Host address width 39
[    0.222343] DMAR: DRHD base: 0x000000fed91000 flags: 0x1
[    0.222348] DMAR: dmar0: reg_base_addr fed91000 ver 1:0 cap d2008c40660462 ecap f050da
[    0.222349] DMAR: RMRR base: 0x0000004e47d000 end: 0x0000004e49cfff
[    0.222351] DMAR-IR: IOAPIC id 2 under DRHD base  0xfed91000 IOMMU 0
[    0.222351] DMAR-IR: HPET id 0 under DRHD base 0xfed91000
[    0.222352] DMAR-IR: Queued invalidation will be enabled to support x2apic and Intr-remapping.
[    0.225331] DMAR-IR: Enabled IRQ remapping in x2apic mode
[    0.972523] iommu: Default domain type: Passthrough (set via kernel command line)
[    1.198667] DMAR: No ATSR found
[    1.198724] DMAR: dmar0: Using Queued invalidation
[    1.198825] pci 0000:00:00.0: Adding to iommu group 0
[    1.198843] pci 0000:00:01.0: Adding to iommu group 1
[    1.198854] pci 0000:00:04.0: Adding to iommu group 2
[    1.198863] pci 0000:00:08.0: Adding to iommu group 3
[    1.198877] pci 0000:00:12.0: Adding to iommu group 4
[    1.199080] pci 0000:00:14.0: Adding to iommu group 5
[    1.199091] pci 0000:00:14.2: Adding to iommu group 5
[    1.199281] pci 0000:00:14.3: Adding to iommu group 5
[    1.199298] pci 0000:00:15.0: Adding to iommu group 6
[    1.199308] pci 0000:00:15.1: Adding to iommu group 6
[    1.199322] pci 0000:00:16.0: Adding to iommu group 7
[    1.199332] pci 0000:00:17.0: Adding to iommu group 8
[    1.199352] pci 0000:00:1b.0: Adding to iommu group 9
[    1.199377] pci 0000:00:1b.4: Adding to iommu group 10
[    1.199397] pci 0000:00:1d.0: Adding to iommu group 11
[    1.199412] pci 0000:00:1e.0: Adding to iommu group 12
[    1.199438] pci 0000:00:1f.0: Adding to iommu group 13
[    1.199449] pci 0000:00:1f.3: Adding to iommu group 13
[    1.199460] pci 0000:00:1f.4: Adding to iommu group 13
[    1.199470] pci 0000:00:1f.5: Adding to iommu group 13
[    1.199477] pci 0000:01:00.0: Adding to iommu group 1
[    1.199483] pci 0000:01:00.1: Adding to iommu group 1
[    1.199488] pci 0000:01:00.2: Adding to iommu group 1
[    1.199493] pci 0000:01:00.3: Adding to iommu group 1
[    1.199515] pci 0000:3e:00.0: Adding to iommu group 14
[    1.199536] pci 0000:3f:00.0: Adding to iommu group 15
[    1.199538] DMAR: Intel(R) Virtualization Technology for Directed I/O
[    1.510318] AMD-Vi: AMD IOMMUv2 driver by Joerg Roedel <jroedel@suse.de>
[    1.510319] AMD-Vi: AMD IOMMUv2 functionality not available on this system
[   11.522539] nvidia 0000:01:00.0: DMAR: 32bit DMA uses non-identity mapping
```
- Output for IOMMU groups: 
```
henryrocha@legionY740> ./iommu_groups.sh                                                                                                                          ~/Repositories/IOMMU-viewer
IOMMU Group 0:
	00:00.0 Host bridge [0600]: Intel Corporation 8th Gen Core Processor Host Bridge/DRAM Registers [8086:3ec4] (rev 07)
IOMMU Group 1:
	00:01.0 PCI bridge [0604]: Intel Corporation Xeon E3-1200 v5/E3-1500 v5/6th Gen Core Processor PCIe Controller (x16) [8086:1901] (rev 07)
	01:00.0 VGA compatible controller [0300]: NVIDIA Corporation TU106BM [GeForce RTX 2060 Mobile] [10de:1f51] (rev a1)
	01:00.1 Audio device [0403]: NVIDIA Corporation TU106 High Definition Audio Controller [10de:10f9] (rev a1)
	01:00.2 USB controller [0c03]: NVIDIA Corporation TU106 USB 3.1 Host Controller [10de:1ada] (rev a1)
	01:00.3 Serial bus controller [0c80]: NVIDIA Corporation TU106 USB Type-C UCSI Controller [10de:1adb] (rev a1)
IOMMU Group 10:
	00:1b.4 PCI bridge [0604]: Intel Corporation Cannon Lake PCH PCI Express Root Port #21 [8086:a32c] (rev f0)
IOMMU Group 11:
	00:1d.0 PCI bridge [0604]: Intel Corporation Cannon Lake PCH PCI Express Root Port #13 [8086:a334] (rev f0)
IOMMU Group 12:
	00:1e.0 Communication controller [0780]: Intel Corporation Cannon Lake PCH Serial IO UART Host Controller [8086:a328] (rev 10)
IOMMU Group 13:
	00:1f.0 ISA bridge [0601]: Intel Corporation Device [8086:a30d] (rev 10)
	00:1f.3 Audio device [0403]: Intel Corporation Cannon Lake PCH cAVS [8086:a348] (rev 10)
	00:1f.4 SMBus [0c05]: Intel Corporation Cannon Lake PCH SMBus Controller [8086:a323] (rev 10)
	00:1f.5 Serial bus controller [0c80]: Intel Corporation Cannon Lake PCH SPI Controller [8086:a324] (rev 10)
IOMMU Group 14:
	3e:00.0 Non-Volatile memory controller [0108]: Samsung Electronics Co Ltd NVMe SSD Controller SM981/PM981/PM983 [144d:a808]
IOMMU Group 15:
	3f:00.0 Ethernet controller [0200]: Realtek Semiconductor Co., Ltd. RTL8111/8168/8411 PCI Express Gigabit Ethernet Controller [10ec:8168] (rev 15)
IOMMU Group 2:
	00:04.0 Signal processing controller [1180]: Intel Corporation Xeon E3-1200 v5/E3-1500 v5/6th Gen Core Processor Thermal Subsystem [8086:1903] (rev 07)
IOMMU Group 3:
	00:08.0 System peripheral [0880]: Intel Corporation Xeon E3-1200 v5/v6 / E3-1500 v5 / 6th/7th/8th Gen Core Processor Gaussian Mixture Model [8086:1911]
IOMMU Group 4:
	00:12.0 Signal processing controller [1180]: Intel Corporation Cannon Lake PCH Thermal Controller [8086:a379] (rev 10)
IOMMU Group 5:
	00:14.0 USB controller [0c03]: Intel Corporation Cannon Lake PCH USB 3.1 xHCI Host Controller [8086:a36d] (rev 10)
	00:14.2 RAM memory [0500]: Intel Corporation Cannon Lake PCH Shared SRAM [8086:a36f] (rev 10)
	00:14.3 Network controller [0280]: Intel Corporation Wireless-AC 9560 [Jefferson Peak] [8086:a370] (rev 10)
IOMMU Group 6:
	00:15.0 Serial bus controller [0c80]: Intel Corporation Cannon Lake PCH Serial IO I2C Controller #0 [8086:a368] (rev 10)
	00:15.1 Serial bus controller [0c80]: Intel Corporation Cannon Lake PCH Serial IO I2C Controller #1 [8086:a369] (rev 10)
IOMMU Group 7:
	00:16.0 Communication controller [0780]: Intel Corporation Cannon Lake PCH HECI Controller [8086:a360] (rev 10)
IOMMU Group 8:
	00:17.0 SATA controller [0106]: Intel Corporation Cannon Lake Mobile PCH SATA AHCI Controller [8086:a353] (rev 10)
IOMMU Group 9:
	00:1b.0 PCI bridge [0604]: Intel Corporation Cannon Lake PCH PCI Express Root Port #17 [8086:a340] (rev f0)
```
