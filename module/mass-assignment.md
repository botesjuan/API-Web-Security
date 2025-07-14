# Mass Assignment Attacks

* [Mass Assignment Vulnerabilities](https://university.apisec.ai/products/api-penetration-testing/categories/2150251359/posts/2157505663)  
* [PortSwigger Web Security Academy - API Testing](https://portswigger.net/web-security/learning-paths/api-testing/api-testing-mass-assignment-vulnerabilities/api-testing/testing-mass-assignment-vulnerabilities)  

>The user registration request may contain key-values for username, email address, and password.  
>An attacker could intercept this request and add parameters like "isadmin": "true".  
>If the data object has a corresponding value and the API provider does not sanitize the attacker's  
>input then there is a chance that the attacker could register their own admin account.  

## Testing Account Registration for Mass Assignment  

>Start the local OWASP vAPI docker container lab:  

```
cd ~/lab
sudo docker-compose up -d
```  

>Set `postman` to custom proxy and direct requests to Burp Suite: `127.0.0.1:8080`  

>Intercepting the registration signup form, and adding a JSON field `isadmin` with value of true, is accepted.

![crapi-mass-assignment-add-isadmin-json-field.png](/images/crapi-mass-assignment-add-isadmin-json-field.png)  

>Mass Assignement key-values to test in the JSON POST body:  

```
"isadmin": true,
"isadmin":"true",
"admin": 1,
"admin": true, 
```  

>Excessive data exposure GET request to `/community/api/v2/community/posts/recent?limit=3&offset=0`,  
>reveal not new user information about user admin roles.  

>Performing a GET request to `/identity/api/v2/user/dashboard` reponded with the field:  

```
"role":"ROLE_USER"
```  

## Fuzzing for Mass Assignment with Param Miner  

>Attempt to change to `"role":"ROLE_ADMIN"` value using signup form with the Burp Extension ***Param Miner***:  

>To use Param Miner, right click on a request in Burp and click "Guess (cookies|headers|params)".  
>In Burp Suite Pro, identified parameters will be reported as scanner issues.  
>The ***output*** tab shows found parameters or - Param Miner - Output:  

### Mass Assignment Vulnerability  

1. Login to the application from http://localhost:8888/login
2. Click Shop in the navbar to visit http://localhost:8888/shop
3. There is an initial available balance of $100. Order the Seat item for $10 from the shop by using the Buy button and observe the request sent.  

![crapi-mass-assignment-buy-product-credit-response.png](/images/crapi-mass-assignment-buy-product-credit-response.png)  

>Observing the POST request `/workshop/api/shop/orders`, the credit has been reduced by $10.  

4. Alter the fields in POST request /workshop/api/shop/orders to change the value of quantity in the request body to a negative value and send the buy request.   
5. It can be observed that the available balance has now increased and the buy order has been placed.  

![crapi-mass-assignment-buy-product-negative-quantity.png](/images/crapi-mass-assignment-buy-product-negative-quantity.png)  

6. We can verify that the order has been placed by going to the [Past Orders](http://127.0.0.1:8888/past-orders) section and verify exploitation of mass assignment.  

## Other Mass Assignment Vectors  

>POST to `/api/v1/register` with partial body value, include `org` to gain access to another organizations.

```JSON
{
"username":"hAPI_hacker",
"email":"hapi@hacker.com",
"org": "§CompanyA§",
"password":"Password1!"
}
```  

>Assign other organizations, you will likely be able to gain unauthorized access to the other group’s resources. To perform such an attack, you’ll need to know the names or IDs used to identify the companies in requests. 


## Hunting for Mass Assignment  

>Add unauthorized products using mass assignment vulnerabilities, in the `http://127.0.0.1:8888/workshop/api/shop/products` endpoint.
>Change the GET request to POST and the content-type to `application/json`:  

![crapi-mass-assignment-add-products.png](/images/crapi-mass-assignment-add-products.png)  

>Impact is create our own product items, and quantity that has a negative value allow purchase item ,and exploit lead to a new account balance positive.  

----  

>Mass Assignment vulnerabilities are present when an attacker is able to:  

* Overwrite object properties that they should not be able to  

>Which of the following is required for a mass assignment vulnerability to be present?  

* An API must be lacking user input validation

>Which of the following often helps the most in discovering parameter names to use in a Mass Assignment attack?

* Admin API Documentation  

>Which of the following crAPI URLs is vulnerable to Mass Assignment?  

* http://crapi.apisec.ai/workshop/api/shop/products  

>What is the impact of the crAPI mass assignment vulnerability?  

* An attacker can arbitrarily add funds to their account  

# Mass Assignment Assessment  

>[Mass Assignment Assessment](https://university.apisec.ai/products/2147849691/categories/2150251359/posts/2163536891)  

>Which two requests are available for testing vAPI for Mass Assignment (API6)?  

* GET /vapi/api6/user/me
* POST /vapi/api6/user

>What is the field that can be used in a mass assignment attack against /vapi/api6?  

* credit  

>What is the flag for successfully exploiting vAPI's Mass Assignment vulnerability?  

* api6_afb969db8b6e272694b4  

>What HTTP response code is returned after performing a successful mass assignment attack against vAPI?  

* 200 ok  

>What HTTP response code is returned when sending a PUT request to http://vapi.apisec.ai/vapi/api6/user?  

* 500 error  

----  
