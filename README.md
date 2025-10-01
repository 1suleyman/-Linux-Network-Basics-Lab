# 🌐 Linux Network Basics Lab

In this lab, I explored **network configuration and troubleshooting** on a Linux system. The lab involved identifying IP addresses, network interfaces, gateways, and testing connectivity to servers.

---

## 📋 Lab Overview

**Goal:**

* Identify IP addresses on primary network interfaces
* Determine interface names and default gateway
* Test connectivity to a remote web server
* Troubleshoot SSH and network issues

**Learning Outcomes:**

* Use `ip addr` and `ip link` to inspect network interfaces
* Use `ip route` to view default gateways and routing tables
* Test TCP connectivity with `telnet`
* Test reachability with `ping`
* Manage network interfaces (`ip link set dev <interface> up`)
* Verify SSH service status (`systemctl status sshd`)

---

## 🛠 Step-by-Step Journey

### Step 1: Identify IP Addresses

```bash
ip addr
```

* Find the IP addresses assigned to the system.
* **Example output for primary interfaces** (ignore Docker/internal interfaces):

```
eth0: 172.16.238.187/24
eth1: 172.16.239.187/24
```

💡 **Tip:** Look for the `inet` lines to find assigned IPv4 addresses.

---

### Step 2: Identify Interface Names

* Use the same `ip addr` output to see interface names.
* **Example:** `eth0`, `eth1`

💡 **Tip:** Interfaces are listed at the start of each block in `ip addr` (e.g., `1: lo`, `16: eth1@if17`).

---

### Step 3: Identify Default Gateway

```bash
ip route
```

* Look for the line starting with `default via`
* **Example:** `default via 172.16.238.1 dev eth0`

💡 **Tip:** The `default` route defines where traffic goes when no specific route matches.

---

### Step 4: Test Connectivity to Web Server

**Goal:** Check if HTTP (port 80) is reachable on a remote server `devapp01-web`.

```bash
telnet devapp01-web 80
```

* If successful: blank screen or connected message
* If failed: connection error (`No route to host`)

💡 **Tip:** `ping <hostname>` can also test reachability.

---

### Step 5: Troubleshoot Network Interface

* Switch to root to avoid repeated `sudo`:

```bash
sudo su
```

* Verify interface is active:

```bash
ip link show eth0
```

* If interface is down:

```bash
ip link set dev eth0 up
```

---

### Step 6: Verify SSH Service

```bash
systemctl status sshd
```

* Ensures SSH server is **active and running**.
* Needed to SSH into remote servers for troubleshooting.

---

### Step 7: Test SSH Connectivity

```bash
ssh user@<remote-server-ip>
```

* Ensure correct password or public key is used.
* If denied, verify interface is up and network route is correct.

---

## ✅ Key Commands Summary

| Task                            | Command / Notes                  |
| ------------------------------- | -------------------------------- |
| List IP addresses               | `ip addr`                        |
| List network interfaces         | `ip link`                        |
| View routing table              | `ip route`                       |
| Test TCP connection (HTTP port) | `telnet <hostname> 80`           |
| Test reachability               | `ping <hostname>`                |
| Bring interface up              | `ip link set dev <interface> up` |
| Check SSH status                | `systemctl status sshd`          |
| SSH to remote server            | `ssh user@<IP>`                  |

---

## 💡 Notes / Tips

* **Ignore internal/Docker interfaces** when identifying system IPs.
* **UP** means the interface is active and ready for traffic.
* **Default gateway** must be reachable for outgoing traffic.
* **Telnet** is useful to test if a specific service/port is reachable.
* **Control-C** stops continuous ping loops.
* Always verify passwords and network connectivity when SSH fails.

---

## 📌 Lab Summary

| Step                     | Status | Key Takeaways                              |
| ------------------------ | ------ | ------------------------------------------ |
| Identify IP addresses    | ✅      | eth0: 172.16.238.187, eth1: 172.16.239.187 |
| Identify interface names | ✅      | eth0, eth1                                 |
| Check default gateway    | ✅      | default via 172.16.238.1 dev eth0          |
| Test HTTP connectivity   | ⚠️     | telnet failed, troubleshoot needed         |
| Verify SSH service       | ✅      | SSH active and running                     |
| Interface management     | ✅      | eth0 brought up successfully               |

---

## ✅ References

* [Linux `ip` command](https://man7.org/linux/man-pages/man8/ip.8.html)
* [Using `telnet` to test TCP ports](https://linuxize.com/post/linux-telnet-command/)
* [Managing SSH service](https://www.ssh.com/ssh/systemctl/)
