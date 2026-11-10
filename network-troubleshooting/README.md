# Basic Network Connectivity and DNS Troubleshooting

## 1. Scenario

A user reports that their computer is connected to Wi-Fi but cannot access websites.

The objective of this investigation was to examine network configuration, local and external connectivity, DNS resolution, network routing and HTTPS connectivity using Windows Command Prompt.

## 2. Tools Used

- Windows Command Prompt
- `ipconfig`
- `ipconfig /all`
- `ping`
- `nslookup`
- `tracert`
- `curl`

## 3. Troubleshooting and Results

### Test 1: Inspect Network Configuration

**Command:** `ipconfig`

**Result:**
- Wi-Fi adapter identified.
- IPv4 address present.
- Default gateway present.

**Observation:** The computer had an IPv4 configuration and a default gateway.

### Test 2: Test Local Network Connectivity

**Command:** `ping 192.168.24.1`

**Result:** Four successful replies.

**Response times:** 43 ms, 8 ms, 1 ms and 2 ms.

**Observation:** The computer successfully communicated with its local gateway.

### Test 3: Test External IP Connectivity

**Command:** `ping 8.8.8.8`

**Result:** Four successful replies.

**Response times:** 3 ms, 3 ms, 6 ms and 3 ms.

**Observation:** The computer successfully reached an external IP address.

### Test 4: Test DNS Resolution

**Command:** `nslookup google.com`

**Result:** Successful DNS lookup.

**DNS server:** 192.168.24.1 (unifi)

**Returned addresses:**
- IPv4: 142.250.195.142
- IPv6: 2404:6800:4006:801::200e

**Observation:** The DNS server successfully resolved the domain name and returned IPv4 and IPv6 addresses.

### Test 5: Inspect Network Configuration Details

**Command:** `ipconfig /all`

**Result:**
- DHCP Enabled: Yes
- DNS Server: 192.168.24.1
- Default Gateway: 192.168.24.1

**Observation:** The computer received its network configuration automatically through DHCP. The DNS server and default gateway were using the same local IP address.

### Test 6: Trace the Network Route

**Command:** `tracert -4 google.com`

**Result:**
- First hop: 192.168.24.1
- Trace completed successfully.
- No request timeouts observed.

**Observation:** The trace completed successfully, and all reported hops responded to the traceroute probes.

### Test 7: Test HTTPS Connectivity

**Command:** `curl -I https://www.google.com`

**Result:** HTTP/1.1 200 OK

**Observation:** The computer successfully established an HTTPS connection and received a successful HTTP response from Google's web server.

## 4. Overall Findings

All seven diagnostic checks completed successfully. Testing was performed against Google services. Successful results do not establish that all websites or applications are accessible.

The investigation confirmed that:
- The computer had a valid network configuration.
- Local gateway communication was successful.
- External IP connectivity was available.
- DNS resolution was functioning.
- The route to Google's destination was successfully traced.
- An HTTPS request to Google returned HTTP 200 OK.

No general connectivity or DNS failure was identified during this investigation.

If the original user continued experiencing problems, further investigation would focus on the affected websites, browser configuration, proxy settings, firewall rules and application-specific issues.

## 5. What I Learned

- How to inspect Windows network configuration.
- How to test local and external network connectivity.
- How DNS resolves domain names into IP addresses.
- How to interpret traceroute results.
- How to use curl to test HTTPS connectivity and inspect HTTP response headers.
- How to document troubleshooting findings and develop evidence-based conclusions.

## 6. Possible Future Improvements

- Investigate browser and proxy configuration.
- Compare connectivity across different websites.
- Explore DHCP and DNS configuration in more detail.
- Practise troubleshooting simulated DNS and connectivity failures.
