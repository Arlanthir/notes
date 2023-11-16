# Cybersecurity

## Introduction

The goal of cybersecurity is to protect the **confidentiality**, **integrity** and **availability** of *assets*.

- **Confidentiality**: Information is only available to those who should have access (e.g. by encryption).
- **Integrity**: Data is known to be correct and trusted (e.g. by hashes).
- **Availability**: Information is available for use by legitimate users when it is needed (e.g. by redundancy).

**Asset** is anything that either:
- Holds value (e.g. data, protected by maintaining its integrity).
- Produces value (e.g. server, protected by maintaining its availability).
- Provides access to value (e.g. password, protected by maintaining its confidentiality).

About threats:
- **Vulnerability**: weakness that makes an asset susceptible to attack or failure.
- **Threats**:
  - **Attack**: intentional action to reduce the value of an asset.
  - **Failure**: unintentional action that can reduce the value of an asset.

## OWASP Top 10 (2021)

https://owasp.org/www-project-top-ten/

The top 10 most common vulnerabilities in web apps. New list comes out every three years, give or take.

1. Broken Access Control
2. Cryptographic Failures
3. Injection
4. Insecure Design
5. Security Misconfiguration
6. Vulnerable and Outdated Components
7. Identification and Authentication Failures
8. Software and Data Integrity Failures
9. Security Logging and Monitoring Failures
10. Server-Side Request Forgery (SSRF)


## Tools

- [OWASP WebGoat](https://owasp.org/www-project-webgoat/): Deliberately insecure web application for education.
