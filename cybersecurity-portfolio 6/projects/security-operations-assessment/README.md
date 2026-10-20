# Security Operations Assessment: Access Control, Incident Response and Data Protection

**Frameworks:** NIST SP 800-53 Rev. 5, NIST SP 800-61 Rev. 3 (CSF 2.0), NIST SP 800-41, NIST SP 800-37 (RMF), FIPS 197  
**Skills:** Access reviews and RBAC design, log analysis, incident response evaluation, network segmentation, firewall rule writing, encryption planning  
**Context:** B.S. Cybersecurity coursework (WGU), rewritten for this portfolio

## Scenario

FinSecure Corp. is a fictional financial services company. This is a three-part security review of one organization. I worked from its user-role matrix, access-control policies, firewall rules, IDS and authentication logs, and network and system architecture diagrams, and found where they broke down. For each gap I recommended a fix and mapped it to a NIST control.

| Part | Focus | Main deliverables |
|---|---|---|
| 1 | Access control & security operations | RBAC design, revised 24-account role matrix, policy gap analysis, threat identification from logs |
| 2 | Incident response & network security | Evaluation of a RAT incident response, network architecture review, firewall rule rewrites |
| 3 | Data protection & resilience | Encryption recommendations, architecture flaw analysis, monitoring and backup improvements |

---

## Part 1: Access Control and Security Operations

### RBAC design and access review
I recommended **role-based access control (RBAC)** so access follows job function instead of being granted person by person. Reviewing the user-role matrix against that model turned up four misalignments:

| User | Issue | Fix |
|---|---|---|
| Junior system admin | Standing Domain Admin rights to all internal systems | Delegated admin rights only; temporary elevation requires approval, expires, and is logged (AC-6) |
| Customer support rep | Access to the payroll system | Removed; role limited to CRM and email (AC-6) |
| Terminated HR assistant | Account still active with read/write on HR portal and payroll | Disabled (AC-2) |
| Contractor past end date | Account still active | Disabled (AC-2) |

I then rebuilt the full **24-account role matrix**, covering employees, contractors, a vendor, an intern, a shared reporting account and a service account, so that every account's system access and privilege level matches its role.

### Policy gaps

| Gap in current policy | Recommendation | Control |
|---|---|---|
| Accounts stay active up to 7 days after departure | Tie deactivation to the documented end date so it happens automatically | AC-2 |
| Shared accounts required to cut licensing costs, passwords changed yearly | Individual accounts by default; approved shared accounts get an owner, limited access and logging | IA-2 |
| Unrestricted access to system logs | Read-only access by role; only a few admins can modify or delete; access to logs is itself logged | AU-9 |

### Operational practices
- **Change management:** firewall changes were undocumented, and emergency changes needed no approval or review. Recommended a change ticket with approval, testing and a rollback plan, plus after-the-fact review of emergency changes (CM-3).
- **Security awareness:** training was one email a year and a five-minute briefing for new hires. Recommended onboarding training, regular refreshers and phishing simulations (AT-2).
- **Asset management:** laptops weren't inventoried, and there was no check-in or wipe process. Recommended tracking by serial number, user and status, with reimaging before reuse (CM-8, MP-6).

### Threats identified in the logs

| Finding | Evidence | Response |
|---|---|---|
| Likely brute-force account compromise | 5 failed logins and then a success from an unfamiliar external IP, followed the next day by a DNS request to a known malicious domain | Disable the account, kill sessions, reset credentials, require MFA before restoring (AC-7, IA-2) |
| GhostX RAT on an internal host | High-severity malware alert | Isolate the host through endpoint security, remove the malware, check for spread (IR-4, SI-3) |
| Logins after end dates | Successful logins by a terminated employee and an expired contractor | Automate deprovisioning from HR and contract end dates (AC-2) |

---

## Part 2: Incident Response and Network Security

### Evaluating the GhostX RAT response
I reviewed the response timeline against the NIST CSF 2.0 **Detect, Respond and Recover** functions from SP 800-61 Rev. 3:

| Action | Assessment |
|---|---|
| **Detect:** IDS flagged outbound C2 traffic | Partially effective. The alert fired 38 minutes after the malware was installed, and the investigation that followed missed related activity. |
| **Respond:** workstation isolated 15 minutes after the alert | Reasonably fast, but SMB lateral movement from the same host appeared 38 minutes after isolation, so containment either failed or was undone. |
| **Recover:** endpoint restored from backup | Restored availability, but a malicious DNS query came from the same IP the next morning, and AV definitions were updated only after that. Nothing showed the threat was actually gone. |

