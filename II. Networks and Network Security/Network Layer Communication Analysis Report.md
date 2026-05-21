
# Network Layer Communication Analysis Report

## Part 1: Summary of the Problem Found in the Tcpdump Log

The network traffic analysis revealed a problem involving the DNS service and the UDP protocol. Customers attempting to access the website `www.yummyrecipesforme.com` received the error message “destination port unreachable,” preventing access to the website.

The tcpdump logs showed that the client computer sent DNS requests using the UDP protocol to port 53 of the DNS server. In response, the server returned an ICMP error message stating: “udp port 53 unreachable.”

Since port 53 is used for DNS services, this indicates that the DNS server was unable to receive or process DNS requests. As a result, the domain name could not be translated into an IP address, preventing the browser from connecting to the website.

The log entries also showed:

* The `A?` flag, which indicates a DNS query requesting an IPv4 address record.
* The `+` symbol, which indicates recursion was desired in the DNS query.

These are normal DNS query indicators and confirm that the client was attempting standard DNS name resolution.

The repeated ICMP “port unreachable” responses suggest that the DNS service on the server was unavailable, misconfigured, blocked, or not listening on port 53.

---

# Part 2: Analysis of the Incident and Possible Causes

## Time of Incident

The incident occurred at approximately `1:24:32.192571 PM`, according to the timestamps in the tcpdump log.

---

## How the IT Team Became Aware of the Incident

Several customers reported being unable to access the website `www.yummyrecipesforme.com`. Users experienced long loading times followed by the error message “destination port unreachable.”

The IT team attempted to access the website and experienced the same issue.

---

## Actions Taken by the IT Department

The IT department used the tcpdump network analysis tool to capture and inspect network traffic while attempting to access the website.

During packet analysis, the team observed:

1. Outgoing UDP packets sent from the client to the DNS server on port 53.
2. ICMP error responses from the DNS server stating “udp port 53 unreachable.”
3. Multiple repeated failed DNS requests.

The incident was then escalated to the security engineering team and reported to the supervisor for further investigation.

---

## Key Findings from the Investigation

The investigation determined that the issue affected the DNS service running on UDP port 53.

Key findings include:

* DNS requests sent by the client could not be delivered successfully.
* The DNS server responded with ICMP “port unreachable” messages.
* The domain name could not be resolved into an IP address.
* Because DNS resolution failed, the client browser could not establish a connection to the website server.

This confirms that the network service affected during the incident was the DNS service.

---

## Likely Causes of the Incident

Possible causes include:

* The DNS service was down or not running on the server.
* Port 53 was blocked by a firewall or access control list.
* The DNS server was misconfigured.
* Network connectivity issues prevented communication with the DNS server.
* A Denial of Service (DoS) or Distributed Denial of Service (DDoS) attack affected DNS availability.
* The DNS server may have been overloaded with requests.
