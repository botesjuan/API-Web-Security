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

1. start `mitmproxy` / `mitmweb`   
2. Browse to MITMProxy web interface, `http://127.0.0.1:8081` - mitmweb proxy.  
3. On browser set the proxy port as `8080`
4. Navigate each endpoint of the API performing functions to be captured.
5. In MITMweb save the capture to file.  

![mitmweb-mitmproxy.png](/images/mitmweb-mitmproxy.png)  

6. Then run command `mitmproxy2swagger -i flows -o spec.yml -p http://127.0.0.1:8888 -f flow --examples`  
7. Edit online swagger at `https://editor.swagger.io`  

----  

## APIs and Excessive Data Exposure  

>[Using APIs and Excessive Data Exposure - Endpoint Analysis](https://university.apisec.ai/products/api-penetration-testing/categories/2150251353/posts/2157505648)  

