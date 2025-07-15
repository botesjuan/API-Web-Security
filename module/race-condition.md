# Race Conditions  

## Limit overrun race conditions  

>[PotSwigger Academy - Limit overrun race conditions](https://portswigger.net/web-security/learning-paths/race-conditions/race-conditions-limit-overrun/race-conditions/limit-overrun-race-conditions-r6f5)  

>Identify potential attack vector in a single-use or rate-limited endpoint that has some kind of security impact.  
>Methodology of Server-side jitter is useful during the initial discovery enumeration of Race conditions attacks.  
>Attacking "time-of-check to time-of-use" (TOCTOU) flaws.  

>Trigger race conditions custom action using Burp Suite Professional.  

## Identify Race Condition Creteria  

>Without a session cookie, can only access an empty cart.  
>From this, can infer that:  

* State of the cart is stored server-side in logged in session.  
* Any operations on the cart are keyed on session ID or associated user ID.  

![portswigger_race_condition_prep.png](/images/portswigger_race_condition_prep.png)  

>Prepare the race condition duplicate similtaneous requests:  

1. In Repeater, add the new tabs to a new group. 
2. Right-click the grouped tab `Race-Condition-Group` then select Duplicate tab. 
3. Create 19 duplicate tabs, are automatically added to the group.  
4. Send the group of requests in ***parallel***, using `single-packet attack` to apply discount multiple times at once.  

![portswigger_race_condition_prep_seperate_connections.png](/images/portswigger_race_condition_prep_seperate_connections.png)  

>Click ***Send group(parallel)*** button in repeater group.  

>Results in cart page, after refresh your cart and confirm that the 20% reduction has been applied more than once, result of race condition attack.  

## Bypass Rate Limits via race conditions  

>Target rate limiting defend against brute-force attacks to obtain passwords.  
>Deduce that the number of failed attempts per username must be stored server-side.  

>***indentify brute force race condition***  

>Enumerate possible race condition attack by sending multiple grouped request in parallel,  
>and notice that more than 3 responses contain same message `Invalid username or password`,  
>instead of the lockout message, suggesting race condition possible.  

### Turbo Intruder  

>Highlight the value of the password `%s`, and Send the login password request to ***Turbo Intruder*** extension.  

![portswigger_race_condition_rate_limit_turboIntruder.png](/images/portswigger_race_condition_rate_limit_turboIntruder.png)  

```python
def queueRequests(target, wordlists):

    # as the target supports HTTP/2, use engine=Engine.BURP2 and concurrentConnections=1 for a single-packet attack
    engine = RequestEngine(endpoint=target.endpoint,
                           concurrentConnections=1,
                           engine=Engine.BURP2
                           )
    
    # assign the list of candidate passwords from your clipboard
    passwords = wordlists.clipboard
    
    # queue a login request using each password from the wordlist
    # the 'gate' argument withholds the final part of each request until engine.openGate() is invoked
    for password in passwords:
        engine.queue(target.req, password, gate='1')
    
    # once every request has been queued
    # invoke engine.openGate() to send all requests in the given gate simultaneously
    engine.openGate('1')

def handleResponse(req, interesting):
    table.add(req)
```  

>Copy passwords wordlist into clipboard.  
>click ***Attack***  

>Study the responses, for a successful login `HTTP/2 302 Found`, wait for lockout to reset.  

![portswigger_race_condition_rate_limit_turboIntruder_brute_force.png](/images/portswigger_race_condition_rate_limit_turboIntruder_brute_force.png)  

>Brute force login using race condition attack successful.  

## 