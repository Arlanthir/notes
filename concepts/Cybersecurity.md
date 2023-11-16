# Cybersecurity

## Introduction

The goal of cybersecurity is to protect the **confidentiality**, **integrity** and **availability** of *assets*.

### Important concepts

- **Confidentiality**: Information is only available to those who should have access (e.g. by encryption).
- **Integrity**: Data is known to be correct and trusted (e.g. by hashes).
- **Availability**: Information is available for use by legitimate users when it is needed (e.g. by redundancy).
- **Authentication**: Determining the identity of a user (who the user is).
- **Authorization**: Applying access control to a user (what the user can do).
- **Accounting** (Audit): Measuring activity.
- **Non-repudiation**: Preventing a subject from denying previous activity.
- **Least privilege**: Each subject should have only the minimum necessary rights needed to perform their task.
- **Separation of duties**: For any given task, more than one individual needs to be involved.
- **Defense in depth**: Each layer has its own defense mechanisms.
- **Fail safe**: When system fails, it should fail to a safe state (emergency exit open when there is a power failure).
- **Fail secure**: The default state is locked (lock closed when there is a power failure).
- **Single point of failure**: A part of the system that stops the entire system when it fails.

**Asset** is anything that either:
- Holds value (e.g. data, protected by maintaining its integrity).
- Produces value (e.g. server, protected by maintaining its availability).
- Provides access to value (e.g. password, protected by maintaining its confidentiality).

#### Threats

- **Vulnerability**: weakness that makes an asset susceptible to attack or failure.
- **Threats**:
  - **Attack**: intentional action to reduce the value of an asset.
  - **Failure**: unintentional action that can reduce the value of an asset.
- **Exploit**: Code written to take advantage of a vulnerability.

Threat actors:
1. Script kiddies: Low skill, simple attacks, motivated by revenge or fame.
2. Hacktivists: Moderate to high skill, looking to make an example of an organization, motivated by activism.
3. Hackers: High skill, looking to understand how things work.
4. Cyber criminals: High skill, looking for financial exploits (e.g. ransomware).
5. Advanced persistent threat: Very high skill, backed by states, looking to weaken a political adversary, motivated by national interest.

## Identifying vulnerabilities

**CVE**: Common Vulnerabilities and Exposure

System to identify and standardize names for publicly known cybersecurity vulnerabilities (e.g. log4j = [CVE-2021-44228](https://cve.mitre.org/cgi-bin/cvename.cgi?name=cve-2021-44228)).

CVE feeds the National Vulnerability Database (NVD) of NIST (National Institute of Standards and Technology), e.g.: https://nvd.nist.gov/vuln/detail/CVE-2021-44228

**CVSS**: Common Vulnerability Scoring System

Quantifies the severity of a vulnerability between 0-10.

Organizations need to analyze the CVSS in their own context (i.e. a CVSS of 10 may not be critical to your company because the affected applications are not public).

**CWE**: Common Weakness Enumeration

Standardizes weaknesses, e.g. [CWE-89 SQL Injection](https://cwe.mitre.org/data/definitions/89.html).

### [OWASP Top 10 (2021)](https://owasp.org/www-project-top-ten/)

The top 10 most common vulnerabilities in web applications. New list comes out every three years, give or take.

1. Broken Access Control (Authorization)
2. Cryptographic Failures
3. Injection
4. Insecure Design
5. Security Misconfiguration
6. Vulnerable and Outdated Components
7. Identification and Authentication Failures
8. Software and Data Integrity Failures
9. Security Logging and Monitoring Failures
10. Server-Side Request Forgery (SSRF)

#### [OWASP API Security Top 10](https://owasp.org/API-Security/editions/2023/en/0x11-t10/)

Specific for APIs

1. Broken Object Level Authorization
  Change ID of a request and access a resource you shouldn't have access to.
2. Broken Authentication
3. Broken Object Property Level Authorization
4. Unrestricted Resource Consumption
5. Broken Function Level Authorization
6. Unrestricted Access to Sensitive Business Flows
7. Server Side Request Forgery
8. Security Misconfiguration
9. Improper Inventory Management
10. Unsafe Consumption of APIs

[OWASP Cheat Sheets](https://cheatsheetseries.owasp.org/index.html)

### [SANS Top 25](https://www.sans.org/top25-software-errors/)

Focuses on all types of applications, not just web apps.
Has other vulnerabilities like Out-of-bounds writes/reads, race conditions, etc.

OWASP gives more credence to **risk**, SANS gives more credence to **prevalence** of each weakness.

## Tools

- [Fiddler](https://www.telerik.com/fiddler): Proxy tool to intercept and change requests.
- [Charles Proxy](https://www.charlesproxy.com/): Proxy tool to intercept and change requests.
- [HTTP Toolkit](https://httptoolkit.com/): Proxy tool to intercept and change requests.
- [Burp Suite](https://portswigger.net/burp): Proxy tool with security testing capabilities.
- [ZAP](https://www.zaproxy.org/): Zed Attack Proxy, formerly OWASP ZAP. Security testing tool.
- [OWASP WebGoat](https://owasp.org/www-project-webgoat/): Deliberately insecure web application for education.
- [OWASP Juice Shop](https://owasp.org/www-project-juice-shop/): Deliberately insecure web application for education with more training features and CTF challenges.

