# Injection  

>[Injection - Beyond the Top 10](https://university.apisec.ai/products/owasp-api-security-top-10-and-beyond/categories/2152492281/posts/2166899129)  

>Injection vulnerabilities take place when an attacker is able to send commands that are executed by the systems that support the web application.  
>Common injection attacks are SQL injection, Cross-site scripting (XSS), and operating system command injection (OS CMD).  

## Injection Preventative Measures  

* Keeping data separate from commands and queries  
* Perform data validation using a single, trustworthy, and actively maintained library.  
* Validate, filter, and sanitize all client-provided data, or other data coming from integrated systems.  
* Special characters should be escaped using the specific syntax for the target interpreter.  
* Prefer a safe API that provides a parameterized interface.  
* Always limit the number of returned records to prevent mass disclosure in case of injection.  
* Validate incoming data using sufficient filters to only allow valid values for each input parameter.  
* Define data types and strict patterns for all string parameters.  

>Additional Resources:  

* CWE-20: Improper Input Validation
* CWE-200: Exposure of Sensitive Information to an Unauthorized Actor
* CWE-319: Cleartext Transmission of Sensitive Information  

----  

## Insufficient Logging and Monitoring  

>Logging and monitoring are necessary and important layer of API security. 
>In order to detect an attack against an API an organization must have monitoring in place (SIEM).  

>OWASP 2019 Preventative Measures:  

* Log all failed authentication attempts, denied access, and input validation errors.
* Logs should be written using a format suited to be consumed by a log management solution and should include enough detail to identify the malicious actor.
* Logs should be handled as sensitive data, and their integrity should be guaranteed at rest and transit.
* Configure a monitoring system to continuously monitor the infrastructure, network, and API functioning.
* Use a Security Information and Event Management (SIEM) system to aggregate and manage logs from all components of the API stack and hosts.
* Configure custom dashboards and alerts

## Business Logic Flaws  

>Business logic vulnerabilities are weaknesses within applications that are unique to the policies and features of a given API provider.  
>The exploitation of business logic takes place when an attacker leverages misplaced trust or features of an application against the API.  

>Preventative Measures:  

* Threat Modeling Approach: Understand the business processes and workflows your API supports. Identifying threats, weaknesses, and risks during the design phase
* Reduce trust relationships with users, systems, or components. Business logic vulnerabilities is used to exploit trust relationships  
* Regular training can help developers to understand and avoid business logic vulnerabilities. 
* Training should cover secure coding practices, common vulnerabilities, and how to identify potential issues during the design and coding phases.  
* Implement a bug bounty program  

>Additional Resources:  

* CWE-840: Business Logic Errors  

----  

>What is the most common area where injection flaws are often found?  

* Within input parameters that are expected to be sent to an interpreter  

>When is an API considered to be vulnerable to injection flaws?  

* If client-supplied data is directly used or concatenated to SQL/NoSQL/LDAP queries, OS commands, XML parsers, and ORM/ODM without validation.  

>What is a key prevention strategy for injection flaws in APIs?  

* Keep data separate from commands and queries, and ensuring data validation using a reliable library.  

>Which of the following is NOT a vulnerability in terms of API logging and monitoring?  

* Log integrity is guaranteed.  

>What are the key measures to prevent the misuse of APIs due to lack of sufficient logging and monitoring?  

* Log all failed authentication attempts, denied access, and input validation errors.
* Use a Security Information and Event Management (SIEM) system to aggregate and manage logs from all components of the API stack and hosts.
* Configure custom dashboards and alerts, enabling suspicious activities to be detected and responded to earlier.  

>Which of the following can be considered a business logic vulnerability?  

* An application that allows trusted end users to upload and execute code without restrictions  

>A rule of thumb to identify business-logic vulnerabilities is:  

* Understanding business functions and how a feature could be leveraged in an attack  

>According to OWASP, which vulnerability is still an unsolved problem, but is more relevant to the 2019 OWASP API Security Top 10?  

* Injection  

>Which of the vulnerabilities beyond the 2023 OWASP Top 10 is the most difficult to identify generically?  

* Business Logic Flaws  

>Which of the vulnerabilities beyond the 2023 OWASP Top 10 will most likely lead to user-supplied code being executed?  

* Injection  

----  

![owasp-api-security-top-10-and-beyond-30-lessons.png](/images/owasp-api-security-top-10-and-beyond-30-lessons.png)  
