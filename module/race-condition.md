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

## Hidden multi-step Collisions  

>[PortSwigger Race Condition - Collisions Logic Flaws](https://portswigger.net/web-security/learning-paths/race-conditions/race-conditions-hidden-multi-step-sequences/race-conditions/hidden-multi-step-sequences)  

>Expose time-sensitive variations of the kinds of logic flaws  

>Collisions identification, using the ***Trigger race conditions*** custom action, sends parallel requests with a single click.  

>Connection ***warming*** technique to send initial request,  
>time of request delays to see if following requests through backend architecture servers are delayed  
>and may not interfere with collision race Condition attack.  

![portswigger_race_condition_collisions.png](/images/portswigger_race_condition_collisions.png)  

>How collision attack happened above:  

1. add cheap gift card product to cart  
2. Going to cart to check gift cart contain cheap gift card  
3. Before sending checkout request with cheap product gift card, update a cart add request with expensive 3l33t jacket product
4. then `Send group (parallel)` requests both the expensive product request and checkout cart with cheap gift card
5. Due to collision race condition checkout purchase success with expensive product.  

>Result the expensive product purchased and the store credit goes negative value.   

## Single-endpoint Race Conditions  

>Consider a password reset mechanism that stores the user ID and reset token in the user's session.  
>Two parallel password reset requests from the same session, but with two different usernames, could potentially cause the collision.  

![portswigger_race_condition_single_end_collisions.png](/images/portswigger_race_condition_single_end_collisions.png)  

>The email update for customer portal allow race condition to single endpoint exploitation in compromising account takeover.  

>[PortSwigger Lab: Single-endpoint race conditions](https://portswigger.net/web-security/learning-paths/race-conditions/race-conditions-single-endpoint/race-conditions/lab-race-conditions-single-endpoint#)  

## Time-Sensitive Tokenz  
 
>Site use time stamps to hash password reset tokens.  

>Send parallel force password reset for two different users at the same time, 
>this will result in duplicate matching tokens because the same timestamp used to generate the reset tokenz.  

>Receive the reset token url email and edit the name in the url to match `carlos` target victim user.  

![portswigger_race_condition_force-forgot-password-tokenz.png](/images/portswigger_race_condition_force-forgot-password-tokenz.png)  

>[PortSwigger Lab: Exploiting time-sensitive vulnerabilities](https://portswigger.net/web-security/learning-paths/race-conditions/race-conditions-time-sensitive-attacks/race-conditions/lab-race-conditions-exploiting-time-sensitive-vulnerabilities#)  

----  
