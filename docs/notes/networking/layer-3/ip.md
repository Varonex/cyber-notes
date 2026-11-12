# IP

Protocol organized in addresses to communicate between nodes of different networks.
Two distinct IP versions exist : `IPv4` and `IPv6`.

| Mode      | Definition               | Description                                                                                                 | Example                                                           |
| --------- | ------------------------ | ----------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------- |
| Unicast   | 1:1 communication.       | Direct communication between an emitter and a single receiver.                                              | Connecting to a website.                                          |
| Broadcast | 1:All communication.     | A single emitter sends a packet to its entire LAN. Is not routed. Does no longer exist in IPv6.             | DHCP discover request.<br>Host ID full of 1's.                    |
| Multicast | 1:Many communication.    | A single emitter can send communications to a dedicated group of machines. Works like a subscription model. | mDNS (IPv4 support through Class D addresses)<br>IPv6-native.     |
| Anycast   | 1:Nearest communication. | A myriad of geographically separate servers share the same IP address. Routing determines the nearest host. | Root DNS servers being interconnected but geographically distant. |

| Scope                  | Definition                                      |
| ---------------------- | ----------------------------------------------- |
| Loopback               | Local-to-host traffic. Cannot be routed out.    |
| Link local             | Local-to-subnet traffic. Cannot be routed out.  |
| Private / Local unique | Internal network traffic. Cannot be put online. |
| Global unicast         | Can reach internet and navigate everywhere.     |

## IPv4

The first IP protocol that was used worldwide.

### How it works

Addresses : 4 unsigned bytes. `W.X.Y.Z`.
An address comes with a mask : `W'.X'.Y'.Z'`. The mask separates the Net ID from the Host ID.

| Class | Prefix     | Net ID Range                     | Base Mask                    |
| ----- | ---------- | -------------------------------- | ---------------------------- |
| A     | `0xxxxxxx` | `1.0.0.0` to `126.0.0.0`         | `255.0.0.0`, noted `/8`      |
| B     | `10xxxxxx` | `128.0.0.0` to `191.255.0.0`     | `255.255.0.0`, noted `/16`   |
| C     | `110xxxxx` | `192.0.0.0` to `223.255.255.0`   | `255.255.255.0`, noted `/24` |
| D     | `1110xxxx` | `224.0.0.0` to `239.255.255.255` | /, considered `/4`           |
| E     | `1111xxxx` | `240.0.0.0` to `255.255.255.255` | /, considered `/4`           |

!!! warning "Reserved addresses"
	- `0.0.0.0/8` : the default/unspecified address.
	- `127.0.0.0/8` : the loopback address (`127.0.0.1` = `localhost`).
	- `255.255.255.255`: broadcast in the entire network, and is useful in DHCP discover requests, as the machine does not know anything about the network it is in yet.
	- `169.254.0.0/16`: link local, doesn't get routed at all to keep packets in the network.

!!! note "Behaviors"
	- `Address & Mask = Net ID`. All 1's give the Net ID.
	- `Address & !Mask = Host ID`.
	- If the Host ID is full of 0's, the address is a network address.
	- If the Host ID is full of 1's, the address is a broadcast address.
	- Subnetting consists in toggling additional 0's into 1's from left to right on the mask to create subnets.

### Header

<div class="packet-frame-wrapper">
    <table class="packet-frame">
        <colgroup>
            <col style="width: 40px;"/>
            <col span="32" style="width: 3%;"/>
        </colgroup>
        <tr>
            <td></td>
            <td colspan="8">0 - 7</td>
            <td colspan="8">8 - 15</td>
            <td colspan="8">16 - 23</td>
            <td colspan="8">24 - 31</td>
        </tr>
        <tr>
            <td>0</td>
            <td colspan="4">Version</td>
            <td colspan="4">IHL</td>
            <td colspan="8">Type of Service</td>
            <td colspan="16">Total Length</td>
        </tr>
        <tr>
            <td>4</td>
            <td colspan="16">Identification</td>
            <td>R</td>
            <td>DF</td>
            <td>MF</td>
            <td colspan="13">Fragment Offset</td>
        </tr>
        <tr>
            <td>8</td>
            <td colspan="8">TTL</td>
            <td colspan="8">Protocol</td>
            <td colspan="16">Checksum</td>
        </tr>
        <tr>
            <td>12</td>
            <td colspan="32">Source Address</td>
        </tr>
        <tr>
            <td>16</td>
            <td colspan="32">Destination Address</td>
        </tr>
        <tr>
            <td>20</td>
            <td colspan="24">Options (0-40 bytes)</td>
            <td colspan="8">Padding (4B alignment)</td>
        </tr>
        <tr>
            <td>20<br/>to<br/>60</td>
            <td colspan="32">Data</td>
        </tr>
    </table>
