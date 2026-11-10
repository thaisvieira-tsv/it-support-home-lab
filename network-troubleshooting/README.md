# Basic Network Connectivity and DNS Troubleshooting

## 1. Scenario

A simulated IT support scenario involving a computer connected to Wi-Fi but unable to access websites.

The objective was to investigate network configuration, local and external connectivity, DNS resolution, network routing and HTTPS connectivity using Windows Command Prompt.

The tests were performed to assess whether the computer could communicate with its local gateway, reach external destinations, resolve domain names and access a website over HTTPS.

## 2. Environment and Tools

**Environment:**
- Operating System: Microsoft Windows
- Network Connection: Wi-Fi
- Network Adapter: Intel Wi-Fi 6 AX201 160MHz

**Tools Used:**
- `ipconfig`
- `ipconfig /all`
- `ping`
- `nslookup`
- `tracert`
- `curl`

## 3. Troubleshooting and Results

### Test 1: Inspect Network Configuration

**Objective:** Identify the active network adapter and check whether the computer has an IPv4 address and default gateway.

**Command:**
```cmd
ipconfig
```

**Relevant Results:**
```text
Wireless LAN adapter Wi-Fi:

IPv4 Address:       [redacted]
Subnet Mask:        255.255.252.0
Default Gateway:    [local gateway]
```

**Observation:**

The Wi-Fi adapter had an IPv4 configuration and a default gateway. Other network adapters were also present, including a VirtualBox virtual adapter and disconnected interfaces. The Wi-Fi adapter was the active connection used for this investigation.

### Test 2: Test Local Network Connectivity

**Objective:** Determine whether the computer could communicate with its default gateway.

**Command:**
```cmd
ping [local gateway]
```

**Relevant Results:**
```text
Reply from [local gateway]: bytes=32 time=43ms TTL=64
Reply from [local gateway]: bytes=32 time=8ms TTL=64
Reply from [local gateway]: bytes=32 time=1ms TTL=64
Reply from [local gateway]: bytes=32 time=2ms TTL=64

Packets: Sent = 4, Received = 4, Lost = 0 (0% loss)

Minimum = 1ms, Maximum = 43ms, Average = 13ms
```

**Observation:**

All four ICMP packets received a response from the default gateway, confirming successful communication between the computer and the local network gateway.

The first response was slower than the subsequent responses. However, no packet loss was observed during this test.

### Test 3: Test External IP Connectivity

**Objective:** Determine whether the computer could reach an external IP address without relying on DNS name resolution.

**Command:**
```cmd
ping 8.8.8.8
```

**Relevant Results:**
```text
Pinging 8.8.8.8 with 32 bytes of data:
Reply from 8.8.8.8: bytes=32 time=3ms TTL=115
Reply from 8.8.8.8: bytes=32 time=3ms TTL=115
Reply from 8.8.8.8: bytes=32 time=6ms TTL=115
Reply from 8.8.8.8: bytes=32 time=3ms TTL=115

Packets: Sent = 4, Received = 4, Lost = 0 (0% loss)

Minimum = 3ms, Maximum = 6ms, Average = 3ms
```

**Observation:**

All four packets received a response from `8.8.8.8`, Google's public DNS server.

This confirmed successful connectivity to an external IP address during testing. Combined with the previous test, it demonstrated that communication was working both within the local network and beyond the default gateway.

### Test 4: Test DNS Resolution

**Objective:** Check whether the computer could resolve a domain name into its corresponding IP addresses.

**Command:**
```cmd
nslookup google.com
```

**Relevant Results:**
```text
Server:  unifi
Address: [local DNS server]

Name:    google.com
Addresses:
    [Google IPv6 address]
    [Google IPv4 address]
```

**Observation:**

The DNS query was sent to the local DNS server, identified as `unifi`.

The server successfully returned IPv4 and IPv6 addresses for `google.com`, confirming that DNS resolution was working for the tested domain.

The DNS server address matched the computer's default gateway, suggesting that the local network device was also providing DNS resolution.

