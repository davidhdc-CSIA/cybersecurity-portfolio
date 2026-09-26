# Cybersecurity Legal and Ethics Case Analysis

**Frameworks:** Computer Fraud and Abuse Act (CFAA), Electronic Communications Privacy Act (ECPA), Sarbanes-Oxley (SOX), ISC2 and EC-Council codes of ethics  
**Skills:** Legal and regulatory analysis, access control governance, policy development, security awareness program design  
**Context:** B.S. Cybersecurity coursework (WGU), rewritten for this portfolio

## Scenario
An internal investigation at a technology consulting firm found serious problems in its business intelligence unit:
- **Improper accounts:** accounts created under former employees' names were given elevated privileges and used to access financial and executive documents.
- **Attacks on other companies:** penetration testing tools were used to scan and break into outside companies' networks without authorization.
- **Shell companies:** these may have been used to inflate sales figures.
- **Client data leaks:** confidential client data ended up with competitors.

I analyzed the legal exposure and the ethical failures, then recommended policies to prevent a recurrence.

## Legal Analysis
| Law | Violation | Status |
|---|---|---|
| **CFAA** | Improperly created accounts with escalated privileges accessed protected financial and executive documents without authorization | Non-compliant |
| **ECPA** | Unauthorized penetration and scanning of outside companies' networks, plus intelligence gathering on other companies' communications and stored data | Non-compliant |
| **SOX** | Payments from possible shell companies may have inflated sales, misleading investors | Non-compliant |

**Negligence (failure of duty of care):**
- No user account audits
- No least privilege or separation of duties; every workstation had full administrative rights
- No information barriers between client teams
- A security analyst with a conflict of interest approved the improper accounts

## Root Causes
- **No oversight of accounts:** no one audited accounts or privilege changes, so the improper accounts went unnoticed.
- **No data separation:** no data classification or loss prevention controls kept one client's information apart from another's.
- **Conflict of interest:** a personal relationship between the unit's leader and the analyst responsible for overseeing it went unchecked.

## Recommendations
1. **User account management and audit policy:** Regularly review accounts and privilege changes, and flag and disable accounts belonging to former employees.
2. **Least privilege and separation of duties:** Limit each account to what the role requires, and ensure no single person controls a critical process.
3. **Data loss prevention program:** Inventory and classify data, set handling rules, deploy centralized DLP, and train staff.
4. **Ethics and conflict-of-interest policy:** Define improper relationships and gifts, and create reporting channels.
5. **Security awareness training program:**
   - **Leadership:** the CISO, security, and compliance teams set its direction.
   - **Delivery:** HR runs the training, with completion tracked.
   - **Content:** ethical standards (ISC2, EC-Council), conflicts of interest, and the legal consequences of unauthorized access.

## What I Learned
Almost every violation here traces back to a missing access control. The improper accounts, excessive privileges, and lack of audits made the misconduct possible and let it go undetected. Governance failures become technical failures, and the reverse is true too.
