# picoCTF Writeups

## Web Gauntlet 1
- Opened the login page in browser  
- Entered `admin' --` in the username field and any password  
- Submitted the form and bypassed login using SQL injection  


- ## Web Gauntlet 2
- Tried previous injection got blocked  
- Entered `admin' /*` to bypass the password check using block comment  
- Submitted the form and bypassed login  


- ## Web Gauntlet 3
- Used `admin';` in username to terminate query early  
- Submitted the form and accessed admin account
- 

- ## Irish-Name-Repo 1
- Visited the challenge page  
- Entered `' OR 1=1--` in the username field and any password  
- Bypassed login and retrieved flag  
- Flag: `picoCTF{s0m3_SQL_6b96db35}`

- ## Irish-Name-Repo 2
- Tried `' OR 1=1--`,got detected  
- Entered `admin'--` to bypass the password check  
- Logged in as admin and retrieved flag  
- Flag: `picoCTF{m0R3_SQL_plz_752d1173}`

- ## Irish-Name-Repo 3
- Inspected hidden form field and changed value to 1  
- Entered `' be 1=1--` based on ROT 13 encryptiom
- Submitted and accessed admin panel  
- Flag: `picoCTF{3v3n_m0r3_SQL_d490b67d}`

- ## JaWT Scratchpad
- Logged in with any user to get JWT cookie  
- Decoded JWT header and payload using jwt.io  
- Used `jwtcrack` and `rockyou.txt` to bruteforce secret ,found `ilovepico`  
- Modified payload to `{"user":"admin"}`, resigned with secret  
- Replaced cookie and refreshed page  
- Flag: `picoCTF{jawt_was_just_what_you_thought_9ed4519dee8140de7a186a5df5a08d6e}`

- ## Secrets
- Opened site and inspected source  
- Navigated to `/secret/` > `/hidden/` > `/hiddenagain/` etc.  
- Found hidden flag using view-source on final page  
- Flag: `picoCTF{succ3ss_@h3n1c@10n_34327aaf}`

- ## Client-side-again
- Opened the challenge page and inspected JavaScript  
- Printed obfuscated array contents to console  
- Reconstructed logic to extract substrings used in flag  
- Guessed correct flag from those pieces  
- Flag: `picoCTF{not_this_again_ef49bf}`

- ## Who are you?
- Used Burp or extension to modify request headers:  
  - `User-Agent: PicoBrowser`  
  - `Referer: mercury.picoctf.net`  
  - `Date: 2018-...`  
  - `DNT: 1`  
  - `X-Forwarded-For: 31.3.152.55`  
  - `Accept-Language: sv`  
- Reloaded page and accessed flag  
- Flag: `picoCTF{http_h34d3rs_v3ry_c0Ol_much_w0w_0da16bb2}`

- ## IntroToBurp
- Submitted any OTP code  
- Captured request with Burp and sent to Repeater  
- Removed `otp` parameter from POST body  
- Sent modified request and received flag in response  
- Flag: `picoCTF{#0TP_Bypvss_SuCc3$S_6bffad21}`

## Java Script Kiddie 1
- Retrieved `bytes` array from AJAX request  
- Wrote Python script to brute-force 16-digit key  
- Used correct key on form to generate PNG  
- Scanned image with `zbarimg`  
- Flag: `picoCTF{905765bf9ae368ad98261c10914d894e}`

## Java Script Kiddie 2
- Retrieved obfuscated `bytes` again  
- Wrote script to try 32-char key (only even indices matter)  
- Found valid key, submitted form, saved output PNG  
- Scanned with `zbarimg` to get flag  
- Flag: `picoCTF{f1ee7ff44419a675d1a0f0a1a91dff4c}`
