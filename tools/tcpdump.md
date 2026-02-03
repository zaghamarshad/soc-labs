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

<img width="700" height="203" alt="Screenshot 2026-02-02 at 3 23 17 PM" src="https://github.com/user-attachments/assets/21cfa9b9-1014-44dc-9cb0-b8f6c91537bb" />

## **To listen on loopback**

```bash
sudo tcpdump -i lo
```

On a specific port:

```bash
sudo tcpdump -i lo port 5555
```

Set up a server-client connection using Netcat (`-l` for listen, `-v` for verbose, and `-p` for port):

```bash
nc -lvp 5555
```

Read a PCAP file:

```bash
tcpdump -r pcapfile.pcap
```

Count total packets:

```bash
tcpdump -r pcapfile.pcap --count
```

Read first 2 packets:

```bash
tcpdump -r pcapfile.pcap -c 2
```

Show timestamps as Unix epoch:

```bash
tcpdump -tt -r pcapfile.pcap
```

Filter examples:

```bash
tcpdump -tt -r 2021-09-14.pcap port 80 and host 10.0.0.168 | grep -E "GET|\.exe"
tcpdump -tt -r 2021-09-14.pcap port 80 and host 10.0.0.168 | grep -E "\.exe"
tcpdump -tt -r 2021-09-14.pcap port 80 and host 10.0.0.168 | grep -E "GET.*\.exe"
```

Extract source IPs and count:

```bash
tcpdump -tt -r 2024-04-18.pcap -n tcp | cut -d " " -f 3
tcpdump -tt -r 2024-04-18.pcap -n tcp | cut -d " " -f 3 | cut -d "." -f 1-4
tcpdump -tt -r 2024-04-18.pcap -n tcp | cut -d " " -f 3 | cut -d "." -f 1-4 | sort
tcpdump -tt -r 2024-04-18.pcap -n tcp | cut -d " " -f 3 | cut -d "." -f 1-4 | sort | uniq
tcpdump -tt -r 2024-04-18.pcap -n tcp | cut -d " " -f 3 | cut -d "." -f 1-4 | sort | uniq -c
tcpdump -tt -r 2024-04-18.pcap -n tcp | cut -d " " -f 3 | cut -d "." -f 1-4 | sort | uniq -c | sort -nr
```

Exclude lines containing “user”:

```bash
grep -v user
```

