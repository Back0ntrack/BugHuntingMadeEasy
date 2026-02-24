# Advanced Bypass Techniques

## **Advanced Techniques to Bypass No Rate Limiting, 403 Restrictions**�&#xDEAB;**, and Captcha Validation**🤖

_**When testing web applications, encountering rate limits, 403 restrictions, or CAPTCHA challenges is common. However, improper implementation of these security measures can often be bypassed using various techniques. Below are 14 advanced methods to bypass these restrictions effectively.**_

1️⃣**Customizing HTTP Methods:** _If a request is blocked when using the GET method, try changing it to other HTTP methods such as OPTIONS, HEAD, POST, PUT, DELETE, TRACE, or CONNECT. Some servers may not enforce restrictions on alternative HTTP methods, allowing a bypass._

2️⃣**Using Different HTTP Protocol Versions:** _Some security measures apply only to specific HTTP versions. Switching between HTTP/1.0, HTTP/1.1, HTTP/2.0, or toggling between HTTPS and HTTP can sometimes bypass restrictions._

3️⃣**Changing the User-Agent String:** _Some servers block requests based on specific User-Agent strings. Modifying the User-Agent to mimic another browser or bot can sometimes bypass restrictions._

* _User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/91.0.4472.124 Safari/537.36_
* _User-Agent: Mozilla/5.0 (Macintosh; Intel Mac OS X 10\_15\_7) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/90.0.4430.85 Safari/537.36_
* _User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64; rv:89.0) Gecko/20100101 Firefox/89.0_

4️⃣**Changing the Accept-Encoding Header:** _Servers may enforce restrictions based on specific encodings. Changing or removing the Accept-Encoding header can sometimes bypass these protections._

* _Accept-Encoding: gzip, deflate_
* _Accept-Encoding: br_
* _Accept-Encoding: identity_
* _Accept-Encoding: \*_

5️⃣**Using Different Port Numbers:** _Modifying the protocol in Burp Suite by switching between http:// and https:// may bypass security measures in some cases._

6️⃣**HTTP Header Manipulation to Spoof IP and Evade Detection:** _Modifying HTTP headers to spoof the origin IP can help bypass IP-based rate limiting or geographic restrictions._

* _X-Forwarded-For: 127.0.0.1_
* _X-Forwarded-By: 127.0.0.1_
* _X-Forwarded-For-Original: 127.0.0.1_
* _X-Custom-IP-Authorization: 127.0.0.1_
* _X-Originating-IP: 127.0.0.1_
* _X-Remote-IP: 127.0.0.1_
* _X-Remote-Addr: 127.0.0.1_
* _Forwarded-For: 127.0.0.1_

7️⃣**Case Sensitivity Payloads:** _Some applications enforce rate limits based on case-sensitive checks. Modifying the email address case can sometimes bypass restrictions._

* _**Rate limit applies to:**_ [_victim@gmail.com_](mailto:victim@gmail.com)
* _**Bypass using:**_ [_Victim@gmail.com_](mailto:Victim@gmail.com) _or_ [_VICTIM@gmail.com_](mailto:VICTIM@gmail.com)

8️⃣**IP Rotation Technique:** _Using IP rotation can help bypass rate limits imposed on a single IP._

* _Navigate to the Forgot Password page of the target website._
* _Submit a password reset request using your email address._
* _Capture the Forgot Password request in burp suite._
* _Looped the request 50 times, from 127.0.0.1 till 127.0.0.50 (value of the X-Forwarded-For header)._
* _Check the responses to ensure that the rate limit is being bypassed and that each request is processed as if it came from a different IP address._

9️⃣**Response Manipulation to Bypass Restrictions:** _Modify server responses in Burp Suite to trick the application into bypassing security restrictions._

* _Use Burp Suite to capture the original request when interacting with the target application._
* _After the application blocks your attempts due to rate limiting or 403 restrictions, make multiple further attempts._
* _Capture the blocking request in Burp Suite._
* _Manipulate the response, changing it from invalid to valid, to make it appear as though the restriction has been bypassed._

🔟**URL Encoding and Double URL Encoding:** _Some filters only recognize exact string matches. Encoding or double encoding special characters such as `/`, `?`, `&`, `=`, `@`, `#`, `%`, `+`, `.` can sometimes bypass these restrictions._

1️⃣1️⃣**Null Payload Injection:** _Injecting null characters (%00) in query parameters, headers, or request bodies may trick the application into bypassing security mechanisms_.

* [_test@email.com_](mailto:test@email.com)_%00_
* _session=abcd%00 (used in session or cookie headers)_

1️⃣2️⃣**Reusing a Previous CAPTCHA Solution:** _Some applications do not verify if a CAPTCHA token has already been used. You can reuse a previously solved CAPTCHA to bypass validation._

* _Use Burp Suite to capture the original request containing the captcha challenge._
* _Solve the captcha manually or extract the valid solution from the original request._
* _Intercept a new request with the captcha challenge and replace the solved captcha value with the previous one._
* _Forward the modified request to the server and verify if it bypasses the captcha._

1️⃣3️⃣**Submitting an Empty CAPTCHA:** _If the application does not properly validate CAPTCHA fields, submitting an empty CAPTCHA may sometimes work._

* _Use Burp Suite to capture the request that includes the captcha field._
* _Modify the request by removing or leaving the captcha field empty._
* _Forward the request to the server to check if it successfully bypasses the captcha validation._

1️⃣4️⃣**Removing the CAPTCHA Parameter from the Request:** _Some applications only check for the presence of a CAPTCHA parameter without verifying its validity. Removing the CAPTCHA parameter from the request may bypass validation._

* _Use Burp Suite to capture the request containing the captcha parameter._
* _In the intercepted request, remove the captcha-related parameter entirely._
* _Forward the request to see if the server processes it without validating the captcha._
