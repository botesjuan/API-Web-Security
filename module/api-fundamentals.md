# API Security Fundamentals 2025  

* [API Security Fundamentals 2025](https://university.apisec.ai/products/api-security-fundamentals-2025)  

## Intro  

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
* 
10. Unsafe Consumption of APIs  
* 
  
## 

