# Home Proxmox Server

 In January this year (2023) bought myself a cheap PC from AliExpress £157, 16G of memory and a cheap NVME and SSD Drive. Total spend was about £250, with the hope of teaching myself about Proxmox, Virtualization and Docker.

## PC Specification  

| Hardware | Description |
|-----|----|
 | [Firewall Mini PC Pentium N6005 N5105](https://www.aliexpress.com/item/1005003991560461.html?spm=a2g0o.order_list.order_list_main.4.37b51802ONX0sX)  | 4 x Intel i226-V 2.5G Lans 2*DDR4 2*M.2 NVMe AES-NI Home Router PC Network Security Appliance. |
 | [Samsung 870 QVO](https://www.amazon.co.uk/gp/product/B089QXQ1TV/ref=ppx_yo_dt_b_asin_title_o02_s00?ie=UTF8&th=1) | 1 TB 2.5 Inch Internal Solid State (SSD) |
| [2 x Crucial RAM 8GB DDR4](<https://www.amazon.co.uk/gp/product/B08C4Z69LN/ref=ppx_yo_dt_b_asin_title_o08_s00?ie=UTF8&psc=1>) | 3200MHz CL22 (or 2933MHz or 2666MHz) Portable Memory CT8G4SFRA32A |

## Installation Instructions

I've installed Proxmox on the NVME drive and used the SSD drive to store ISO images, snapshots and virtual machines. I've been using it for a number of months now and I've had no problems so far. As far as backups goes, I've setup proxmox to backup my VM's every day at 21:00, just in case things go south.

I've also created a Debian VM which I'm currently running Docker on, thankfully no issues to speak of, which is great as this my homelab, I don't imagine that I'll be running huge numbers of VM's on it. Saying that, at some point in the future, I do intend to upgrade it to something more powerful and energy efficient.

I followed Techo Tims Instructions - [Before I do anything on Proxmox, I do this first...](https://www.youtube.com/watch?v=GoZaMgEgrHw) video, and it work great. Bare in mind that these instructions are written for Proxmox 8.0.4 and are for my consumption, so if your using a newer version, best to make sure the following is still applicable.

Add the following at the bottom of /etc/apt/sources.list

```shell script
# Added by Billy Dickson 14/04/2023
# not for production use

deb http://download.proxmox.com/debian bookworm  pve-no-subscription
```

Comment out the following in /etc/apt/source.list.d/pve-enterprise.list

```shell script
# deb https://enterprise.proxmox.com/debian/pve bookworm pve-enterprise
```
At the the prompt, type the following

```shell script
apt-get update
apt dist-upgrade
apt autoremove
```

Re-boot proxmox.

```shell script
reboot
```
Give it a few minutes to come back up.

Comment out and add the following in /etc/default/grub

```shell script
#GRUB_CMDLINE_LINUX_DEFAULT="quiet"
GRUB_CMDLINE_LINUX_DEFAULT="quiet intel_iommu=on"
```

Update the grub bootloader

```shell script
upgrade-grub
```

Add the following to /etc/modules

```shell script
vfio
vfio_iommu_type1
vfio_pci
vfio_virqfd
```

Reboot proxmox

```shell script
reboot
```
Give it a minute or two to come back up

### References  

- Download [Proxmox Software](https://hometechhacker.com/5-great-proxmox-small-form-factor-hardware-options/)
- Proxmox VE [Documentation](https://pve.proxmox.com/pve-docs/)
- Proxmox [VLAN Networking](https://www.youtube.com/watch?v=zx5LFqyMPMU)
- LevelOneTech  [Forum](https://forum.level1techs.com/)
- HomeTech Hacker - 5 Great [Proxmox Small Form Factor Hardware Alternatives](https://hometechhacker.com/5-great-proxmox-small-form-factor-hardware-options/)
- Beelink GTR5 review  [A mini Ryzen](https://www.tomsguide.com/reviews/beelink-gtr5)
- Techno Tim  [Documentation](https://technotim.live/)
