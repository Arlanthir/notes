# Cybersecurity

## Introduction

The goal of cybersecurity is to protect the **confidentiality**, **integrity** and **availability** (CIA) of *assets*.

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

- **Vulnerability**: Weakness that makes an asset susceptible to attack or failure.
- **Threats**:
  - **Attack**: Intentional action to reduce the value of an asset.
  - **Failure**: Unintentional action that can reduce the value of an asset.
- **Exploit**: Code written to take advantage of a vulnerability.

Threat actors:
1. Script kiddies: Low skill, simple attacks, motivated by revenge or fame.
2. Hacktivists: Moderate to high skill, looking to make an example of an organization, motivated by activism.
3. Hackers: High skill, looking to understand how things work.
4. Cyber criminals: High skill, looking for financial exploits (e.g. ransomware).
5. Advanced persistent threat: Very high skill, backed by states, looking to weaken a political adversary, motivated by national interest.

### Data protection

- **PII**: Personally Identifiable Information
- **PHI**: Protected Health Information (subset of PII)
- **SSI**: Sensitive Financial Information (subset of PII)
- Data at rest: databases and web servers.
- Data in motion: data sent in requests.
- Data in use: data in device memory.

Generally protected by encryption.

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

1. **Broken Access Control (Authorization)**  
   Users can access privileged functions / resources they shouldn't have access to.
2. **Cryptographic Failures**  
   Transmitting data in cleartext or weak encryption, HTTP instead of HTTPS.
3. **Injection**  
   User-changed inputs that make the system act in unintended ways (e.g. SQL Injection).
4. **Insecure Design**  
   Lack of threat modeling and other security controls before coding, such as analyzing user stories for failure flows.  
   Example of poor design: insecure password recovery by easy questions.
5. **Security Misconfiguration**  
   Poor configuration of web servers allows attackers to exploit them (e.g. default passwords).
6. **Vulnerable and Outdated Components**  
   Lack of checking/auditing for known vulnerabilities in dependencies.
7. **Identification and Authentication Failures**  
   Weak passwords, lack of MFA, unhashed passwords, vulnerability to brute-force, phishing, etc.
8. **Software and Data Integrity Failures**  
   Not verifying signatures of software (e.g. dependencies) that is put into production.  
   E.g. SolarWinds distributed a malicious update tampered with by a nation-state (suspected to be Russia).
9. **Security Logging and Monitoring Failures**  
   Not logging or not reviewing the logs, for instance in a Security Information and Event Management (SIEM) system.  
   Probing should trigger alerts. Alerts should be sent to a SOC (Security Operations Center).
10. **Server-Side Request Forgery (SSRF)**  
    Application allows the user to provide external endpoints that are called (e.g. image URLs).  
    Typically attackers send the application to `localhost` to probe the local machine's vulnerabilities or
    access resources directly without being stopped by a firewall.

#### [OWASP API Security Top 10 (2023)](https://owasp.org/API-Security/editions/2023/en/0x11-t10/)

Specific for APIs

1. **Broken Object Level Authorization**  
   Change ID of a request and access a resource you shouldn't have access to.
2. **Broken Authentication**  
   It's possible to impersonate another user.
3. **Broken Object Property Level Authorization**  
   Combines Excessive Data Exposure (serving more data than the client needs, relying on the client to filter it.) and
   Mass Assignment (blindly storing objects sent by the client without filtering for allowed properties).
4. **Unrestricted Resource Consumption**  
   Vulnerability to DDoS or mass requests that cause expenses (e.g. SMS 2FA).
5. **Broken Function Level Authorization**  
   Relying on client to "hide" calls to admin functionality.
6. **Unrestricted Access to Sensitive Business Flows**  
   Exposing flows that are sensitive when automated, such as buying all tickets to a concert to resell.
7. **Server Side Request Forgery (SSRF)**  
   Application allows the user to provide external endpoints that are called (e.g. image URLs).
