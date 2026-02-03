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

<img width="700" height="203" alt="Screenshot 2026-02-02 at 3 23 17 PM" src="https://github.com/user-attachments/assets/21cfa9b9-1014-44dc-9cb0-b8f6c91537bb" />

---

## **Capturing Live Traffic**

### Listen on loopback interface:

```bash
sudo tcpdump -i lo
```

### Capture on a specific port:

```bash
sudo tcpdump -i lo port 5555
```

### Set up a test connection with Netcat:

Use Netcat to set up a server-client connection (`-l` for listen, `-v` for verbose, `-p` for port):

```bash
nc -lvp 5555
```

---

## **Reading PCAP Files**

### Basic read:

```bash
tcpdump -r pcapfile.pcap
```

### Count total packets:

```bash
tcpdump -r pcapfile.pcap --count
```

### Read first N packets:

```bash
tcpdump -r pcapfile.pcap -c 2
```

### Display Unix epoch timestamps:

```bash
tcpdump -tt -r pcapfile.pcap
```

---

## **Filtering Traffic**

### Filter by port and host, search for GET requests or executables:

```bash
tcpdump -tt -r 2021-09-14.pcap port 80 and host 10.0.0.168 | grep -E "GET|\.exe"
tcpdump -tt -r 2021-09-14.pcap port 80 and host 10.0.0.168 | grep -E "\.exe"
tcpdump -tt -r 2021-09-14.pcap port 80 and host 10.0.0.168 | grep -E "GET.*\.exe"
```

---

## **IP Address Analysis**

### Extract and analyze source IPs:

Extract source IP addresses (field 3):

```bash
tcpdump -tt -r 2024-04-18.pcap -n tcp | cut -d " " -f 3
```

Get only the IP address (remove port):

```bash
tcpdump -tt -r 2024-04-18.pcap -n tcp | cut -d " " -f 3 | cut -d "." -f 1-4
```

Sort IP addresses:

```bash
tcpdump -tt -r 2024-04-18.pcap -n tcp | cut -d " " -f 3 | cut -d "." -f 1-4 | sort
```

Remove duplicates:

```bash
tcpdump -tt -r 2024-04-18.pcap -n tcp | cut -d " " -f 3 | cut -d "." -f 1-4 | sort | uniq
```

Count occurrences of each IP:

```bash
tcpdump -tt -r 2024-04-18.pcap -n tcp | cut -d " " -f 3 | cut -d "." -f 1-4 | sort | uniq -c
```

Sort by most frequent IPs:

```bash
tcpdump -tt -r 2024-04-18.pcap -n tcp | cut -d " " -f 3 | cut -d "." -f 1-4 | sort | uniq -c | sort -nr
```

---

## **Additional Filtering**

### Exclude lines containing specific text:

```bash
grep -v user
```

