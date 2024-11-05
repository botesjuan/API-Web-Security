# Security Misconfiguration  

>[API8:2023 Security Misconfiguration](https://university.apisec.ai/products/owasp-api-security-top-10-and-beyond/categories/2152492174/posts/2166898954)  

>Security Misconfiguration represents a catch-all for many vulnerabilities related to the systems that host APIs.  
>When an API's security is misconfigured it can be detrimental to the confidentiality, integrity, and availability.  

## OWASP Preventative Measures  

>API life cycle should include:  

* Repeatable hardening process leading to fast and easy deployment of a properly locked down environment  
* Task to review and update configurations across the entire API stack. Review include: orchestration files, API components, and cloud services (S3 bucket permissions)  
* Automated process to continuously assess the effectiveness of the configuration and settings in all environments  
* API communications from the client to the API server and any downstream/upstream components happen over an encrypted communication channel (TLS) internal or public-facing  
* Set of HTTP verbs each API can be accessed by: all other HTTP verbs should be disabled (e.g. HEAD)  
* APIs accessed from browser-based clients should, implement Cross-Origin Resource Sharing (CORS) policy
* Include applicable Security Headers
* Restrict incoming content types/data formats to those that meet the business/ functional requirements.  
* All servers in chain (e.g. load balancers, reverse and forward proxies, and back-end servers) process incoming requests in a uniform manner to avoid desync issues  
* Define and enforce all API response payload schemas, including error responses.  

>Additional Resources:  

* CWE-2: Environmental Security Flaws
* CWE-16: Configuration
* CWE-209: Generation of Error Message Containing Sensitive Information
* CWE-319: Cleartext Transmission of Sensitive Information
* CWE-388: Error Handling
* CWE-444: Inconsistent Interpretation of HTTP Requests ('HTTP Request/Response Smuggling')
* CWE-942: Permissive Cross-domain Policy with Untrusted Domains  

----  

>Which of the following is a potential outcome of having a security misconfiguration in an API?  

* Disclosure of sensitive user data
* Full server compromise due to revealed system details  

>What can happen if Transport Layer Security (TLS) is missing in an API?  

* Attackers can intercept and read the API data being communicated over the network  

>Which of the following is NOT a security misconfiguration?  

* Exposing an unsupported version of the API  

>Which of the following is ***NOT*** a recommended measure for preventing API security misconfigurations?  

* Allow all HTTP verbs for each API  

>How can security misconfigurations in an API be detected?  

* Through web application vulnerability scanners like Burp Suite, Nessus, Qualys, OWASP ZAP, and Nikto
* Manually, by inspecting the headers, SSL certificate, cookies, and parameters  

----  
