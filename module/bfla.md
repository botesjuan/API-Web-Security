# Broken Function Level Authorization  

>[API5:2023 Broken Function Level Authorization](https://university.apisec.ai/products/owasp-api-security-top-10-and-beyond/categories/2152492169/posts/2166898626)  

>BFLA Scenario, a fintech API susceptible to BOLA would allow an attacker the ability to see what is in the bank account of another user,   
>while the same API vulnerable to BFLA would allow an attacker to transfer funds from other users' accounts to their own.  

## OWASP Preventative Measures  

* Enforcement mechanisms should deny all access by default, requiring explicit grants to specific roles for access to any function.
* Review API endpoints against function level authorization flaws, while keeping in mind the business logic of the application and groups hierarchy.
* Administrative controllers inherit from an administrative abstract controller that implements authorization checks based on the user's group/role.
* Admin functions inside a regular controller implement authorization checks based on the user's role.  

>Additional Resources:

* CWE-285: Improper Authorization

----  

>According to the OWASP API, which of the following is NOT a recommended preventative measure for Broken Function Level Authorization?  

* Make sure that all administrative functions inside a regular controller do not implement authorization checks based on the user's group and role    

>When testing for BFLA vulnerabilities, what functionality should you look for that could be exploited?  

* Functionality that allows a user to make an unlimited number of requests to the API  
* Functionality that allows a user to add themselves to any user group  
* Functionality that allows a user to change their own user name or password     

>What is the primary impact of exploiting a Broken Function Level Authorization vulnerability according to OWASP?

* An attacker could access unauthorized functionality, with administrative functions being key targets.  

>According to OWASP, what makes implementing proper authorization checks challenging in modern applications?  

* Modern applications have complex user hierarchies and many types of roles or groups.  

>What is the primary threat associated with Broken Function Level Authorization in API Security?  

* An attacker could gain unauthorized access to certain endpoints they should not have access to.  

----  
