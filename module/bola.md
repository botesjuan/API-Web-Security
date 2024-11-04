# BOLA  

>[API1:2023 Broken Object Level Authorization](https://university.apisec.ai/products/owasp-api-security-top-10-and-beyond/categories/2152491878/posts/2166865832)  

>API users should only have access to sensitive resources that belong to them.  
>When BOLA is present an attacker will be able to access the sensitive data of other users.  

>OWASP Preventative Measures

* Implement a proper authorization mechanism that relies on the user policies and hierarchy.
* Authorization mechanism check if the logged-in user has access to perform requested action on the record in every function input from the client to access the database
* Prefer the use of random and unpredictable values as GUIDs for records' IDs.
* Write tests to evaluate the vulnerability of the authorization mechanism.  

## Additional Resources

* CWE-284: Improper Access Control
* CWE-285: Improper Authorization
* CWE-639: Authorization Bypass Through User-Controlled Key  

----  

>What does BOLA stand for in the context of API security?

* Broken Object Level Authorization  

>In the given example, what demonstrates a BOLA vulnerability?

* Bruce was able to access another user's information (Harvey Dent's) using his own authorization.

>According to the course content, which statement about BOLA is true?  

* BOLA vulnerabilities are common, easily exploitable, and often require minimal technical skills to discover.  

>If an API provider does not have sufficient access controls, what's the worst that can happen?  

* The API can allow a user to obtain another user's resources via API requests.  

----  
