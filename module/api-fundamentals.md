# API Security Fundamentals 2025  

* [API Security Fundamentals 2025](https://university.apisec.ai/products/api-security-fundamentals-2025)  

## API Intro  

>Governance, testing and monitoring  
>API expose data and logic flaws, and are often have over-permissioned.  
>API endpoints can be discovered even if they're not documented, such as mobile application.  
>PCI DSS 4.0, standard emphasizes API security, including identifying business logic abuse.  
>Attackers, Inspecting network traffic in the browser’s developer tools to identify APIs.  
>Business logic flaws makes API security testing critical.  

## Intro OWASP API Security Top 10  

1. Broken Object Level Authorization (BOLA)  
* UI is nog control for logic controls as direct API functions may allow malicious action to perform or obtain danagerous results  
2. Broken Authentication  
* No captacha, no rate limiting, brute forcing, no MFA, result in allowing unauthentication  
3. Broken Object Property Level Authorization (BOPLA)  
* Too much info revealed, less is better, and mass assignment to alter records  
4. Unrestricted Resource Consumption  
* Rate limiting, restricting connection access not preventing mass abuse unauthenticated  
5. Broken Function Level Authorization  
* allow access to dangerous function elevating roles or gaining paid services without control checks (RBAC)  
6. Unrestricted Access to Sensitive Business Flows  
* Bypass business condition logic checks to skip steps to exploit the meant flow  
7. Server Side Request Forgery (SSRF)  
* Sending API to go to malicious destination externally, cloud resources or internal systems  
8. Security Misconfiguration  
* server hardening, best practices, verbose error response, cookies, and authentication controls  
9. Improper Inventory Management  
* Life cycle of all API instances, version control, documented, regular audits, and decommission old endpoints  
10. Unsafe Consumption of APIs  
* Do not trust 3rd parties, zero trust, downstream supply chain breach impacting your APIs and request compliance reports from 3rd parties  
  
## Quiz Summary  

* What do "BOLA" attacks OWASP #1. target? Authorization flaws
* Which OWASP category addresses out-of-date API versions? Improper Inventory Management
* What real-world example illustrates BOLA in the OWASP Top 10? Peloton’s API exposing user data
* What does “Excessive Data Exposure” typically involve? Returning unnecessary or sensitive data via APIs  
* Which best practice addresses Excessive Data Exposure? Use data minimization techniques
* What vulnerability does rate limiting primarily help prevent? Unrestricted Resource Consumption
* What real-world example demonstrated Broken Function Level Authorization? Bumble allowing free-to-premium account upgrades
* How does SSRF typically exploit APIs? Manipulating user input to access unauthorized resources  
* What general best practice applies to all OWASP API vulnerabilities? Test APIs continuously  

----  

## API Attack Analysis  

>API attacks sorted by volume of breaches:  

1. Rate limiting (OWASP 4)
2. Broken Authorization (OWASP 1)
3. Broken Authentication (OWASP 2)
4. Excess data Exposure (OWASP 3)
5. Improper Inventory Management (OWASP 9)  

>Is API Security a priority to the business and investing in securing API endpoints?  

## Threat Modeling  

* Identify  
* Assess  
* Probability  
* Impact  
* Mitigtion  

>What is the hack value to the business crown jewels, pii, pci, customer data, health, etc.?  

>See [CISSP Study Notes](https://github.com/botesjuan/CISSP-Studies/blob/main/README.md)  

```
Risk = Threat  *  Vulnerability  *  Likelihood  *  Impact  
```  

* Why is rate limiting not a perfect defense against high-volume attacks?  
  * Attackers will keep requests below the rate-limit threshold
  * Attackers will distribute attacks across thousands of IP addresses
  * Attackers will use residential proxies  
* What percentage of API breaches are attributed to OWASP 1, 2, and 3 vulnerabilities?  
  * 90%  
* During a threat modeling exercise, which question should you ask first?  
  * What do we have that attackers want?  
* Why do attackers often target APIs?  
  * To access sensitive data like PII or corporate information  
* Which defense mechanism is most associated with preventing high-volume attacks, but is not perfectly effective?  
  * Rate limiting  
* In API security, why is “Broken Authorization” a critical issue?  
  * It allows unauthorized users to access other users’ data  

## Govern, Monitor & Test  

>Governance  
* Inventory, consistancy, standard, policies, documentation, formats, development life cycle and enforcing security  
>Monitoring  
* Runtime traffic, Threat detection, Incident response, Control validation, uncover Anomalies, WAF proactive and Reactive alerting with SIEM  
>Testing  
* Continuous automated Security, Data, and Logic testing through Positive amd negative fuzzing tests using tools like Postman, Burp, Zap, etc.  

* What is the primary goal of API governance? 
  * Enforcing consistent processes for API development and deployment
* Which of the following is NOT a key component of API documentation?  
  * User activity logs  
* What is a limitation of API security monitoring tools?  
  * They often lack context to distinguish malicious from legitimate requests  
* What is the most common API documentation format?  
  * OpenAPI Specification (OAS)  
* Why should API testing be continuous and automated?  
  * To detect vulnerabilities before deployment  
* Which approach allows for comprehensive and frequent API testing?  
  * Automated API security scanning  

## Application Security Technology Landscape  

>Definitions  

* SAST - static application security testing 
* DAST - dynamic application security testing
* CVEs - Common Vulnerability Entities  
* Secure design - shift left lifecycle with security in the room at initial  
* Development - code review
* Testing - Pentesting / bug bounty  
* Deployment - web server & api gateway
* Retirement - governance and depreciation  

* Which of the following technologies is NOT commonly associated with application security?  
  * Data Encryption technologies  
* Why are APIs less commonly exploited via vulnerabilities like SQL injection or cross-site scripting?  
  * Such vulnerabilities are generally easily detected and fixed  
* When should security considerations begin during the API lifecycle?  
  * At the API definition phase  
* Which of the following is most effective for finding API-specific vulnerabilities during the testing phase?  
  * Automated API-specific security testing  
* What is the recommended action when a new API version is published?  
  * Update the version and retire older versions  
* What tool is commonly used during runtime to detect and block API attacks?  
  * web app firewall (WAF)  
  
## API Best Practices  

>Summary of key questions to secure API endpoints:  

* What percentage of respondents in a Nokia/RapidAPI survey perform security testing on their APIs?  
  * 4%
* What is a significant difference between traditional cyberattacks and API attacks?
  * API attacks require more technical expertise than traditional cyberattacks  
* What is the top API vulnerability in the OWASP API Security Top 10?  
  * Broken Object Level Authorization  
* Which principle is emphasized to mitigate BOLA risks?
  * Enforce least privilege
* Which vulnerability involves modifying or escalating privileges improperly?  
  * Broken Function Level Authorization  
* What is an example of Security Misconfiguration?  
  * Providing overly detailed error messages
  * Misusing encryption keys
  * Failing to validate inputs  
* What risk does Unsafe API Consumption address?  
  * Assuming 3rd party APIs are secure  
* Why is it important to train developers on API security issues?  
  * Developers can create more secure and resilient APIs  
* What is a common tactic attackers use to bypass rate-limiting defenses?  
  * Distributing attacks across many IP addresses  
* What is the first step in a threat modeling process?  
  * Identify the API attack surface and vulnerabilities  
* What are the three pillars of API security University?  
  * Governance, Monitoring, Testing  
* Which tool is helpful for enforcing API governance?  
  * API Gateway  
* What is an example of a logic vulnerability in APIs?  
  * Cross-account access  
* What is a best practice for using 3rd party APIs?  
  * Always validate the data being returned  
* What is a strategy for mitigating risks associated with unknown APIs?  
  * Leverage the WAF to discover APIs in use   
* Why is it important to incorporate security “as far left” as possible in the application lifecycle?  
  * To prevent vulnerabilities before deployment  
* What is the primary focus of attackers when targeting APIs?  
  * Logic flaws and authorization gaps  
* How can you prevent Server-Side Request Forgery?  
  * Only process URLs that pass validity checks  
* What is one major benefit of maintaining up-to-date API documentation?  
  * Enhanced security and usability  
* How should you handle user inputs?  
  * Reject any inputs that do not meet expectations  
  
----  
