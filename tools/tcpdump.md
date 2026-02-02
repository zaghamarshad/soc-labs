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

## **To listen at loopback**

```bash
sudo tcpdump -i lo
```

on specific port
```bash
sudo tcpdump -i lo port 5555
```

We can setup server client connection using `NetCat` Command -options l for listen v verbose and p is port
```bash
nc -lvp 5555
```