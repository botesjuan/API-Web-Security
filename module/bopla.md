# Broken Object Property Level Authorization  

>[BOPLA - API3:2023 Broken Object Property Level Authorization](https://university.apisec.ai/products/owasp-api-security-top-10-and-beyond/categories/2152491880/posts/2166898703)  

>Broken Object Property Level Authorization (BOPLA) is a combination of Mass Assignment and Excessive Data Exposure.  

>Excessive data exposure is when an API endpoint responds with more information than is needed to fulfill a request.  

>Mass assignment occurs when an API consumer includes more parameters in its requests than the application intended  
>and the application adds these parameters to code variables or internal objects. In this situation,  
>a consumer may be able to edit object properties or escalate privileges.  

## OWASP Preventative Measures  

* When exposing an object using an API endpoint, always make sure that the user should have access to the object's properties  
* Avoid using generic methods such as `to_json()` and `to_string()`. Instead, select specific object properties you specifically want to return.  
* Avoid using functions that automatically bind a client's input into code variables, internal objects, or object properties ***Mass Assignment***  
* Allow changes only to the object's properties that should be updated by the client.  
* Implement a schema-based response validation mechanism as an extra layer of security. Define and enforce data returned by all API methods.  
* Keep returned data structures to the bare minimum  

----  

>Additional Resources:  

* API3:2019 Excessive Data Exposure - OWASP API Security Top 10 2019
* API6:2019 - Mass Assignment - OWASP API Security Top 10 2019
* CWE-213: Exposure of Sensitive Information Due to Incompatible Policies
* CWE-915: Improperly Controlled Modification of Dynamically-Determined Object Attributes  

----  


