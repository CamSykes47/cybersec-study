# Old Sessions
>Category: Web Exploitation

>"Proper session timeout controls are critical for securing user accounts. If a user logs in on a public or shared computer but doesn’t explicitly log out (instead simply closing the browser tab), and session expiration dates are misconfigured, the session may remain active indefinitely.

>This then allows an attacker using the same browser later to access the user’s account without needing credentials, exploiting the fact that sessions never expire and remain authenticated.

>Your friend tells you to check out a new social media platform he built a few years ago. Although its still under development, he said the site is almost complete. He also mentioned that he hates constantly logging into sites, and so has made his page that 'once you login, you never have to log-out again'!


## Info Gathering:

- Link: http://dolphin-cove.picoctf.net:[insertinstanceID]/
- Allows you to register and stay logged in
- Upon logging in, one user points out a "strange site" at http://dolphin-cove.picoctf.net:[insertinstanceID]/sessions
	- accessing this link shows two active sessions, 'admin' and yours, with session identifiers
- Hints:
	- "Do you know how to use the web inspector?"
	- "Where are cookies stored?"
		- cookies stored locally == keys updated via inspector will allow you to locally access a different username?
	
### Solution 1 (initial solution):

- (Firefox) F12 > Inspector 
- Using the web inspector, I changed the permanent modifier for 'admin' to False and changed the key to something else.
- Then, I changed my name with inspect element to 'admin', keeping my session key
- I opened the main site in a new tab, logged in using admin as the username and my password from the username I registered with
- I was allowed access, giving me the flag: picoCTF{REDACTED}

### Solution 2 (Better One):
- Go to /sessions link and copy the session ID for admin
- go back to main page
- (Firefox) F12 > Storage > Cookies
- look for the session cookie and enter the copied ID as the value.
- refresh the page
- picoCTF{REDACTED}

## Vulnerabilities:
1. No session IDs expire on timeout
2. Session IDs for multiple users are exposed with no access control to this list
3. Session IDs don't change if logins or privileges change
-> Leads to Session Hijacking via Cookie Spoofing

## Notes:

- My initial solution probably wasn't the intended solution, but an interesting option as the vulnerability also allows access without actually editing cookies
- Should review Session Hijacking
	- https://www.thesslstore.com/blog/the-ultimate-guide-to-session-hijacking-aka-cookie-hijacking/
	- https://whiteintel.io/blog/session-hijacking

## Review:

### Impact: 

- Such a setup could lead to attackers gaining access to PII left on the social media site, which can open them up to identity theft attempts and fraud.
- Depending on the nature of the site and whether it also captures bank information for in-app purchases, this can also lead to financial loss for users who have their bank information accessed by an attacker who gets into their account via the hijacking.
- The business operating the site would face hefty losses due to lawsuits and user outrage afterwards due to the data leak, which was caused by poor security standards.

### How to Prevent:
- Set a rule to expire inactive sessions after a set amount of time
- Only store session data on the server-side with proper environmental authentication checks in place so that only administrators can view/access it
- Use HTTPS on the full site, not just login pages
- Use Secure Cookies
- Regenerate session IDs upon each login attempt

### How to Detect:
- Fingerprinting session tokens using data such as User-Agent headers or IP subnets
	- Can be difficult to track if attempts are done via mobile networks
- Set monitoring to check for single session IDs used from either multiple IP addresses or in varying geographical locations and/or with multiple attempts in a short time frame
