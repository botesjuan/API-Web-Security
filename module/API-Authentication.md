# API Authn & Authz  

* [API Authentication and Authorization](https://university.apisec.ai/products/api-authentication/categories/2155368013/posts/2177832033)  

## API Authentication Brief  

>Who is authenticated.  

* Basic Authentication - Username and Password  
* API Keys - HTTP Headers  
* TLS Authentication - HTTPS certificate  
* Token Based Authentication - User id, role, Expire, trusted 3rd partiy  
* Token Based Auth and Authorization with OAuth and OpenID Connect  
* OpenID connect vs OAuth - federation and delegation  
* OAuth 2 - 2012 - no signing, require HTTPS  

>Authorization is the process of ***Figuring out what the user is allowed to do***  

>Which of the following statements is incorrect about basic authentication?  
 * Basic Authentication can be used to authenticate both the user and the machine at the same time.  

## Oauth Actors  

* [API Authentication Oauth Actors](https://university.apisec.ai/products/api-authentication/categories/2155375023/posts/2177860649)  

>The problem:  

* sharing with 3rd party authentication details on users behalf.  
* The application should not see the suers credentials.  
* User need to be aware that is has allowed app access  

>OAuth is delegation, ***not*** authorization.  

>Example:  
>the act itself became a delegation and the delegation was then used when I went to the bank and I presented the proof of delegation  
>and the bank rejected delegation proof anyway, because other things took place.  
>So delegation is different from authorization and OAuth is really about delegation.  

* What was the main problem that led to the inception of OAuth?  
  * Companies wanted to expose APIs that used authentication but didn't want 3rd parties to see the user's credentials  
* The Actors of OAuth 2.0 are  
  * The client (Application), the resource server (The API), the authorization server and the resource owner (User)  
* Who delegates to who in OAuth  
  * The authorization server delegates to the client. the client is the application  
  
## OAuth 2.0 Interaction patterns  

* [OAuth interaction patterns or OAuth flows](https://university.apisec.ai/products/api-authentication/categories/2155375676/posts/2177863558)  

>Code Flow steps 

1. User visit site, login  
2. Authorize scope PXE
3. Send state parameter
4. Authentication process send to redirect to authentication service prompting user to go through verfication process  
5. 
