# Improper Assets Management  

>[Testing for Improper Assets Management](https://university.apisec.ai/products/api-penetration-testing/categories/2150251354/posts/2157505650)  

>Often times an API provider will update services and the newer version of the API will be available over a new path like the following:

```
api.target.com/v3
/api/v2/accounts
/api/v3/accounts
/v2/accounts
```  

>API versioning could also be maintained as a header:

```
Accept: version=2.0
Accept api-version=3
```  

>In addition versioning could also be set within a query parameter or request body.

```
/api/accounts?ver=2
POST /api/accounts

{
"ver":1.0,
"user":"hapihacker"
}
```  

>Earlier versions of the API may no longer be patched or updated. Since the older versions lack this support, they may expose the API to additional vulnerabilities.  
>For example, if `v3` of an API was updated to fix a vulnerability to injection attacks, then there are good odds that requests that involve v1 and v2 may still be vulnerable. 

>Non-production versions of an API include any version of the API that was not meant for end-user consumption. Non-production versions could include:  

```
api.test.target.com
api.uat.target.com
beta.api.com
/api/private
/api/partner
/api/test
```  

## Find Improper Assets Management Vulnerabilities  

>Authenticated web directory discovery to find different undocument versions that is possible vulnerable endpoints.

>Using Burp repeat get valid `Authorization: Bearer` header token cookie.  

```
ffuf -ic -c -w /home/kali/Downloads/apisec/api-version-wordlist.txt:FUZZ -u http://127.0.0.1:8888/FUZZ -H 'Authorization: Bearer <token>'

3. Use POST method:

gobuster dir -k -U vdaisley -P calculus20 -u http://dev.topology.htb/ -w /home/kali/Downloads/apisec/api-version-wordlist.txt

```  

>Authenticated Directory scan, recursive depth set, to identify API versions.  

```
ffuf -ic -c -recursion -recursion-depth 6 -v -w /home/kali/Downloads/apisec/api-version-wordlist.txt:FUZZ -u http://127.0.0.1:8888/FUZZ -H 'Authorization: Bearer eyJhbGciOiJSUzI<snip>DKmr4ChPLUcQ'
```

![crapi-inproper-assets-management.png](/images/crapi-inproper-assets-management.png)  