**Recommended improvements:**
1. Automated EDR quarantine on confirmed C2 activity, plus an organization-wide search for related indicators (same user, IP or signature) across network, DNS, authentication and endpoint logs.
2. A recovery checkpoint before reconnecting a system: verify the backup is clean, update definitions, run a full scan, reset credentials and monitor the endpoint.

### Network architecture weaknesses

| Weakness | Remediation |
|---|---|
| Web and mail servers sit behind the same firewall as internal networks, with no DMZ | Move public servers into a DMZ; deny DMZ-to-internal traffic unless there's a documented need |
| HR and Finance subnets share a switch with no filtering between them | Separate VLANs or zones with an internal firewall/ACL; block and log everything else. This would have stopped the observed HR → Finance SMB attempt. |
| Firewall management console reachable from any source on port 8080 | Dedicated management network, VPN or approved admin devices only, MFA, and logging of login attempts |

### Firewall rule rewrites
I found two rule weaknesses, tied each to evidence in the IDS log, and wrote replacement rules:

**Rule 102: Any → Internal Networks, any service, Allow.** This let in an external port scan of the Finance subnet and internal SMB lateral movement.

```
Source:       Any
Destination:  Internal networks
Service:      Any
Action:       Deny, log, alert
Placement:    After specific allow rules for approved business traffic (default deny)
```

**Rule 103: Internal → Any external, any service, Allow.** This let the infected workstation query a malicious domain straight through a public resolver (8.8.8.8), bypassing company DNS filtering and logging.

```
Source:       HR and Finance endpoint networks
Destination:  Any external DNS server
Service:      UDP/TCP 53
Action:       Deny, log, alert
Exception:    Only the approved internal resolver may query external DNS
```

---

## Part 3: Data Protection and System Resilience

### Cryptographic protections

| Risk | Recommendation | Protects |
|---|---|---|
| Payroll and employee data sent between HR and Finance over unencrypted FTP | Replace with SFTP over SSH | Confidentiality of files and credentials in transit |
| Customer PII and financial records stored in clear text in the Finance database | AES-256 (transparent database encryption where supported), keys stored apart from the database server | Confidentiality at rest, even if the database files are copied |

### Architecture design flaws

| Flaw | Countermeasure | Control |
|---|---|---|
| Web interface, app server and sensitive record systems share one data center zone with no internal boundary | Separate segment for the web tier; internal firewall rules allow only required connections | SC-7 |
| Domain Admin work done from ordinary IT user workstations | Dedicated hardened admin workstation, separate privileged account, MFA | AC-6 |
| Single gateway into the data center | Second gateway with automatic failover, tested regularly | CP-8 |

### Resilience improvements
- **Centralized monitoring:** logs were kept on each device and firewall/VPN reports reviewed weekly. Recommended a SIEM that collects system, application, firewall, VPN and endpoint events and alerts in near real time (AU-6).
- **Backups:** nightly backups used standard permissions, and restores were tested once a year. Recommended immutable backups for the retention period, restricted backup administration and quarterly restore tests (CP-9).

---

## What I Learned
The same few gaps kept showing up. The terminated employee whose account stayed active was also logging in after their end date. The flat network that let one firewall rule allow everything also let malware reach Finance from HR. Fixing access control, segmentation and monitoring together closes far more than fixing any one alone. Reviewing the incident also taught me that closing a ticket isn't the same as removing the threat: a host can be restored from backup and still show malicious activity the next morning.

## References
- NIST SP 800-53 Rev. 5, *Security and Privacy Controls for Information Systems and Organizations* (2020). https://doi.org/10.6028/NIST.SP.800-53r5
- Nelson, A., et al. NIST SP 800-61 Rev. 3, *Incident Response Recommendations and Considerations for Cybersecurity Risk Management* (2025). https://doi.org/10.6028/NIST.SP.800-61r3
- Scarfone, K., & Hoffman, P. NIST SP 800-41 Rev. 1, *Guidelines on Firewalls and Firewall Policy* (2009). https://doi.org/10.6028/NIST.SP.800-41r1
- NIST SP 800-37 Rev. 2, *Risk Management Framework* (2018). https://doi.org/10.6028/NIST.SP.800-37r2
- NIST FIPS 197, *Advanced Encryption Standard* (2023 update). https://doi.org/10.6028/NIST.FIPS.197-upd1
- Wills, M. *The Official (ISC)² SSCP CBK Reference*, 6th ed. (2022)

*Case scenario, logs and diagrams were provided by WGU and are not reproduced here. This write-up summarizes my own analysis and recommendations.*
