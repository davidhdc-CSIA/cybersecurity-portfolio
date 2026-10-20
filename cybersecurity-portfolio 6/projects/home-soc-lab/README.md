# Home SOC Lab: Linux Authentication Monitoring

**Tools:** Splunk Enterprise, Splunk Universal Forwarder, Ubuntu (UTM VM), macOS, Wireshark  
**Skills:** Log forwarding, SPL searches, authentication-event investigation, packet analysis

## What I Built
I built a home SOC lab to practice collecting security logs, investigating authentication activity, and analyzing network traffic. Splunk Enterprise runs on my macOS host, and an Ubuntu VM runs in UTM.

- Installed a Splunk Universal Forwarder on the Ubuntu VM and configured it to send `/var/log/auth.log` to Splunk Enterprise.
- Verified that Linux authentication events arrived in Splunk.
- Wrote searches that find failed password attempts, extract source IP addresses, and group events into five-minute intervals to spot repeated failures.
- Captured traffic in Wireshark and examined DNS queries, TCP connection setup, and TLS handshakes using host and protocol filters.

## What It Demonstrates
The lab gave me hands-on practice with log forwarding, Splunk searches, authentication-event investigation, and packet analysis. The failed-login searches are a starting point for spotting unusual activity and building an alerting baseline.
