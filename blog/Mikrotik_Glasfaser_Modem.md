---
layout: post
---

# Set up a Mikrotik Router HEXs with Fiber Optic Modem

## Table of Contents

1. [Introduction](#Introduction)
2. [Hardware](#Hardware)
3. [Prerequisites](#Prerequisites)
4. [Configuration](#Configuration)
5. [Troubleshooting](#Troubleshooting)
6. [References](#References)

### Introduction

The configuration guide for setting up a [Mikrotik Router HEXs](https://mikrotik.com/product/hex_s#fndtn-specifications)
a for your home network. The router is used in conjunction with a fiber optic modem to provide Internet access.

Compared to having a home network with a FritzBox router, the Mikrotik router has several advantages including: 
- flexibility to configure custom network deployments including VLANs and VPNs,
- better WLAN coverage and speed as the access point can be placed in a more central location, 
- costs savings (Mikrotik 72.90€ + PoE injector 15€ + Access Point 84.50€ = 172.40€) compared to a FritzBox router (219€), 
- and less space consumption as the router and the access point can be placed separately.

### Hardware

- [o2online.de](https://www.o2online.de/) as an Internet Service Provider.
- [Mikrotik Router HEXs](https://mikrotik.com/product/hex_s#fndtn-specifications) powered by a PoE injector.
- [Fiber Optic Modem 2](https://www.telekom.de/zuhause/geraete-und-zubehoer/wlan-und-router/glasfaser-modem-2) from Telekom.
- [PoE Injector](https://www.tp-link.com/de/business-networking/accessory/tl-poe150s/) compatible with an access point (24V or 48V).
- [Access Point](https://www.tp-link.com/de/business-networking/ceiling-mount-ap/eap653/) or any other compatible access point.

### Prerequisites

- The fiber optic modem is activated by your Internet Service Provider and connected to the fiber optic socket (you need to set up the `Glasfaser-ID` and `Modem-ID`).
- The Mikrotik Router is connected to the fiber optic modem through the PoE injector to the WAN port `ether1`.
- You have credentials to set up you PPPoE WAN connection (username and password). You can get your personal credentials from your 
 account on the `o2online.de` website (`Mein o2 -> Tarif & Optionen -> Vertrag verwalten -> Zugangsdaten für Ihre Hardware`).

### Configuration

The configuration procedure is based on the [Mikrotik RouterOS](https://mikrotik.com/software) version 6.49.15 (stable)
and assumes that the Mikrotik under the default configuration. Otherwise, you need to reset the configuration to the default 
either by using the command `/system reset-configuration` or by pressing the reset button on the router 
(see [Manual:Reset](https://help.mikrotik.com/docs/display/UM/hEX+S#:~:text=Reset%20button&text=Press%20the%20button%20and%20apply,configuration%20and%20bridge%20all%20interfaces.)).

You can the commands below to configure the Mikrotik Router HEXs by copying and pasting it either:
- into the terminal `ssh admin@192.168.88.1` (note that `TAB` key can be used for auto-completion) 
- or by using the `Winbox GUI`
- or by `http://192.168.88.1/webfig/#Terminal` in your browser

#### Configuration Steps

The configuration includes the following steps:

```shell
/ip dhcp-server set [find name=defconf] name=dhcp-server lease-time=1d

/interface vlan add interface=ether1 vlan-id=7 name=vlan07-o2 comment=o2
/interface pppoe-client
  add interface=vlan07-o2 \
    add-default-route=yes \
    user="USE_YOUR_OWN_PPPOE_USER@q1.ccx-o2.de" \
    password="USE_YOUR_OWN_PPPOE_PASSWORD" \
    disabled=yes \
    use-peer-dns=no \
    name=pppoe-out1 \
    comment=o2
/interface list member add list=WAN interface=pppoe-out1 comment=o2

/ip dns set allow-remote-requests=yes servers=8.8.8.8,8.8.4.4
/ip firewall nat add action=masquerade chain=srcnat out-interface=ether1 comment="Glasfaser-Modem 2"
/ip address add address=192.168.100.2/30 interface=ether1 comment="Glasfaser-Modem 2"

/system ntp client set enabled=yes
/system ntp client set server-dns-names=de.pool.ntp.org
/system clock set time-zone-name=Europe/Berlin

/ip service set api disabled=yes
/ip service set api-ssl disabled=yes
/ip service set ftp disabled=yes
/ip service set telnet disabled=yes
  
/interface pppoe-client set [find name=pppoe-out1] disabled=no
```

### Troubleshooting

The following notes may help you to troubleshoot the configuration process:
- Keep in mind that the `o2online.de` require tagging your PPPoE WAN connection with VLAN 7 (the `vlan-id=7 name=vlan07-o2` interface) if the modem does not do it automatically.
- The PPPoE connection must be disabled by default (`disabled=yes`) and enabled only after the configuration is completed.


### References

- [Virtual Local Area Network (VLAN)](https://help.mikrotik.com/docs/display/ROS/VLAN)
- [Reset Mikrotik Configuration](https://wiki.mikrotik.com/wiki/Manual:Reset)
- [Mikrotik HEXs Startup Guide](https://help.mikrotik.com/docs/display/UM/hEX+S)
- [Advanced Mikrotik Configuration](https://github.com/ViToni/mikrotik-router-setup/)