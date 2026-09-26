# Crack the Gate 1

>Web Exploitation

>"We’re in the middle of an investigation. One of our persons of interest, ctf player, is believed to be hiding sensitive data inside a restricted web portal. We’ve uncovered the email address he uses to log in: ctf-player@cylabacademy.org. Unfortunately, we don’t know the password, and the usual guessing techniques haven’t worked. But something feels off... it’s almost like the developer left a secret way in. Can you figure it out?"

## Info Gathering:

- Link: http://chatelaine.cylabacademy.net:[insert session number]
- Login Page upon accessing
- View-Source reveals this note in text: ABGR: Wnpx - grzcbenel olcnff: hfr urnqre "K-Qri-Npprff: lrf
- ROT13 applied gives following: "NOTE: Jack - temporary bypass: use header "X-Dev-Access: yes"
- Hints:
	- Developers sometimes leave notes in the code; but not always in plain text.
	- A common trick is to rotate each letter by 13 positions in the alphabet.


## Initial Thoughts:

- I know what needs to be added to the header, it's just a matter of getting the authentication response from the site
- What methods are available to intercept, review, & send edited request data with included header?

## Vulnerabilities:

1. Backdoor method to site left publicly available in website source code
2. Cipher applied to note is easily reversed, revealing information to anyone who recognizes ROT13 cipher
3. Client has ability to inject custom header to server without any authorization checks (header spoofiing)

### Solution 1 (Burp Suite):

1. Access Link
2. Ctrl+U to view source for site (can also be done via F12)
3. Notes in the code reveal a string of characters, immediately followed by a note to <!-- Remove before pushing to production! -->  
4. Putting the characters through a ROT13 cipher gives dev bypass header for login.
5. Run Burp Suite proxy and enable Interception to capture authentication attempt
6. Attempt to login with provided email address and random password while proxy's running
7. Review intercepted request in Proxy and pass to Repeater
8. Add "X-Dev-Access: yes" header to data and send to the server
9. Will receive successful request data with flag in JSON data : academy{REDACTED}

### Solution 2 (Lightweight/Browser Dev Tools)

1. Access Link
2. Ctrl+U to view source for site (can also be done via F12)
3. Notes in the code reveal a string of characters, immediately followed by a note to <!-- Remove before pushing to production! -->  
4. Putting the characters through a ROT13 cipher gives dev bypass header for login.
5. F12 > Network
6. Try to login as provided email with random password
7. Review captured request in Network tab > right-click > Edit and Resend
8. Add Header (name: X-Dev-Access; value: yes) > Send
9. Status 200 > Open Response > flag listed in JSON data


## Notes:

- ROT13 is a commonly used cipher that should be attempted when looking over a string of characters
- Check to see if sites are vulnerable to header spoofing
- View Source is a powerful first step in web exploitation as you never know what devs may leave available in the source code

## Review

### How To Detect:

- System should monitor and log authentication requests for unusual header behavior, then pass an alert if non-standard headers are received during a login attempt

### How To Defend/Prevent:

- Unrecognized headers trigger an authentication error during login attempt (request headers are untrusted input always)
- Have code checked for any notes that may direct to bypasses or confidential access data or any 'TODO: remove' lines and remove such notes prior to pushing to prod
- Remove any backdoor logic from source code (most secure) OR place backdoor behind specific server-side checks that aren't exposed to clients (second most secure)