</div>

| Field                        | Length    | Description                                                                                                           | Value                                                          |
| ---------------------------- | --------- | --------------------------------------------------------------------------------------------------------------------- | -------------------------------------------------------------- |
| Version                      | 4b        | Version 4.                                                                                                            | `0100`                                                         |
| I(nternet) H(eader) L(ength) | 4b        | Header length. The value is multiplied by 4 to align it on 4B.                                                        | `5-15 * 4 = 20-60`                                             |
| Type of Service              | 1B        | Defines handling priority. Now divided in two distinct parts : `DSCP` (6b) and `ECN` (2b).                            | `0x00` to `0xFF`                                               |
| Total Length                 | 2B        | Total length. Defines the maximum IP packet size of `2^16`.                                                           | `(IHL * 4) + \|Data\|`<br>in `20-65535`                        |
| Identification               | 2B        | Unique identifier given to the initial packet. Helps the recipient in regrouping split packets back.                  | `0-65535`                                                      |
| R(eserved)                   | 1b        | /                                                                                                                     | `0`                                                            |
| D(on't) F(ragment)           | 1b        | If enabled, disables fragmentation.                                                                                   | `0`: Allows fragmentation.<br>`1`: Disallow fragmentation.     |
| M(ore) F(ragments)           | 1b        | If set to 1, indicates that further fragments are incoming (to assemble them back).                                   | `0`: No remaining fragment.<br>`1`: More fragments are coming. |
| Fragment Offset              | 13b       | Indicates the exact position in bytes relative to the original packet. Multiplied by 8 to express an amount of bytes. | `0-8191 * 8 = 0-65528`                                         |
| T(ime) T(o) L(ive)           | 1B        | Number of hops before the packet gets discarded. Gets decreased by each network node until it reaches 0.              | `0-255`                                                        |
| Protocol                     | 1B        | IANA ID for the protocol encapsulated by this packet.                                                                 | `0-255`<br>- `1`: ICMP<br>- `6`: TCP<br>- `17`: UDP<br>...     |
| Checksum                     | 2B        | Checksum.                                                                                                             | Checksum                                                       |
| Source Address               | 4B        | Source address.                                                                                                       | IP                                                             |
| Destination Address          | 4B        | Destination address.                                                                                                  | IP                                                             |
| Options + Padding            | 0B to 40B | Additional options. Padding is here to ensure the word is aligned on a multiple of 4B.                                | /                                                              |
| Data                         | Varying   | The data segment of the packet.                                                                                       | /                                                              |

### Attack types

- **IP spoofing** : forging the source IP address to impersonate a trusted host or trigger reflection/amplification attacks.
- Fragmentation attacks
	- **Overfragmentation** : fragment packets a lot to evade IDS and other stateless security devices.
	- **Fragment overlap** : manipulate the `Fragment Offset` values so fragments overwrite each other, exploiting differences in, how the IDS and the target OS reassemble the payload.
- **Source routing abuse** : Using IP `Options` to force packets through an attacker-controlled path.

## IPv6

TODO

<div class="packet-frame-wrapper">
    <table class="packet-frame">
        <colgroup>
            <col style="width: 40px;"/>
            <col span="32" style="width: 3%;"/>
        </colgroup>
        <tr>
            <td></td>
            <td colspan="8">0 - 7</td>
            <td colspan="8">8 - 15</td>
            <td colspan="8">16 - 23</td>
            <td colspan="8">24 - 31</td>
        </tr>
        <tr>
            <td>0</td>
            <td colspan="4">Version</td>
            <td colspan="8">Traffic Class</td>
            <td colspan="20">Flow Label</td>
        </tr>
        <tr>
            <td>4</td>
            <td colspan="16">Payload Length</td>
            <td colspan="8">Next Header</td>
            <td colspan="8">Hop Limit</td>
        </tr>
        <tr>
            <td>8</td>
            <td colspan="32" rowspan="4">Source Address</td>
        </tr>
        <tr><td>12</td></tr>
        <tr><td>16</td></tr>
        <tr><td>20</td></tr>
        <tr>
            <td>24</td>
            <td colspan="32" rowspan="4">Destination Address</td>
        </tr>
        <tr><td>28</td></tr>
        <tr><td>32</td></tr>
        <tr><td>36</td></tr>
        <tr>
            <td>40</td>
            <td colspan="32">Data</td>
        </tr>
    </table>
</div>
