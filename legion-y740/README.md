# Lenovo Legion Y740

## Laptop Info

- Model: `Legion Y740-17IRHg`
- [Lenovo's drivers](https://pcsupport.lenovo.com/br/en/products/laptops-and-netbooks/legion-series/legion-y740-17irhg/downloads/driver-list)
- [How to install Lenovo Vantage](https://support.lenovo.com/us/en/solutions/ht505081)

# BIOS

- To access the BIOS keep hitting `F2` after turning the laptop on.
- To access the bootable drives menu keep hitting `F12` after turning the laptop on.

# Switchable graphics

To enable the use of switchable graphics you need to install specific drivers. Enable switchable graphics in the BIOS and then in Manjaro run:
```
$ sudo mhwd -a pci 0300
```
You may need to remove the already installed drivers, do:
```
$ sudo mhwd -r pci DRIVERS_NAME
```
