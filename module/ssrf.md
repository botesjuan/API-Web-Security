# Server Side Request Forgery  

>[API7:2023 Server Side Request Forgery - SSRF](https://university.apisec.ai/products/owasp-api-security-top-10-and-beyond/categories/2152492172/posts/2166898781)  

>SSRF takes place when a user is able to control the remote resources retrieved by an application.  
>An attacker use an API to supply their own input, in the form of a URL,  
>to control the remote resources that are retrieved by the targeted server.  

## Out-of-Band SSRF  

>Out-of-Band (or Blind) SSRF takes place when a vulnerable server performs a request from user input,  
>but does not send a response back to the attacker indicating a successful attack.  
>Victim server makes the request to the exploit URL specified by the attacker,  
>but the attacker does not receive a direct message back from the victim server.  

## OWASP Preventative Measures  

* Isolate the resource fetching mechanism in your network: retrieve remote resources and not internal services  
* Use allow lists of Remote origins users are expected to download resources from (e.g. Google Drive, Gravatar, etc.)
* URL schemes and ports
* Accepted media types for a given functionality  
* Disable HTTP redirections  
* Use a well-tested and maintained URL parser to avoid issues caused by URL parsing inconsistencies.  
* Validate and sanitize all client-supplied input data.  
* Do not send raw responses to clients.  

>Additional Resources:  

* CWE-918: Server-Side Request Forgery (SSRF)  

----  

>What is Server-Side Request Forgery (SSRF)?  

* An attack where an attacker uses the server as a proxy to hide malicious activities.
* An attack where the attacker controls remote resources retrieved by a server.
* An attack that can be used to expose private data and scan the victim's internal network.  

>What is a potential consequence of an SSRF vulnerability?  

* The server can be used as a proxy to hide malicious activities.
* Information disclosure, bypassing firewalls, or other security mechanisms.
* It can lead to DoS.  

>Which of the following is ***NOT*** a preventative measure against SSRF?  

* Enable X-SSRF Headers  

>What is a key characteristic of SSRF exploitation?  

* Requires the attacker to find an API endpoint that receives a URI as a parameter and then accesses the provided URI.  

>What could be a business risk from successful exploitation of SSRF?  

* It could lead to internal services enumeration or information disclosure.
* It could lead to the server being used as a proxy.
* It could bypass firewalls or other security mechanisms.  

----  

## Parameter Pollution  

>Server-side Parameter pollution can occur in structured data formats, REST API paths, or query strings.  
>Server-side parameter pollution can occur in JSON or XMNL structured data formats.  

>Examples of parameter Pollution:  

```
GET /userSearch?name=peter%26foo=xyz&back=/home

GET /users/search?name=peter&email=foo&publicProfile=true

GET /users/search?name=peter&name=carlos&publicProfile=true

GET /edit_profile.php?name=peter%2f..%2fadmin

{name="peter","access_level":"administrator"}
```  

* [PortSwigger Web Security Academy - API Testing](https://portswigger.net/web-security/learning-paths/api-testing/api-testing-testing-for-server-side-parameter-pollution-in-the-query-string/api-testing/server-side-parameter-pollution/injecting-invalid-parameters)  

>Using *** Backslash Powered Scanning***