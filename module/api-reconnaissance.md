# API Reconnaissance  

## Intro Reconnaissance  

>[Intro to API Reconnaissance](https://university.apisec.ai/products/api-penetration-testing/categories/2150259092/posts/2159321350)  

>Wordlist & Indicators  

```
api
/api/v1
v1
v2
v3
rest
swagger
swagger.json
doc
docs
man
manuals
documents
text
txt
backup
bac
graphql
graphiql
altair
playground
api4
apis
uat
developer
dev
test
tst
rapi
rapidapi
prog
program
search
guru
interface
int
```  

## Passive Reconnaissance  

>[Passive Reconnaissance](https://university.apisec.ai/products/api-penetration-testing/categories/2150259092/posts/2157852412)  

>Google Dorking:  

| **Google Dorking Query** | **Expected results** |
|-|-|
| `inurl:"/wp-json/wp/v2/users"` | Finds all publicly available WordPress API user directories. |
| `intitle:"index.of" intext:"api.txt"` | Finds publicly available API key files. |
| `inurl:"/api/v1" intext:"index of /"` | Finds potentially interesting API directories. |
| `ext:php inurl:"api.php?action="` | Finds all sites with a XenAPI SQL injection vulnerability. |
| `intitle:"index of" api_key OR "api key" OR apiKey -pool` | This is one of my favorite queries. It lists potentially exposed API keys. |

>GitDorking

>Searching GitHub for OSINT could reveal your target’s API capabilities, documentation, and secrets, such as API keys, passwords, and tokens.  
>Parameters like:  

```
filename:swagger.json
extension: .json
```  

>Searching GitHub for your target organization name paired with:

```
api key
api keys
apikey
authorization: Bearer
access_token
secret
token
```  

>Analyze the source code in the Code tab, find bugs in the Issues tab, and review proposed changes in the Pull requests tab.  

>TruffleHog  

>TruffleHog is a great tool for automatically discovering exposed secrets.  

```
sudo docker run -it -v "$PWD:/pwd" trufflesecurity/trufflehog:latest github --org=target-name
sudo docker run -it -v "$PWD:/home/kali/Download/htb" trufflesecurity/trufflehog:latest github --org=target-name 
```  

>Shodan Search phrases:  

>[https://www.shodan.io/](https://www.shodan.io/)  

```
hostname:"targetname.com"
"content-type: application/json"
"content-type: application/xml"
"200 OK"
"wp-json"
```  

Wayback Machine - Archive web site searching:  

[https://web.archive.org/](https://web.archive.org/)  

>Zombie APIs fall under the Improper Assets Management vulnerability on the OWASP API Security Top 10 list.  

## Active Reconnaissance  

>[Active API Reconnaissance](https://university.apisec.ai/products/api-penetration-testing/categories/2150259092/posts/2159320580)  

>NMAP API target: `sudo nmap 127.0.0.1 -p 22,21,80,443,8000,8888,8080,8443,8025,25,44665 -sCV -A -O --open`  

>AMASS Subcommands:                                                                                                        
* amass intel - Discover targets for enumerations                                                             
* amass enum  - Perform enumerations and network mapping

```
amass enum -list
amass enum -active -d crapi.apisec.ai
amass enum -active -d microsoft.com | grep api
```  

>Gobuster: Directory Brute-force with Gobuster.  

```
gobuster dir -u target-name.com:8000 -w /home/hapihacker/api/wordlists/common_apis_160
```  

>Gobuster, with exclude length when error state status code that matches the provided options for non existing urls

```
gobuster dir -u http://127.0.0.1:8888 -w /usr/share/dirb/wordlists/common.txt --exclude-length 7092
Gobuster dir scan set with status code whitelist and status code black list, including exclude response length of zero.

gobuster dir -u https://tst-vaschannelinsureapi.apps.ocptst.ho.fosltd.co.za/ -w /usr/share/wordlists/dirb/common.txt -k --exclude-length 0 -s "200,300,301,302,404,500" -b ""


gobuster dir -u http://127.0.0.1:8888 -w ./common_apis_160.txt -x 200,202,301 -b 302 --exclude-length 7092
```  

>Kiterunnner  

```
kr scan HTTP://127.0.0.1:8888 -w /home/kali/Downloads/apisec/routes-large.kite
```  

![kiterunner-replay1.png](/images/kiterunner-replay1.png)  

>Replay interresting request from results:  

```
kr kb replay "POST    414 [    183,    7,   8] http://127.0.0.1:8888/team/save 0cf682ca32082dfb895a509e9d42e98a026ac8ea" -w /home/kali/Downloads/apisec/routes-large.kite
```  

![kiterunner-replay2.png](/images/kiterunner-replay2.png)  
