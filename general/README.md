## Reintall Manjaro on a dual boot configuration with UEFI

To reinstall simply do not format the UEFI partition and the other OS's partions. GRUB will handle the new install.

## Fix A2DP Sink not available for KDE on Manjaro

Edit the file in `/etc/bluetooth/main.conf` and set `AutoEnable=true`.