### Test 5: Inspect Detailed Network Configuration

**Objective:** Investigate the computer's network configuration, including DHCP, DNS server and gateway information.

**Command:**
```cmd
ipconfig /all
```

**Relevant Results (Wi-Fi adapter):**
```text
Description: Intel(R) Wi-Fi 6 AX201 160MHz

DHCP Enabled: Yes
IPv4 Address: [redacted]
Subnet Mask: 255.255.252.0
Default Gateway: [local gateway]
DHCP Server: [local DHCP server]
DNS Servers: [local DNS server]
```

**Observation:**

DHCP was enabled, meaning the computer obtained its network configuration automatically from a DHCP server.

The DHCP server, default gateway and DNS server all used the same local network address.

This provided further insight into how the computer received its network configuration and which device it used for gateway and DNS services.

### Test 6: Trace the Network Route

**Objective:** Identify the route taken by network traffic from the computer to an external destination.

**Command:**
```cmd
tracert -4 google.com
```

**Relevant Results:**
```text
Tracing route to google.com
over a maximum of 30 hops:

1     1 ms     7 ms     1 ms   [local gateway]
2     4 ms     4 ms     4 ms   [ISP router]
...
9     2 ms     2 ms     5 ms   [intermediate router]
10    2 ms     2 ms     2 ms   [destination]

Trace complete.
```

**Observation:**

The trace completed successfully after 10 hops.

The first hop was the local gateway, followed by intermediate routers within the internet service provider's network and Google's network.

All reported hops responded to the traceroute probes, with no visible timeouts.

This demonstrated how `tracert` can be used to investigate the path taken by network traffic and identify intermediate devices between a computer and an external destination.

### Test 7: Test HTTPS Connectivity

**Objective:** Test whether the computer could establish an HTTPS connection and receive an HTTP response from an external web server.

**Command:**
```cmd
curl -I https://www.google.com
```

**Relevant Results:**
```text
HTTP/1.1 200 OK
Content-Type: text/html; charset=ISO-8859-1
Server: gws
```

**Observation:**

The request successfully reached Google's web server and returned an HTTP `200 OK` response.

This indicated that the HTTPS request was successfully processed and that the server returned a successful response.

Unlike the previous ping tests, this test checked web connectivity using HTTP over HTTPS.

## 4. Overall Findings

All seven diagnostic checks completed successfully during the exercise.

The computer obtained its IPv4 configuration through DHCP, established communication with its default gateway and successfully reached an external IP address.

DNS resolution for `google.com` was successful through the local network's DNS resolver. Traceroute confirmed that traffic reached Google's network through the local gateway and intermediate routers. An HTTPS request to Google also returned an HTTP `200 OK` response.

Based on these results, local gateway connectivity, external IP connectivity, DNS resolution for the tested domain and HTTPS connectivity to Google were functioning at the time of testing.

These results do not establish that every website or application was accessible, nor do they identify or resolve a specific network fault.

If the simulated user continued experiencing problems, further investigation would focus on the affected websites, browser configuration, proxy settings, firewall rules and application-specific issues.

## 5. What I Learned

- How to identify active and inactive network adapters using `ipconfig`.
- How to interpret IPv4 addresses, subnet masks and default gateways.
- How to use `ping` to test local and external network connectivity.
- How DNS translates domain names into IP addresses.
- How DHCP automatically provides network configuration.
- How to use `tracert` to investigate the route taken by network traffic.
- How to use `curl` to test HTTPS connectivity and interpret HTTP response codes.
- How to interpret command-line output and document evidence-based troubleshooting findings.

## 6. Possible Future Improvements

- Compare DNS resolution using the local DNS resolver and a public DNS server.
- Investigate how DNS forwarding works on the local network device.
- Test connectivity to additional websites and services.
- Explore common troubleshooting scenarios involving DNS failures, packet loss and incorrect gateway configurations.

---

*This project was completed as a practical learning exercise to develop foundational IT support and network troubleshooting skills.*
