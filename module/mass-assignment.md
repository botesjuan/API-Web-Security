# Mass Assignment Attacks

>[Mass Assignment Vulnerabilities](https://university.apisec.ai/products/api-penetration-testing/categories/2150251359/posts/2157505663)  

>The user registration request may contain key-values for username, email address, and password.  
>An attacker could intercept this request and add parameters like "isadmin": "true".  
>If the data object has a corresponding value and the API provider does not sanitize the attacker's  
>input then there is a chance that the attacker could register their own admin account.  

## Testing Account Registration for Mass Assignment  


## Fuzzing for Mass Assignment with Param Miner  



## Other Mass Assignment Vectors  




## Hunting for Mass Assignment  




