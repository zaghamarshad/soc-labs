# SOC Lab Project: DoS Detection Using Firewall, Suricata IDS, and Splunk

## Project Overview

This project demonstrates how to **detect Denial-of-Service (DoS) behavior** in an isolated lab environment using:

* Linux routing and firewall (iptables)
* Suricata IDS
* Splunk SIEM

The focus of this project is **detection and analysis**, not attacking.
All traffic is generated inside a **safe, isolated virtual lab**.

---

## Purpose of This Project

The main goals of this lab were:

* To understand how DoS traffic looks at the **network level**
* To learn how **firewall logs** and **IDS logs** differ
* To detect abnormal traffic using **SIEM logic**
* To practice **real SOC troubleshooting**, not just theory

---

##  Lab Architecture

![Network Discovery Screenshot](image.png)

```
Client VM (NAT)
   |
   | 192.168.96.0/24
   |
Firewall VM (2 NICs)
   - ens33: NAT (192.168.96.198)
   - ens37: Host-only (192.168.56.1)
   |
   | 192.168.56.0/24
   |
Server VM (Host-only)
   - 192.168.56.10
```


---

## Tools Used

* Ubuntu Server (Client, Firewall, Server)
* iptables (Firewall + logging)
* Suricata IDS (Detection)
* Splunk Enterprise (SIEM)
* hping3 (traffic simulation – lab only)
* tcpdump (packet verification)

---

## What Problem I Faced (Real Issues)

### Problem 1: Firewall Logs Were Not Appearing

* iptables does not log traffic by default
* Only system logs were visible in Splunk

**Solution**

* Added explicit LOG rules in iptables
* Verified logs in `/var/log/syslog` before sending to Splunk

---

### Problem 2: Firewall Was Not in Traffic Path

* Client and server were on the same network
* Traffic bypassed firewall completely

**Solution**

* Redesigned network using NAT + Host-only
* Added second NIC to firewall
* Fixed routing on client and server

---

### Problem 3: Client Could Not Ping Server

* Client did not know how to reach Host-only network

**Solution**

```bash
sudo ip route add 192.168.56.0/24 via 192.168.96.198
```

---

### Problem 4: Suricata Was Running But No Alerts

* Suricata had **zero rules loaded**

**Solution**

* Fixed `suricata.yaml` rule path
* Removed missing `suricata.rules`
* Loaded `local.rules` correctly

---

## 🔐 Firewall Configuration (iptables)

### Enable IP Forwarding

```bash
sudo sysctl -w net.ipv4.ip_forward=1
```

### Allow Forwarding

```bash
sudo iptables -A FORWARD -i ens33 -o ens37 -j ACCEPT
sudo iptables -A FORWARD -i ens37 -o ens33 -m state --state ESTABLISHED,RELATED -j ACCEPT
```

### Enable NAT

```bash
sudo iptables -t nat -A POSTROUTING -o ens33 -j MASQUERADE
```

### Log Traffic

```bash
sudo iptables -A FORWARD -j LOG --log-prefix "FIREWALL_LOG: "
```

---

## Suricata IDS Setup

### Installed Suricata

```bash
sudo apt install suricata -y
```

### Interfaces Monitored

```yaml
af-packet:
  - interface: ens33
  - interface: ens37
```

### Custom Detection Rule (`local.rules`)

```text
alert icmp any any -> any any (msg:"ICMP Ping Detected"; sid:1000002; rev:1;)
```

### SYN Flood Detection Rule

```text
alert tcp any any -> any 80 (flags:S; threshold:type both, track by_src, count 20, seconds 10; msg:"Possible SYN Flood"; sid:1000003; rev:1;)
```

### Test Configuration

```bash
sudo suricata -T -c /etc/suricata/suricata.yaml -v
```

---

## Traffic Simulation (Safe Lab Only)

### ICMP Test

```bash
ping 192.168.56.10
```

### SYN Flood Simulation

```bash
sudo hping3 -S -p 80 -c 100 192.168.56.10
```

---

## Splunk Configuration

### Logs Ingested

* `/var/log/syslog` (firewall logs)
* `/var/log/suricata/eve.json`
* Apache access logs

### Example Detection Queries

#### Top Talkers

```spl
index=firewall
| rex "SRC=(?<src_ip>\S+)"
| stats count by src_ip
| sort -count
```

#### Traffic Spike Detection

```spl
index=firewall
| bucket _time span=1m
| stats count by _time
| eventstats avg(count) as baseline
| where count > baseline*5
```

#### Suricata Alerts

```spl
index=suricata event_type=alert
| stats count by src_ip alert.signature
```

---

## What I Learned

* A firewall must be **in the routing path** to be effective
* Logs do not appear automatically — **logging must be enabled**
* IDS without rules = blind sensor
* Routing issues are more common than tool issues
* DoS detection is about **patterns**, not tools
* SIEM detection depends on **clean data ingestion**

---

## Difference Between Firewall vs IDS Detection

| Firewall         | Suricata IDS         |
| ---------------- | -------------------- |
| Sees connections | Sees packets         |
| Rate-based       | Signature + protocol |
| Simple logging   | Deep inspection      |
| Blocks traffic   | Detects behavior     |

---


## Conclusion

This project helped me move from **tool-based learning** to **real SOC-style troubleshooting**.
I learned how networking, firewalls, IDS, and SIEM work together in real environments.

This lab reflects **real-world blue team scenarios**, not just textbook examples.

---

##  Author

**Zagham Arshad**
SOC / Blue Team Learner
Canada 🇨🇦

---

