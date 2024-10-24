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

8. Create folders and move the requests in colleciton to folders matching endpoints of the API.

![postman-create-folders-organize-api-endpoints.png](/images/postman-create-folders-organize-api-endpoints.png)  

----  

>Build collection of endpoint functions of API through MITMProxy:

1. start `mitmproxy` / `mitmweb` - command line tool used to proxy web traffic  
2. Browse to MITMProxy web interface, `http://127.0.0.1:8081` - mitmweb proxy.  
3. On browser set the proxy port as `8080`
4. Navigate each endpoint of the API performing functions to be captured.
5. In MITMweb save the capture to file.  

![mitmweb-mitmproxy.png](/images/mitmweb-mitmproxy.png)  

6. Then run command `mitmproxy2swagger -i flows -o spec.yml -p http://127.0.0.1:8888 -f flow --examples`  
7. Edit online swagger at `https://editor.swagger.io`, also used to visualize API Documentation.  

----  

## APIs and Excessive Data Exposure  

>[Using APIs and Excessive Data Exposure - Endpoint Analysis](https://university.apisec.ai/products/api-penetration-testing/categories/2150251353/posts/2157505648)  

>Learn to use Postman functions and features.  

>Updating Postman Collection Authorization:  

![postman-update-collection-authorization-global.png](/images/postman-update-collection-authorization-global.png)  

>Get Authentication token, by sending login request to the API:  

![postman-get-auth-token.png](/images/postman-get-auth-token.png)  

>response received include latest ***bearer*** authenticaiton token in the response body, copy and place at top of collection for this API as global bearer authenticaiton token:  

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

# Endpoint Analysis Assessment  

>[Endpoint Analysis Assessment Exercise](https://university.apisec.ai/products/2147849691/categories/2150251353/posts/2163584818)  