8. **Security Misconfiguration**  
   Poor configuration of API servers allows attackers to exploit them (e.g. default passwords).
9. **Improper Inventory Management**  
   Attacker leverages old API endpoints that are left unmanaged or unpatched.
10. **Unsafe Consumption of APIs**  
    Trusting third-party APIs more than user input, allowing attackers to exploit the APIs in order to attack the system.

[OWASP Cheat Sheets](https://cheatsheetseries.owasp.org/index.html)

### [SANS Top 25](https://www.sans.org/top25-software-errors/)

Focuses on all types of applications, not just web apps.
Has other vulnerabilities like Out-of-bounds writes/reads, race conditions, etc.

OWASP gives more credence to **risk**, SANS gives more credence to **prevalence** of each weakness.

## Supply Chain Security

Practices and measures to ensure the integrity and security of software throughout its development.

Attackers can target a dependency of the software, effectively getting the organization to pull the attack and bypass the organization's outer defenses. They can also target many organizations this way.

- **Provenance**: origin of a software component.
- **Pedrigree**: individuals and organizations involved in the development of the software.
- **CAPEC**: Common Attack Pattern Enumeration and Classification, catalog of common attack patterns.
  CAPEC-437 relates to the supply chain.
- **CNCF**: Cloud Native Computing Foundation offers a listing of possible supply chain compromise types.

### Mitigation

- Check digital signatures of dependencies.
- Group vendors into risk profiles.
- Identify weakest areas in supply chain and work with vendors to improve their security.
- Assess safety of hardware and software supplied to your organization.
- Manage remote work endpoints.
- Continuously monitor third-party risks.

### SLSA - Supply Chain Levels for Software Artifacts

Framework to enhance the security and trustworthiness of sofware supply chains.
- Level 1: Build process must be fully scripted.
- Level 2: Using version control and hosted build service.
- Level 3: Source and build platforms meet specific standards to guarantee the auditability of the source and integrity of the provenance.
- Level 4: Two-person review of all changes and reproducible build process.

### SBOM - Software Bill Of Materials

Formal, machine-readable inventory of software components and dependencies.
- Different types of SBOMs: Design, Source, Build, Analyzed, Deployed and Runtime.

Minimum elements of an SBOM:
- Supplier name
- Component name
- Component version
- Other unique identifiers to identify the component in relevant databases
- Dependency relationship
- Author of SBOM data
- Timestamp of SBOM data

Most common formats:
- Software Package Data eXchange (SPDX)
- CycloneDX 1.3
- Software Identification (SWID) tag

### Dependency-Track

Software Composition Analysis (SCA) tool by OWASP to identify and manage software dependencies.  
You can import a CycloneDX SBOM into Dependency-Track.

It integrates with multiple sources of vulnerability intelligence:
- Internal
- Sonatype OSS Index 
- VulnDB
- Snyk

Offers Exploit Prediction Scoring System (EPSS), helps organizations focus on exploits more likely to be exploited.

## Cloud and Container Security

Cloud "as a Service":
- Infrastructure as a Service (IaaS): Provides virtual machines, storage, networking.
- Platform as a Service (PaaS): Offers platform and environment to build, deploy, and manage applications without managing infrastructure.
- Software as a Service (SaaS): Delivers applications over the internet using a subscription model.

### Shared Responsibility Model

Responsibility is shared between Cloud Service Provider (CSP) and customer.

CSP is responsible for:
- Physical security
- Network infrastructure
- Hypervisor security
- Host operating system
- Service availability

Customer is responsible for:
- Data security
- Identity and Access Management (IAM)
- Application security
- Configuration management
- Patch management
- Data backup and recovery

### Security issues in the cloud

- Data breaches/loss: e.g. misconfigured S3 bucket.
- Lack of control: CSP handles security controls that the customer cannot manage.
- Compliance and regulatory issues: CSPs may not meet specific industry or regional compliance requirements.
- Shared resources: concerns about data isolation.
- Insider misuse: concerns over employees of the CSP having access to your data.
- Virtualization vulnerabilities: possible weaknesses in virtualization technologies.
- Data residency and sovereignty: concerns over the geographical location of stored data.
- CSP reliability: concerns over the availability of the service.

### AWS Security Pillar

AWS Well-Architected Framework has 6 pillars, including Security.

Fundamentals of the Security pillar:

#### Identity and Access Management

- **Account**: individual entity with an isolated set of AWS resources and permissions.  
  It's the root entity under which others such as users, groups, and roles are created and managed.  
  Each account has a unique ID and is billed separately from other accounts.
- **User**: individual entity within an AWS account with specific credentials.  
  Each user can have different IAM **policies** that allow interaction with AWS resources.
- **Group**: collection of IAM users. Allows applying the same set of permissions to multiple users.
- **Role**: similar to users but without permanent credentials.
  Temporary access for trusted entities, like users from other accounts, or applications.

**Policy**: JSON file with:
- Version: IAM policy language version.
- Statement: array of statements, each with:
  - Effect: `"Allow"` or `"Deny"`.
  - Action: specifies the service and action, e.g. `"s3.GetObject"`.
  - Resource: Amazon Resource Name (ARN) of the resource to which the permission applies, e.g. `"arn:aws:s3:::example-bucket/*"`.

#### Detection

Monitoring and auditing:

- CloudWatch: unified view of AWS resources and applications. Has Alarms to notify when certain thresholds are breached (CPU, network, storage).
- CloudWatch Logs: collects and stores log files from AWS resources, including EC2, Lambda functions, and more.
- CloudTrail: log of AWS API calls, including information about who made the call and when.
- Config: track and record changes to resources.
- VPC Flow logs: monitor network traffic in the VPC.
- GuardDuty: export active findings to CloudWatch Events.

#### Infrastructure protection

- Web Application Firewall (WAF).
- Regions and Availability Zones for fault-tolerance and high availability.
- Local Zones: extensions of Regions to provide select services for low-latency applications.
- Outposts: on-premises.
- Layered networks (not using a single VPC) so that compromising a part of the network does not compromise the full network.

#### Data protection

- Data classification: sensitivity levels
- Data encryption

Encryption offered:
- Key Management Service (KMS): Storing encryption keys to secure data at rest.
- Simple Storage Service (S3): Built-in encryption for stored files in buckets.
- Relational Database Service (RDS): Supports encryption via KMS.
- Elastic Block Store (EBS): Container volumes. Supports encryption via KMS.

Data protection in transit:
- PrivateLink: Secure private network connections.
- CertificateManager (ACM): SSL/TLS certificates.
- Virtual Private Cloud (VPC): Create private networks.
- Network Load Balancer (NLB) and Application Load Balancer (ALB) can establish secure connections to backend instances.
- Direct Connect and VPN: Secure connections between on-premises and VPCs.
- CloudFront: Content Delivery Network (CDN) with SSL/TLS support.
- Transit Gateway: Interconnect thousands of VPCs and VPNs.

#### Incident response

Besides the audit tools mentioned before:
- Security Hub: Aggregates security findings from services.
- Systems Manager: Manage and configure resources, including automation for patching and compliance.
- Incident Manager: Collaboration tools, automated response actions and tracking progress.
- Detective: Analyze and identify root cause of security findings.

#### Application security

AppSec stages:
1. Start with threat modeling.
2. Implement automated testing.
3. Perform penetration testing.
4. Conduct manual code reviews.
5. Centralize services for packages/dependencies.
6. Deploy programatically.
7. Assess security properties regularly.
8. Embed security ownership in teams.

Services:
- CodeGuru: automate code reviews.
- CodeArtifact: centralized control of packages/dependencies.
- CloudFormation: deploy software and maintain consistency.
- CodePipeline and CodeBuild: automated and secure software deployment.

#### Container security

Issues:
- Container sprawl: containers are easy to create and scale. As the number increases, so does the difficulty to manage and patch.
- Image vulnerabilities: containers can be built from vulnerable images.
- Shared kernel: vulnerabilities in kernel can affect multiple containers.
- Orchestration complexity: platforms like Kubernetes introduce additional complexities and opportunity for misconfiguration.
- Microservices communication: more communication channels to secure.
- Short lifecycle: rapid pace of change makes it difficult to maintain consistent security policies.
- Monitoring and visibility: traditional tools may not be well-suited to containers.
- Insider threats: inside access to host may compromise all containers.
- Resource constraints: strong security measures may degrade performance or scalability.

Mitigation:
- Keep containers updated.
- Principle of least privilege when defining permissions
- Use secure container registries
- Network segmentation
- Container-specific monitoring tools
- Scan container images for known vulnerabilities

## Defenses and tools

[Zed Attack Proxy](https://www.zaproxy.org/): dynamic scanner, allows automated scanning for vulnerabilities of a live site.
1. Crawling / spidering: finding all pages of the site.
2. Include pages in Contexts to setup authentication and other settings.
3. Active scan: try a number of automated attacks.

### Cross-site scripting (XSS)

Attackers inject a script in a website.
- Reflected: The attacker discovers a URL param that injects a script, creates malicious links to send to users.
- Persistent: The attacker saves an embedded script in a vulnerable website (e.g. through forum attachments).

### Content security policy (CSP)

HTTP header that controls security policies. Can mitigate XSS.

- A policy should include `default-src` policy directive to fallback for other resource types without policies.
- A policy should include a `script-src` (or `default-src`) directive to prevent inline scripts and `eval()` .
- A policy should include a `style-src` (or `default-src`) directive to prevent inline styles.

List of 1.0 directives:
- `connect-src`: URLs that can be contacted (e.g. through fetch / XMLHttpRequest)
- `font-src`: From where fonts can be loaded.
- `img-src`: From where images can be loaded.
- `media-src`: From where video, audio and text tracks can be loaded.
- `object-src`: From where plugins can be loaded.
- `script-src`: From where scripts can be loaded.
- `style-src`:  From where styles can be loaded.
- `default-src`: Fallback for when a more specific directive is not defined.
- `frame-src`: (Deprecated) From where frames can be loaded.
- `report-uri`: Specifies a URL to which the browser sends reports about policy violation.
- `sandbox`: Sandbox policy to apply to the protected resource.

Additions in CSP 2.0:
- `form-action`: Allowed HTML form action destinations.
- `frame-ancestors`: Allowed sources of frame, iframe, object, embed and applet.
- `plugin-types`: Restricts which type of resources can be embedded.
- `base-uri`: Restricts URLs that can specify the document base URL.
- `child-src`: Governs the creation of nested browsing and Worker execution contexts.

E.g.:
```
Content-Security-Policy: default-src 'none'; script-src 'self'; connect-src 'self'; img-src 'self'; style-src 'self';
```

CSPs can also be defined using [<meta> tags](https://developer.mozilla.org/en-US/docs/Web/HTTP/CSP).

You can analyze CSPs with [CSP Evaluator](https://csp-evaluator.withgoogle.com/).

### Security Models

Access Control Models:
- Access Control List: List of permissions attached to an object that define which users or systems have access and can do each operation.
- Bell-LaPadula: Formal state transition model with security labels ("Top Secret" to "Public").
- Role-based Access Control (RBAC): Roles group permissions and can be attributed to users.
- Access-based Access Control: Boolean logic rules to determine if access is granted.

Integrity model:
- Biba Integrity Model: Users can only create content to be read by their integrity level and below. Users can only read content at their integrity level of above.
- Clark-Wilson Model: Uses access control tiples of subject/program/object. Programs are transactions.

Information flow model:
- Brewer-Nash model (Chinese wall)
- Data flow diagrams
- Use case models
- Assurance models

### [Security Knowledge Framework](https://www.securityknowledgeframework.org/) (SKF)

Developed by OWASP, web-application that uses the OWASP Application Security Verification Standard and contains code examples and information to help developers. You can host it and change the code examples for your organization.

Also includes Labs with practical lessons on different attacks.

- Pre-development: detect threats and provide development patterns.
- Post-development: checklists to evaluate security.

### Secure code review

Code should be reviewed with Subject Matter Experts (SMEs).
- Pre-commit: insecure code is not merged, slows down integrations.
- Post-commit: insecure code is merged, faster delivery but higher risk.
- Audit: can be triggered by a security event and should review the entire area of concern, not just a single commit.

## Session management

There are both:
- Pre-authenticated session (e.g. maintaining preferred language of anonymous user).
- Post-authenticated session (e.g. managing access controls).

The ID/token of a post-authentication session is temporarily equivalent to the strongest authentication method used by the application (e.g. replaces user/name password and OTP for the duration of the session).

Federateed identity: a single token is trusted accross multiple systems or organizations (SSO).

### JSON Web Token

Open standard that defines a digitally-signed compact way for securely transmitting information as a JSON object.

- Authorization: The JWT includes the routes, services or resources that are permitted with it.
- Information exchange: The JWT is signed and hashed, assuring identity of the sender and integrity of the information.

JWTs are created by the server but saved locally in the client and resent in every request (e.g. in the Authorization header).

JWTs consist of three parts:
- Header: type (JWT) and hashing algorithm (e.g. SHA256).
- Payload: Claims, i.e., the statements about the user entity.
- Signature: Header and payload signed with the server private key, using the header algorithm.

Example:

```
eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJzdWIiOiIxMjM0NTY3ODkwIiwibmFtZSI6IkpvaG4gRG9lIiwiaWF0IjoxNTE2MjM5MDIyfQ.SflKxwRJSMeKKF2QT4fwpMeJf36POk6yJV_adQssw5c
```

which decoded is:

```
{
  "alg": "HS256",
  "typ": "JWT"
}
{
  "sub": "1234567890",
  "name": "John Doe",
  "iat": 1516239022
}
HMACSHA256(base64UrlEncode(header) + "." + base64UrlEncode(payload), serverSecret)
```

Claims can be:
- Registered: predefined recommended interoperable claims, such as `iss` (issuer) and  `exp` (expiration time).
- Public: Defined in IANA JSON Web Token Registry or with a collision resistant namespace.
- Private: Custom claims to share information between parties that agree on them.

### OAuth

Used for access delegation, i.e. Google can permit users to share account information with third parties without sharing their password.

Access includes allowed scopes and denied scopes (granular access to resources).

Tokens:
- Access token: short-lived, can't be revoked.
- Refresh token: long-lived, used to get new access tokens, revokable.

Usually represented as JWTs, but are format-independent.

### OpenID

Standardized SSO by non-profit OpenID Foundation.


## Tools

- [Fiddler](https://www.telerik.com/fiddler): Proxy tool to intercept and change requests.
- [Charles Proxy](https://www.charlesproxy.com/): Proxy tool to intercept and change requests.
- [HTTP Toolkit](https://httptoolkit.com/): Proxy tool to intercept and change requests.
- [Burp Suite](https://portswigger.net/burp): Proxy tool with security testing capabilities.
- [ZAP](https://www.zaproxy.org/): Zed Attack Proxy, formerly OWASP ZAP. Security testing tool.
- [OWASP WebGoat](https://owasp.org/www-project-webgoat/): Deliberately insecure web application for education.
- [OWASP Juice Shop](https://owasp.org/www-project-juice-shop/): Deliberately insecure web application for education with more training features and CTF challenges.
- [CSP Evaluator](https://csp-evaluator.withgoogle.com/): analyze Content Security Policies.

