# Broken Authentication  

>[API2:2023 Broken Authentication](https://university.apisec.ai/products/owasp-api-security-top-10-and-beyond/categories/2152491879/posts/2166897970)  

>Authentication is the process that is used to verify the identity of an API user,  
>whether that user is a person, device, or system. In other words,  
>authentication is the process of verifying that an entity is who that entity claims to be.  

>Weak Password Policy - Allows users to create simple passwords  

>Credential Stuffing - attack against authentication where a large number of username and password combinations are attempted  

>Predictable Tokens - Weak tokens can easily be guessed, deduced, or calculated by an attacker  

>Misconfigured JSON Web Tokens - JSON Web Tokens (JWTs) are commonly used for API authentication and authorization processes.  

## OWASP Preventative Measures  

* Know all the possible flows to authenticate to the API (mobile/ web/deep links that implement one-click authentication/etc.)  
* Read about your authentication mechanisms. OAuth is not authentication, and neither are API keys.  
* Authentication, token generation, or password storage. Use the standards.  
* Credential recovery/forgot password endpoints should be treated as login endpoints in terms of brute force, rate limiting, and lockout protections.  
* Require re-authentication for sensitive operations (e.g. changing the account owner email address/2FA phone number).
* Use the OWASP Authentication Cheatsheet.
* Implement multi-factor authentication.
* Implement anti-brute force mechanisms to mitigate credential stuffing, dictionary attacks, and brute force attacks on authentication endpoints  
* Implement account lockout/captcha mechanisms to prevent brute force attacks against specific users. Implement weak-password checks  
* API keys should not be used for user authentication. They should only be used for API clients authentication  

>Additional Resources:  

* CWE-204: Observable Response Discrepancy
* CWE-307: Improper Restriction of Excessive Authentication Attempts
* CWE-798: Use of Hard-coded Credentials  

----  

>

