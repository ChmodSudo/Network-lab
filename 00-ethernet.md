# Module 0: Ethernet and MAC addresses

## 1. The problem
Two machines, A and B, are connected by a cable. A cable only carries
electrical or light signals, which means 0s and 1s. Two questions arise:
how does B know where a message starts and ends, and, when several machines
share the same network, how does B know the message is meant for it?

## 2. The solution: the Ethernet frame
Data is cut into small units called frames. A frame works like an envelope:

| Field | Role |
|---|---|
| Destination MAC | Who the frame is for |
| Source MAC | Who sent it |
| Type | What the content is (IPv4, ARP, ...) |
| Data | The content itself |
| FCS (Frame Check Sequence) | Error check: a damaged frame is dropped |

## 3. The MAC address
A MAC (Media Access Control) address is the hardware identifier of a network
card. It is 48 bits long, written as 6 octets in hexadecimal.
Example (mine, masked): `40:25:c2:xx:xx:xx`
- The first 3 octets (`40:25:c2`) form the OUI (Organizationally Unique
  Identifier): they identify the manufacturer of the card.
- The last 3 octets are a serial number given by the manufacturer to this card.

A MAC address identifies the network card (the hardware), not the place
where the machine is connected. That is why it does not change when I move
to another network. This is also why it cannot be used to reach machines on
the Internet, which is the reason for IP addresses (Module 1).

The address `ff:ff:ff:ff:ff:ff` is the broadcast address: a frame sent to it
is received by every machine on the local network.

## 4. Hub versus switch

| | Hub | Switch |
|---|---|---|
| Sends the frame to | All ports, always | Only the port of the destination MAC |
| Learns MAC addresses | No | Yes, in a MAC address table |
| Used today | Almost never (obsolete) | Everywhere |

A switch is faster and more private than a hub, because machines do not
receive frames that are not meant for them.

## 5. My interfaces

| Interface | State | Address | Type | Assigned by | Scope | Role |
|---|---|---|---|---|---|---|
| `lo` | UNKNOWN (normal) | `127.0.0.1/8` | IPv4 | The system | This machine only | Loopback: local tests, nothing leaves the machine |
| `enp0s25` | DOWN | none | n/a | n/a | n/a | Ethernet port, no cable plugged in |
| `wlp4s0` | UP | `192.168.62.x/24` | IPv4 (private) | The router, via DHCP | My local network | Wi-Fi, my only active connection |
| `virbr0` | DOWN | `192.168.122.1/24` | IPv4 (private) | libvirt | This machine and its virtual machines | Virtual bridge for VMs, inactive because no VM is running |

## 6. What I observed
- `ip link show wlp4s0`: `LOWER_UP` means the physical layer is active, so
  my Wi-Fi is connected to the access point. `UP` alone only means the
  interface is enabled by the system.
- `mode DORMANT`: Linux keeps a Wi-Fi interface "dormant" until the
  authentication with the access point is finished. Together with
  `state UP`, it is the normal display of a working Wi-Fi.
- MTU of 1500: the maximum size of the data in one Ethernet frame, in bytes.

## 7. Practical case: server without network
Symptom: a server has no network after being moved to another room.
Diagnosis: `ip link show eno1` displays `NO-CARRIER`, so the interface is
enabled but no signal is detected on the cable.
Solution: check the cable and the switch port, replug or replace the cable,
then confirm that `LOWER_UP` appears. Lesson: always check the physical layer
first, before suspecting the IP configuration or the DNS.
