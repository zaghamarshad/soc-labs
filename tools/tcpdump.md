# `tcpdump` Command Guide

The `tcpdump` command is a powerful network packet analyzer used for troubleshooting and analyzing network activity. 

---

## **Listing Available Network Interfaces**

To display the available network interfaces on your system, use the following command:

```bash
tcpdump -D
```

### Description:
The `-D` option lists all the interfaces on your machine that `tcpdump` can capture packets from.

---

### Example Output:
Below is an example of what the output might look like:

```
1.ens33 [Up, Running, Connected]
2.any (Pseudo-device that captures on all interfaces) [Up, Running]
3.lo [Up, Running, Loopback]
4.bluetooth-monitor (Bluetooth Linux Monitor) [Wireless]
5.nflog (Linux netfilter log (NFLOG) interface) [none]
6.nfqueue (Linux netfilter queue (NFQUEUE) interface) [none]
7.dbus-system (D-Bus system bus) [none]
8.dbus-session (D-Bus session bus) [none]

```

