# Wireless and Mobile Security Assessment

**Frameworks:** NIST SP 800-153 (WLAN security), NIST SP 800-124 Rev. 2 (mobile device security), NIST SP 1800-22 (BYOD security), GDPR, CCPA  
**Skills:** Risk assessment, security architecture, network segmentation, mobile device management, policy development  
**Context:** B.S. Cybersecurity coursework (WGU), rewritten for this portfolio

## Scenario
A fast-growing social media company handling user personal data had a flat wireless network, remote employees without secure access, and an unmanaged bring-your-own-device (BYOD) policy. I assessed the risks and recommended a remediation plan that supports the company's growth and compliance obligations.

## Findings
| Area | Vulnerability | Risk |
|---|---|---|
| WLAN | **No network segmentation.** All devices share one flat network. | One compromised device can scan the network, harvest credentials, and move laterally to sensitive systems. |
| WLAN | **No VPN or MFA for remote access.** Staff connect from public networks, including IT staff reaching production servers. | Intercepted credentials could expose user PII and proprietary source code. |
| Mobile | **BYOD without mobile device management (MDM)** | No way to enforce security settings, detect jailbroken or outdated devices, or remotely wipe lost ones. |
| Mobile | **No required device encryption** | A lost or stolen phone could expose company email, cached credentials, and customer data, creating regulatory exposure. |

## Recommendations

**1. Segment the WLAN.**
- Create separate VLANs and SSIDs for corporate, BYOD, and guest devices.
- Control traffic between them with firewall rules and ACLs.
- Require WPA3-Enterprise with 802.1X authentication.

**2. Secure remote access.**
- Deploy an enterprise VPN with mandatory MFA.
- Grant VPN access by role (least privilege).
- Require device posture checks (patches, encryption, antivirus) before connection.
- Log VPN access for monitoring and audit.

**3. Deploy MDM.**
- Require enrollment before any device can reach company resources.
- Enforce passcodes and OS updates, and enable remote wipe.
- Monitor compliance reports.

**4. Enforce full-disk encryption** through MDM, and verify it before granting access.

### Preventive controls (NIST SP 800-153)
- Block simultaneous wired and wireless connections, which can bridge the internal network to untrusted networks.
- Stop devices from auto-joining unknown Wi-Fi networks, reducing rogue access point risk.

### BYOD strategy
- **Managed BYOD for most staff.** Personal devices must enroll in MDM and meet security standards, and they are placed on a restricted VLAN. Non-compliant devices get guest-network access only.
- **Company-owned devices for high-risk roles.** IT administrators, executives, and anyone with elevated access use corporate-owned devices (COPE or CYOD) for stronger configuration control and monitoring.

## What I Learned
Mobile and wireless security is mostly an access-control problem. Knowing which devices connect, what condition they're in, and what they can reach drives almost every recommendation. The same risk-based thinking applies to the least-privilege access work I do in operations today.
