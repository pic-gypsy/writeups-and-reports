# TryHackMe Billing CTF — Penetration Testing Report

**Assessment type:** Capture the Flag (CTF) / Controlled lab  
**Target:** Billing  
**Platform:** TryHackMe  
**Author:** Abhishek Dinesh  
**Status:** Completed  
**Classification:** Educational — Authorized lab environment

---

## 1. Executive Summary

The Billing CTF exercise involved identifying exposed network services, enumerating a web application, investigating a directory traversal vulnerability, obtaining shell access, and exploring Linux privilege-escalation techniques.

The assessment used network scanning, web directory enumeration, SQL injection testing, an exploitation framework, reverse-shell techniques, and Linux privilege-escalation references.

A key operational lesson concerned Fail2Ban and access stability. The challenge highlighted the importance of understanding authentication monitoring and avoiding actions that could cause the attacking IP to be blocked.

## 2. Scope and Objectives

The assessment was limited to the authorized Billing CTF target.

Objectives:

- Identify exposed network services.
- Discover web application resources and potential vulnerabilities.
- Investigate the directory traversal weakness.
- Establish and improve shell access.
- Investigate local privilege-escalation opportunities.
- Document findings and lessons learned.

## 3. Tools Used

| Tool | Purpose |
|---|---|
| Nmap | Network and service enumeration |
| DirBuster | Web directory and resource discovery |
| sqlmap | Automated SQL injection testing |
| Metasploit Framework | Exploit and payload testing |
| GTFOBins | Research into potentially exploitable Unix/Linux binaries |
| Python reverse-shell generator | Generate a Python reverse-shell payload |
| TTY shell techniques | Improve shell interactivity |

## 4. Reconnaissance and Enumeration

### 4.1 Network scanning

Nmap identified the following exposed ports during initial reconnaissance:

| Port | Service / protocol | Observation |
|---|---|---|
| 22/TCP | SSH | Remote administration service |
| 80/TCP | HTTP | Web application |
| 3886/TCP | Unconfirmed | Service identification requires further evidence |

The initial scan established the target's accessible attack surface. Service-version detection and additional enumeration would be necessary to identify the exact software and potential weaknesses associated with each service.

### 4.2 Web enumeration

DirBuster was used to discover web directories and resources. This helped investigate accessible paths and identify areas for further testing.

Record the discovered resources and exact endpoints from the original scan results.

## 5. Vulnerability Investigation

### 5.1 Directory traversal

**Category:** Path traversal / directory traversal  
**Affected component:** Billing web application — exact endpoint not recorded  
**Potential impact:** Unauthorized access to files outside the intended application directory, depending on server configuration and file permissions.

Directory traversal occurs when an application processes user-controlled file paths without adequate validation or containment checks.

During the exercise, directory traversal was identified as a vulnerability of interest. The exact request, affected parameter, accessed file, and demonstrated impact should be added from the lab notes before treating the finding as fully validated.

**Recommended remediation:**

- Canonicalize and validate user-supplied paths.
- Restrict file access to approved directories.
- Prefer allowlisted file identifiers over arbitrary filesystem paths.
- Apply least-privilege permissions to the web service account.
- Add regression tests for traversal payloads and encoded path variations.

### 5.2 SQL injection testing

**Tool:** sqlmap

sqlmap was used during the assessment to investigate potential SQL injection issues.

The available notes do not establish whether SQL injection was confirmed or exploited successfully. Therefore, this report does not claim SQL injection as a verified finding.

**Recommended remediation if SQL injection is confirmed:**

- Use parameterized queries or prepared statements.
- Avoid constructing SQL queries through unsafe string concatenation.
- Apply least-privilege database permissions.
- Return generic errors to clients and log diagnostic details securely.

## 6. Shell Access and Terminal Handling

### 6.1 Python reverse shell

A web-based utility was used to generate a Python reverse-shell payload during the authorized lab exercise.

A reverse shell allows a target system to initiate a connection back to a listener controlled by the operator. It can provide command execution when a suitable vulnerability or execution path exists.

The exact initial-access vulnerability and payload execution method were not preserved in the available notes.

