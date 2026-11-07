# Glossary

Acronyms used in this repository, grouped by module.

## Module 0: Ethernet and MAC addresses

| Acronym | Full name | Definition |
|---|---|---|
| LAN | Local Area Network | A network limited to one place, such as a home or an office |
| NIC | Network Interface Card | The hardware (wired or Wi-Fi) that connects a machine to a network |
| MAC | Media Access Control | The 48-bit hardware address of a network card, e.g. `40:25:c2:xx:xx:xx` |
| OUI | Organizationally Unique Identifier | The first 3 octets of a MAC address, identifying the manufacturer |
| FCS | Frame Check Sequence | An error-check value at the end of a frame; damaged frames are dropped |
| MTU | Maximum Transmission Unit | The maximum size of the data in one frame, 1500 bytes on Ethernet |
| IEEE | Institute of Electrical and Electronics Engineers | The organization that defines standards (802.3 = Ethernet, 802.11 = Wi-Fi) |
| AP | Access Point | The device that connects Wi-Fi clients to the network |
| SSID | Service Set Identifier | The name of a Wi-Fi network |

## Terms

| Term | Definition |
|---|---|
| Frame | The unit of data sent on an Ethernet network, with source and destination MAC |
| Broadcast | A frame sent to `ff:ff:ff:ff:ff:ff`, received by every machine on the LAN |
| Unicast | A frame sent to one single machine |
| Switch | A device that forwards a frame only to the port of the destination MAC |
| Hub | An obsolete device that repeats every frame to all its ports |
| Loopback | The virtual interface `lo` (`127.0.0.1`), used for tests inside one machine |
