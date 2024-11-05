# API10:2023 Unsafe Consumption of APIs  

>[API10:2023 Unsafe Consumption of APIs](https://university.apisec.ai/products/owasp-api-security-top-10-and-beyond/categories/2152492264/posts/2166899119)  

>Unsafe Consumption of APIs focuses less on the risks of being an API provider and more on the API consumer.  
>Unsafe consumption is a trust issue. When an application is consuming the data of third-party APIs it should treat with zero trust to user input.  
>No trust, data consumed from third-party APIs should be treated with same security standards as end-user-supplied input.  
>When Third-party API provider is compromised then that insecure API connection back to the consumer becomes a vector for the attacker to leverage.  

## OWASP Preventative Measures  

* When evaluating service providers, assess their API security posture.  
* Ensure all API interactions happen over a secure communication channel (TLS).  
* Always validate and properly sanitize data received from integrated APIs before using it.  
* Maintain an allowlist of well-known locations integrated APIs, do not blindly follow redirects.  

>Additional Resources:  

* CWE-20: Improper Input Validation
* CWE-200: Exposure of Sensitive Information to an Unauthorized Actor
* CWE-319: Cleartext Transmission of Sensitive Information  

----  

>How does OWASP describe the risk regarding Unsafe Consumption of APIs?  

* Developers will tend to adopt weaker security standards  

>What is a potential risk when an API interacts with other APIs over an unencrypted channel?  

* Exposure of sensitive information due to cleartext transmission  

>Why is it important to maintain an allow list of well-known locations integrated APIs may redirect to?  

* To avoid blindly following redirects and potential security threats  

>What is a crucial step to ensure the security of data received from integrated APIs?  

* Validate and properly sanitize data before using it  

>What could be a potential outcome of weaker security standards in API integrations?  

* Exposure of sensitive information to unauthorized actors  

----  
