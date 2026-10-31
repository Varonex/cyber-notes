# Bash network basics

## MAC & IP

### Commands

| Commande         | Description                                                                                                           | Notable options                                                                                                                                                                                                                                                                                  |
| ---------------- | --------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| `arp`            | Displays the ARP table. Legacy command.                                                                               | `-a`: BSD format.<br>`-d [e]` : Add an entry.<br>`-D [a]` : Uses the specified hardware.<br>`-e` : Linux format.<br>`-f [f]` : Reads from a file, or from `/etc/ethers`.<br>`-i [i]` : Uses the specified interface.<br>`-p` : Protocol.<br>`-s [e]` : Deletes an entry.<br>`-v` : Verbose mode. |
| `ifconfig`       | Configuration tool for interfaces.                                                                                    | TODOI                                                                                                                                                                                                                                                                                            |
| `ip [o]`         | Generic command to handle interfaces, ARP, routing, etc.                                                              | `-a` : All.<br>`-c` : Color.<br>`-d` : Complementary details.<br>`-f [f]` : IP family.<br>`-h` : Human-readable.<br>`-s` : Additionnal statistics depending on the context.<br>`-t` : Timestamp.<br>`-V` : Verbose mode.<br><br>`-4` : IPv4.<br>`-6` : IPv6.                                     |
| `ping [i]`       | ICMP protocol.                                                                                                        | TODO                                                                                                                                                                                                                                                                                             |
| `route`          | Configuration tool to handle the routing table.                                                                       | TODO                                                                                                                                                                                                                                                                                             |
| `traceroute [i]` | Progressively sends ICMP packets to a specific destination. Progressively increaes TTL to identify all the middlemen. | TODO                                                                                                                                                                                                                                                                                             |

### Objects of IP

| Object       | Description                             | Subobject                                                                                                                                                                                |
| ------------ | --------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `a[ddress]`  | Handles IP-related operations.          | `add [i]` : Adds an IP.<br>`del [i]` : Deletes an IP.                                                                                                                                    |
| `h[elp]`     | Displays the help section.              |                                                                                                                                                                                          |
| `l[ink]`     | Handles interfaces & their MAC address. | `set [l]` : Handles the interface.                                                                                                                                                       |
| `r[oute]`    | Routing table.                          | `add [r]` : Adds a rule.<br>``                                                                                                                                                           |
| `n[eighbor]` | Table ARP.                              | `add` : Add an association.<br>`change` : Change an association.<br>`del` : Delete an association.<br>`flush` : Delete the entirety of the ARP table.<br>`show` : Display the ARP table. |

## DNS

| Commande       | Description | Options | Descriptions Options |
| -------------- | ----------- | ------- | -------------------- |
| `dig [n]`      |             |         |                      |
| `host [n]`     |             |         |                      |
| `nslookup [n]` |             |         |                      |

## TCP/UDP

| Commande  | Description | Options | Descriptions Options |
| --------- | ----------- | ------- | -------------------- |
| `nc`      |             |         |                      |
| `ncat`    |             |         |                      |
| `netstat` |             |         |                      |
| `ss`      |             |         |                      |
| `openssl` |             |         |                      |
| `tcpdump` |             |         |                      |
| `tshark`  |             |         |                      |

## HTTP

| Commande   | Description                                                                                                  | Options | Descriptions Options |
| ---------- | ------------------------------------------------------------------------------------------------------------ | ------- | -------------------- |
| `curl [u]` | Handy tool to interact with webservers online, whether it is via HTTP(S), or any other protocol (smtp, ...). |         |                      |
| `wget [u]` | Tiny utility tool to make HTTP(S) requests alongside a few other types of requests.                          |         |                      |

## SSH

| Commande          | Description                                                                                                                                                             | Options | Descriptions Options |
| ----------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------- | -------------------- |
| `scp [u]`         | Protocole SCP basé sur SSH<br>pour le transfert de fichiers<br>orienté CLI (utile pour les<br>scripts).<br>SCP protocol based on SSH to quickly transfer files via CLI. |         |                      |
| `sftp [u]`        | SFTP protocol based on FTP and layered over SSH to interactively do file transfer operations. Handy for GUI-based apps.                                                 |         |                      |
| `ssh [u]`         | SSH protocol that inherits from TELNET, allowing remote shell connections.                                                                                              |         |                      |
| `ssh-add`         |                                                                                                                                                                         |         |                      |
| `ssh-agent`       |                                                                                                                                                                         |         |                      |
| `ssh-copy-id [u]` |                                                                                                                                                                         |         |                      |
| `ssh-keygen`      | Generate (pub, priv) key couple for SSH connections.                                                                                                                    |         |                      |
| `ssh-keyscan [s]` |                                                                                                                                                                         |         |                      |

## Firewall

| Commande    | Description | Options | Descriptions Options |
| ----------- | ----------- | ------- | -------------------- |
| `arptables` |             |         |                      |
| `ebtables`  |             |         |                      |
| `iptables`  |             |         |                      |
| `ip6tables` |             |         |                      |
| `nftables`  |             |         |                      |
| `ufw`       |             |         |                      |
