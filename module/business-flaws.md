# Unrestricted Access to Sensitive Business Flows  

>[API6:2023 Unrestricted Access to Sensitive Business Flows](https://university.apisec.ai/products/owasp-api-security-top-10-and-beyond/categories/2152492181/posts/2166899017)  

>Unrestricted Access to Sensitive Business Flows represents the risk of an attacker being able to identify and exploit API-driven workflows.  
>An attacker will be able to leverage an organization's API request structure to obstruct other users.  
>This obstruction could come in the form of spamming other users, depleting the stock of highly sought-after items,  
>or preventing other users from using expected application functionality.  

## OWASP Preventative Measures  

* Identify the business flows that might harm the business if they are excessively used  
* Engineering, choose the right protection mechanisms to mitigate the business risk  

>Some of the protection mechanisms methods are used to slow down automated threats:  

* Device fingerprinting: denying service to unexpected client devices (e.g headless browsers) tends to make threat actors use more sophisticated solutions
* Human detection: using either captcha or more advanced biometric solutions (e.g. typing patterns)  
* Non-human patterns: analyze the flow to detect non-human action, e.g. the user accessed the "add to cart" and "complete purchase" functions in less than one second
* Consider blocking IP addresses of Tor exit nodes and well-known proxies
* Secure and limit access to APIs that are consumed directly by machines (such as developer and B2B APIs). Targets for attackers because they don't implement all the required protection  

>Additional Resources:  

* API10:2019 Insufficient Logging & Monitoring  

----  

>What is Unrestricted Access to Sensitive Business Flows about?  

* It's about the risk of an attacker identifying and exploiting API-driven workflows.  

>What is a possible way to detect non-human patterns during the exploitation of Unrestricted Access to Sensitive Business Flows?  

* Analyze the user flow to detect actions that happen too quickly for a human user.  

>Which of the following is NOT a recommended preventative measure against Unrestricted Access to Sensitive Business Flows?  

* Allow unlimited access to APIs that are consumed directly by machines.

>Which of the following is an impact of an attacker exploiting an API with Unrestricted Access to Sensitive Business Flows?  

* It can hurt the business by preventing legitimate users from purchasing a product.  

>What kind of requests does an attack under Unrestricted Access to Sensitive Business Flows usually involve?  

* Requests that are individually legitimate and unidentifiable as an attack.  

----  
