# Payroll Incident  Access Control Case Study (Oct 3, 2023)

- **Incident Timestamp**: Oct 3, 2023, 08:29:57
- **Account/Context**: `Legal\Administrator` event added a payroll entry to `FAUX_BANK`; originating IP `152.207.255.255` (contractor: Robert Taylor Jr.)

## Executive Summary

- **What happened**: A payroll transaction was created under an administrative account tied to a contractor who should have been offboarded. The event used IP `152.207.255.255` and targeted `FAUX_BANK`.
- **Root causes**: Poor account lifecycle management (contractor account remained active), excessive admin privileges for many users, and lack of segregation of duties for payroll actions.
- **Impact**: Unauthorized payroll activity risk, financial loss potential, and regulatory/compliance exposure.

## Event Log (Key entries)

- **10/03/2023 08:29:57**: Payroll event posted under `Legal\Administrator`.
- **Source IP**: `152.207.255.255` (contractor  Robert Taylor Jr.).
- **Target**: `FAUX_BANK` payroll entry.

## Findings (Access Control Issues)

- **Stale contractor account**: Contractor account stayed active beyond contract end date.
- **Excessive privileges**: Broad admin rights assigned irrespective of role (e.g., graphic designer, sales associate).
- **No segregation of duties (SoD)**: Single user could both create and approve payroll changes.
- **Lack of periodic reviews**: No documented quarterly access audits to catch mismatches.

## Recommendations (Mitigations & Implementation Steps)

- **Account Lifecycle Management**: Revoke access at offboarding.
  - Implement automated deprovisioning tied to HR/contractor management systems (SCIM, HR feed).
  - Create a mandatory offboarding workflow that disables accounts within 24 hours of contract end.
  - Log and alert on accounts that remain active past end-date.
- **Role-Based Access Control (RBAC)**: Apply least privilege.
  - Define role catalog (e.g., Payroll Approver, Payroll Initiator, HR Admin, Legal Read-Only).
  - Map permissions to roles and remove direct admin membership from non-admin job roles.
  - Implement just-in-time (JIT) elevation for tasks requiring temporary privilege escalation.
- **Segregation of Duties + Dual Approval**:
  - Require two distinct identities for initiation and approval of payroll transactions.
  - Enforce policy in the payroll system (block single-user approve+create actions).
- **Multi-Factor Authentication (MFA)**:
  - Mandate MFA for all privileged accounts and for any access to financial systems.
  - Use phishing-resistant methods (FIDO2, hardware tokens) for admins.
- **Periodic Access Reviews & Monitoring**:
  - Run quarterly access reviews; HR and business owners must attest to user-role appropriateness.
  - Implement automated reports for privileged accounts, stale access, and high-risk activities.
  - Create SIEM alerts for payroll-related actions from contractor IPs or unusual geolocations.
- **Audit & Forensics**:
  - Retain audit logs for payroll systems and admin activities for a minimum of 1 year.
  - Ensure logs are tamper-evident and stored off-platform for investigations.

## Sample Controls & Quick Wins

- **Immediate**: Disable contractor accounts with terminated contracts; enforce MFA for all admin accounts.
- **3060 days**: Implement RBAC baseline and dual-approval on payroll workflows.
- **90 days**: Deploy automated offboarding integration with HR and run first quarterly access review.

## Sample Audit Checklist (for quarterly reviews)

- **Account status**: Are any accounts active after end-date? (Yes/No)
- **Privileges**: Do non-admin roles have admin-level permissions? (Yes/No)
- **SoD**: Can one user both create and approve payroll? (Yes/No)
- **MFA**: Are all privileged accounts using MFA? (Yes/No)
- **Logging**: Are payroll events logged and retained? (Yes/No)
- **Remediation**: Is there evidence of timely removal action for violations? (Yes/No)

## Policy Snippet (Offboarding)

- **Policy**: "All contractor and employee accounts will be disabled within 24 hours of contract termination or employee separation. Business owners must verify account removal within 72 hours. Exceptions require recorded approval and a documented mitigation plan."

## Metrics to Track

- Time from termination to account disablement (target: <= 24 hours).
- Percentage of privileged accounts with MFA enabled (target: 100%).
- Number of SoD violations detected per quarter (target: 0).
- Results of quarterly access attestations (target: 100% completion).

## Key Takeaways

- Incident root cause: weak account lifecycle processes and excessive privileges.
- Primary mitigations: automated offboarding, RBAC + JIT, MFA, SoD enforcement, and quarterly audits.
- Outcome: applying these controls reduces attack surface and prevents similar payroll fraud.

---

*Prepared for portfolio inclusion.*
