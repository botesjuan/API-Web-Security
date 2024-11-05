# Improper Inventory Management  

>[API9:2023 Improper Inventory Management](https://university.apisec.ai/products/owasp-api-security-top-10-and-beyond/categories/2152492227/posts/2166899032)  

>Improper Inventory Management represents the risks involved with exposing non-production and unsupported API versions.  
>When this is present the non-production and unsupported versions of the API are often not protected by the same security.  

## OWASP Preventative Measures  

* Inventory all API environments, hosts and document important aspects of each, production, staging, test, development, public, internal, partners and the API version  
* Inventory integrated services and document important aspects such as their role in the system, what data is exchanged (data flow), and its sensitivity.  
* Document all aspects of your API such as authentication, errors, redirects, rate limiting, cross-origin resource sharing (CORS) policy, including parameters, requests, and responses  
* Generate documentation automatically by adopting open standards. Include the documentation build in your CI/CD pipeline.  
* Make API documentation available to those authorized to use the API.  
* Use external protection measures such as API security firewalls for all exposed versions of your APIs, not just for the production version  
* Avoid using production data with non-production API deployments. If this is unavoidable, these endpoints should get the same security treatment
* Newer versions of APIs include security improvements, perform risk analysis to make the decision of the mitigation actions required for the older version  

>Poorly managed bills of materials (BOMs) can lead to improper inventory management,  
>which can have a number of negative consequences for a business.  

>Additional Resources  

* CWE-1059: Incomplete Documentation  

----  

>When running multiple versions of an API, what risk does this pose?  

* Increased management resources and expanded attack surface  

>Which of the following can aid an attacker in identifying improperly managed APIs?  

* Outdated API documentation  

>Which of the following is the most severe impact related to API inventory management?  

* Enables an attacker to exploit other known vulnerabilities

>Which of the following is an indication that an API has a "data flow blindspot"?  

* There is no visibility of which type of sensitive data is shared  

>Which of the following practices can help mitigate risks associated with improper API inventory management?  

* Automatically generate documentation with open standards and including it in the CI/CD pipeline  

----  
