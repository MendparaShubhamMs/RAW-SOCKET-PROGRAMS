# Linux Raw-Socket Programming Lab: Layer 2 & Layer 3

**Student:** Mendpara Shubham  
**Roll No / ID:** 25CS044  
**Course / Subject:** Computer Networks (Raw Socket Programming)

This repository contains a comprehensive collection of C programs that demonstrate experiential understanding of both Layer-2 (Data Link) and Layer-3 (Network) communication using Linux raw sockets. By bypassing the standard OS network stack, these programs interact directly with network hardware to capture, parse, construct, and inject raw Ethernet frames, ARP packets, and IPv4/ICMP packets.

### 🌐 Interactive Study Guides & Flowcharts
* **[Layer 2: Ethernet & ARP Flowcharts](Layer-2/index.html)**
* **[Layer 3: IPv4 & Routing Flowcharts](Layer-3/index.html)**


### ▶️ View the Live Demonstration & Code in Action
* **[Layer 2: Video Explaination](https://www.youtube.com/playlist?list=PLXEDWIEtlgzg)**
* **[Layer 3: Video Explaination](https://www.youtube.com/playlist?list=PLaWN859ZDZJw)**

---

## 📁 Repository Structure

### `/Layer2` - Data Link Layer (Ethernet & ARP)
These programs demonstrate hardware-level addressing and address resolution.
1. **`raw_ethernet_capture.c`**: Captures all incoming Ethernet frames and parses basic MAC address and EtherType information.
2. **`raw-ethernet-receive-parse.c`**: Binds directly to a specific network interface via `ioctl` and performs software-level MAC filtering (Accept/Drop) simulating a physical NIC.
3. **`raw-ethernet-send-frame.c`**: Constructs and injects custom Ethernet frames into the network, demonstrating payload padding for minimum frame sizes and custom EtherType (`0x88B5`) routing.
4. **`raw-arp-receive.c`**: Isolates and parses only `ETH_P_ARP` traffic, mapping raw bytes to standard ARP Request and Reply structures.
5. **`raw-arp-request-reply.c`**: Broadcasts a manually crafted ARP Request for a specific IP address and waits in a listening loop to capture and resolve the corresponding ARP Reply.

### `/Layer3` - Network Layer (IPv4 & ICMP)
These programs demonstrate packet construction, IP headers, and router decision-making.
1. **`raw_ip_receive_process.c`**: Captures incoming IPv4 packets and processes header fields including Version, Header Length, Total Length, TTL, Protocol, Checksum, and IP addresses.
2. **`raw_ip_send_process.c`**: Constructs and sends a manual IPv4 packet containing an ICMP Echo Request payload. Utilizes `IP_HDRINCL` to bypass the OS's IP header generation.
3. **`raw_ip_router_parse.c`**: Simulates core router processing on captured packets. Checks TTL, performs longest-prefix matching against a custom routing table, selects the next hop, decrements the TTL, and recalculates the header checksum.

---

## 🛠️ Technologies & Concepts
* **Language:** C
* **OS:** Linux (Tested on Ubuntu/VirtualBox)
* **Networking Layer 2:** Ethernet Frames, MAC Addressing, ARP, Broadcast/Multicast/Unicast, Minimum Frame Padding.
* **Networking Layer 3:** IPv4 Headers, ICMP, Time-to-Live (TTL), Header Checksums, Longest-Prefix Matching, Routing Tables.
* **APIs:** Linux Raw Sockets (`AF_PACKET`, `SOCK_RAW`, `AF_INET`, `IPPROTO_RAW`), Socket Options (`IP_HDRINCL`), `ioctl`.

---

## ⚠️ Important Execution Notes

* **Root Privileges:** Because these programs interact directly with the hardware interface and bypass the OS network layers, they **must** be executed with `sudo`.
* **VirtualBox NAT Routing:** If testing inside a VirtualBox NAT environment, `10.0.2.2` is commonly the virtual default gateway. Use this IP for testing external ARP requests and IP destinations.
* **Network Safety:** The frame and packet injection scripts are designed for isolated lab environments or virtual machines. **Do not capture or inject custom frames on unauthorized production networks.**

---

## ⚙️ Compilation & Execution

Standard GCC compilation is used for all scripts. Navigate to the respective folder and compile:

```bash
# Example compilation
gcc -Wall -Wextra program_name.c -o program_name

# Example execution (Requires interface name or IPs depending on the script)
sudo ./program_name enp0s3
