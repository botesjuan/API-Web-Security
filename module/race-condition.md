# Race Conditions  

## Limit overrun race conditions  

>[PotSwigger Academy - Limit overrun race conditions](https://portswigger.net/web-security/learning-paths/race-conditions/race-conditions-limit-overrun/race-conditions/limit-overrun-race-conditions-r6f5)  

>Identify potential attack vector in a single-use or rate-limited endpoint that has some kind of security impact.  
>Methodology of Server-side jitter is useful during the initial discovery enumeration of Race conditions attacks.  
>Attacking "time-of-check to time-of-use" (TOCTOU) flaws.  

>Trigger race conditions custom action using Burp Suite Professional.  

### Identify Race Condition Creteria  

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

## 