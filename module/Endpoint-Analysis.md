# Endpoint Analysis  

## Reverse Engineering an API  
 
>[Building a Collection in Postman](https://university.apisec.ai/products/api-penetration-testing/categories/2150251353/posts/2157921175)  

>Building a Collection in `Postman`  

1. Start Postman
2. Create new workspace
3. Create a collection in new workspace
4. Select `Capture Requests`, and enable proxy.  

![postman-capture-requests-enable-proxy4.png](/images/postman-capture-requests-enable-proxy4.png)  

5. On browser set the proxy to port `5555` over to postman.  
6. Navigate each function on the target, while traffic is captured in Postman.  
7. In Postman `stop`  capture requests and select the requests that indicate `API` endpoints to add to the collection.  

![postman-add-requests.png](/images/postman-add-requests.png)  

8. Create folders and move the requests in collection to folders matching endpoints of the API.

![postman-create-folders-organize API endpoints](/images/postman-create-folders-organize-api-endpoints.png)  

----  

>Build collection of endpoint functions of API through MITMProxy:

1. start `mitmproxy` / `mitmweb` - command line tool used to proxy web traffic  
2. Browse to MITMProxy web interface, `http://127.0.0.1:8081` - mitmweb proxy.  
3. On browser set the proxy port as `8080`
4. Navigate each endpoint of the API performing functions to be captured.
5. In MITMweb save the capture to file.  

![mitmweb-mitmproxy.png](/images/mitmweb-mitmproxy.png)  

6. Then run command `mitmproxy2swagger -i flows -o spec.yml -p http://127.0.0.1:8888 -f flow --examples`  
7. Edit online swagger at new `https://editor-next.swagger.io/`, also used to visualize API Documentation.  

----  

## APIs and Excessive Data Exposure  

>[Using APIs and Excessive Data Exposure - Endpoint Analysis](https://university.apisec.ai/products/api-penetration-testing/categories/2150251353/posts/2157505648)  

>Learn to use Postman functions and features.  

>Updating Postman Collection Authorization:  

![postman-update-collection-authorization-global.png](/images/postman-update-collection-authorization-global.png)  

>Get Authentication token, by sending login request to the API:  

![postman-get-auth-token.png](/images/postman-get-auth-token.png)  

>response received include latest ***bearer*** authentication token in the response body, copy and place at top of collection for this API as global bearer authenticaiton token:  

![postman-update-collection-authorization-globaltoken.png](/images/postman-update-collection-authorization-globaltoken.png)   

>***SAVE***  

----  

## Excessive Data Exposure  

>***note***:  
>When changing the content of request body data, the `content-length` do not automatically adjust to number of bytes.  
>Unselect the header `content-type`, and then send request to target API:  

![postman-update-content-length.png](/images/postman-update-content-length.png)  

>Browsing the API requests and looking for response containing more information that is display to the web user browser, is considered finding, OWASP API 3: Excessive Data Exposure:  

![OWASP API 3: Excessive Data Exposure](/images/postman-sensitve-data-exposed.png)  

>Excessive Data Exposure occurs when an API provider sends back a full data object,   
>typically depending on the client to filter out the information that they need.   
>From an attacker's perspective, the security issue here isn't that too much information is sent,   
>instead, it is more about the sensitivity of the sent data.   

----  

## Endpoint Analysis Assessment  

>[Endpoint Analysis Assessment Exercise](https://university.apisec.ai/products/2147849691/categories/2150251353/posts/2163584818)  

>[Public Documentation for VAPI](http://vapi.apisec.ai/vapi)  

>When using vAPI, which fields are documented for the POST request to `/vapi/api1/user`?  

![vAPI-fields-POST-request.png](images/vAPI-fields-POST-request.png)  

>Send Postman requests to the Burp proxy port `8080`:  

![postman-proxy-requests-2-burp.png](/images/postman-proxy-requests-2-burp.png)  

>Postman, settings, proxy, custom proxy configuration:  

![postman-proxy-requests-2-burp-custom.png](/images/postman-proxy-requests-2-burp-custom.png)  

>Which request methods are documented for `/vapi/api1/user`?  

![vAPI-request-methods.png](images/vAPI-request-methods.png)  

>Documentation show HTTP verbs to be: `POST`, `GET`, `PUT`, but the endpoint for the specific user profile show HTTP verbs: `HEAD`, `GET`, `PUT`  

>Which of the following response headers are returned with a successful request to `/vapi/api1/user`?  

![vAPI-response-headers-returned.png](images/vAPI-response-headers-returned.png)  

>Which vAPI endpoint is used to get the flag for ***Insufficient Logging & Monitoring***?  

![vAPI-intruder-sniper-flag-attack.png](images/vAPI-intruder-sniper-flag-attack.png)  

----  

## Security Misconfigurations  

>[Finding Security Misconfigurations - Scanning APIs](https://university.apisec.ai/products/api-penetration-testing/categories/2150251355/posts/2157505653)  

>Scanning APIs with OWASP ZAP ^^  

>crAPI Solution Chalanges [crAPI walkthrough](https://zerodayhacker.com/crapi-walkthrough-using-ai/)  

>In the current state of API security, which vulnerability do automated scanners detect? API7:2019 Security Misconfiguration  

>What kind of results can a generic vulnerability scan of a web API lead to? False-Negative  

----  

## Scanning APIs Assessment  

>[APISEC - Scanning APIs Assessment](https://university.apisec.ai/products/2147849691/categories/2150251355/posts/2163585553)  

>Which of the following are security misconfigurations detected when using the automated scan feature of ZAP on http://vapi.apisec.ai/vapi?  

* Application Error Disclosure
* `.htaccess` Information Leak
* X-Content-Type Options Header Missing  

>Which of the following endpoints are detected when using the automated scan feature of ZAP on http://vapi.apisec.ai/vapi?  

* API1  
* API5  

>Which of the following vulnerabilities are indicated at http://vapi.apisec.ai/vapi#tag/API7?  

* Cross-Origin resource sharing (CORS) Misconfiguration  

----  