### 6.2 Interactive TTY shell

TTY-related techniques were used to improve shell interactivity.

A basic reverse shell may lack normal terminal handling, job control, and interactive input support. A TTY upgrade can make administration and subsequent lab operations more manageable.

**Operational lesson:** Understand the limitations of a shell and maintain awareness of session stability during a controlled assessment.

## 7. Privilege Escalation Investigation

**Area investigated:** Fail2Ban and Linux privilege escalation  
**Reference:** GTFOBins

The exercise involved investigating Fail2Ban-related privilege escalation and using GTFOBins as a reference for potentially exploitable system binaries.

Fail2Ban is an intrusion-prevention utility that monitors specified logs and can apply actions, such as firewall bans, when configured conditions are met.

Fail2Ban itself is not inherently a privilege-escalation vulnerability. Escalation depends on the specific configuration, permissions, executable actions, or other weaknesses present on the target.

### Operational observation

The challenge demonstrated that triggering Fail2Ban's configured conditions could result in the attacking IP being blocked. This highlighted the importance of understanding the lab's lockout behavior and maintaining awareness of session stability.

The precise vulnerable configuration, commands executed, and root-verification evidence should be added before describing the mechanism as a reproducible exploit.

## 8. Metasploit Usage

The Metasploit Framework was also used during the exercise.

Metasploit supports exploit research, payload handling, and controlled exploitation workflows. The precise module, target component, and outcome were not recorded in the available notes.

This report therefore records Metasploit as a tool used without attributing a specific successful exploit to it.

## 9. Findings Summary

| Finding | Assessment status |
|---|---|
| SSH exposed on port 22 | Observed |
| HTTP exposed on port 80 | Observed |
| TCP port 3886 exposed | Observed; service unconfirmed |
| Directory traversal | Reported as identified; endpoint and impact evidence pending |
| SQL injection | Testing performed; result unconfirmed |
| Fail2Ban-related privilege escalation | Reported as used; exact mechanism pending |
| Python reverse shell | Technique reported as used |
| Interactive TTY shell | Technique reported as used |
| Metasploit | Tool reported as used; successful module unconfirmed |
| Root access | Reported as achieved by the participant; verification evidence not included |

## 10. Lessons Learned

1. **Enumerate before exploiting.** Network and web enumeration help narrow down the attack surface.
2. **Validate findings.** A tool reporting a possibility does not automatically establish a vulnerability.
3. **Understand shell limitations.** Reverse shells and interactive TTY shells serve related but distinct purposes.
4. **Investigate permissions and configurations.** Linux privilege escalation often depends on how services and executable actions are configured.
5. **Understand defensive controls.** Fail2Ban can affect connectivity and should be considered when interpreting authentication failures and session loss.
6. **Preserve evidence.** Exact requests, commands, outputs, and verification steps make a report reproducible and defensible.

## 11. Recommendations

- Patch exposed services and remove unnecessary network access.
- Validate file paths and restrict application filesystem access.
- Use parameterized database queries.
- Review service permissions and privileged executable configurations.
- Audit Fail2Ban actions, configuration ownership, and write permissions.
- Restrict outbound connections where practical and monitor unexpected reverse connections.
- Record security events centrally and verify that alerts are actionable.

## 12. Conclusion

The Billing CTF provided hands-on practice in network reconnaissance, web enumeration, vulnerability investigation, shell handling, and Linux privilege-escalation research.

The main outcome was learning to connect the different phases of an assessment while considering the effects of defensive mechanisms such as Fail2Ban.

This report reflects the tools and techniques described by the participant. Exact exploit details and supporting evidence remain to be added where they were not preserved in the available notes.

---

## Evidence to Add Before Publication

- Original Nmap command and output.
- DirBuster results and the vulnerable web endpoint.
- The directory traversal request and a redacted proof of impact.
- sqlmap findings, including whether injection was confirmed.
- Metasploit module name and result, if applicable.
- The relevant Fail2Ban configuration or permission weakness.
- Evidence of successful privilege escalation and root verification.
- Sanitized screenshots and any flags, if permitted by the platform's rules.
