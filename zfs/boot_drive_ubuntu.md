# Creating a native boot environment

This guide is pretty good -> https://docs.zfsbootmenu.org/en/latest/guides/ubuntu/noble-uefi.html#install-and-configure-zfsbootmenu

It is pretty easy to modify to boot into raidz/mirror.

## First thing's first

Configure APT so you don't accidently install GRUB -> https://gist.github.com/firepol/adca8ecc331d4c5ff1bd220ff35e5256

## Configuring networking

On Ubuntu you'll need to find your network interface (hopefully the generic kernel supports one). 

Setup networking

```bash
export DEVICE="YOUR_DEVICE"
cat <<EOF >/etc/systemd/network/20-dhcp.network
[Match]
Name=${DEVICE}

[Network]
DHCP=yes
EOF

systemctl enable systemd-networkd
systemctl start systemd-networkd

cat <<EOF >/etc/netplan/01-netcfg.yaml
network:
  version: 2
  renderer: networkd
  ethernets:
    ${DEVICE}:
      dhcp4: true
EOF

netplan apply
systemctl status systemd-networkd
```

That is all it took to get me online in Ubuntu 24.04

## That's it?

Now your package manager -should- work, so getting a fully operational system should be doable.
