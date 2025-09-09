# Broken Access Control

Access control determines whether the user is allowed to carry out the action they are attempting to perform. Broken access control allows us to access/modify data beyond limits.&#x20;

## Access Control Security Models

Access control models define **how access to systems and data is managed**. These models are used in operating systems, databases, applications, and networks.

#### 1. **Programmatic Access Control**

* Access rights are stored in a database.
* Access is granted based on roles, groups, or users.
* Very flexible and detailed control.

#### 2. **Discretionary Access Control (DAC)**

* Resource **owners** decide who gets access.
* Access can be shared or delegated.
* Offers detailed control but can get complicated.

#### 3. **Mandatory Access Control (MAC)**

* Access is **centrally controlled**, not by users.
* Often used in military or classified systems.
* Users **cannot** change permissions.

#### 4. **Role-Based Access Control (RBAC)**

* Users are given roles, and roles have specific permissions.
* Easy to manage: just add or remove users from roles.
* Works best when roles are well-planned and not too many.

## Types of Access Control&#x20;

#### **Vertical Access Control**

* **Restricts access based on user roles.**
* Different roles (e.g., admin vs. regular user) get different permissions.
* Example: Admin can delete any account; regular users can't.

🔸 Used to enforce **least privilege** and **separation of duties**.

#### **Horizontal Access Control**

* **Restricts users to their own data.**
* Everyone has the same type of access, but only to **their own resources**.
* Example: A user can view their own bank account, not others.

#### **Context-Dependent Access Control**

* **Access depends on the current state or actions of the user.**
* Prevents doing things out of order.
* Example: You can't change your cart **after** making a payment.

***

## Vertical Privilege Escalation

If a user can gain access to functionality that they are not permitted to access then this is vertical privilege escalation.&#x20;

### Unprotected admin Functionality

* _Forced browsing the `/admin` panel without logging in._
* &#x20;_Sensitive directories commented in JS files such as `user-control`, `administrator-panel`._&#x20;

### Parameter-based access control methods

* _Modifying the `isAdmin` parameter from false to true._&#x20;
* _If sensitive parameters like role ID or API key are leaked in the response but not sent in the request, try reusing them in your request to escalate privileges (e.g., modify role ID along with editable fields)._
* _When direct access to `/admin` is denied, try bypassing with headers like `X-Original-URL: /admin` in a POST request to `/`, which may trick the server into granting access._
* _When direct access to `/admin` is denied, try bypassing with various HTTP methods such as `GET`, `HEAD`._&#x20;

## Horizontal Privilege Escalation

* _Try to access profile page of another user by changing id to username of that user._&#x20;
* _Try to change email id or some other parameters of other user._&#x20;
* _Sometime a unique id is being used to access the particular user profile. In that case we can found this sometime in within the blogs._
* _Even if access is denied and redirected to login, sensitive data may still leak in the response, making the attack successful._

## Horizontal to Vertical Privilege Escalation

_The key lesson: **map out all steps and endpoints where privilege-sensitive actions occur**. Instead of testing everything randomly, focus on the areas that deal with user roles, account settings, or privilege escalation paths. Always test whether sensitive functions are protected server-side, not just hidden from the UI. Think like a black-box attacker: **"What if I just directly access that final step?"**—this mindset often reveals overlooked flaws._



_Some developers rely on the **Referrer header** for access control, assuming users will only reach sensitive steps through the intended flow. However, this is insecure—**attackers can easily manipulate the Referrer header** to bypass such checks and access restricted functionality directly. Always enforce access control on the server-side using proper authentication and authorization, not client-supplied headers._

***

## Exploitation

### Some Common steps while looking for Broken Access Control Vulnerability

* **Understand User Roles**\
  Identify different user roles (e.g., guest, user, admin) and what actions each is allowed to perform.
* **Log In as Multiple Users**\
  Use at least two valid accounts (e.g., normal user and admin or another normal user) to compare functionalities and access.
* **Map Key Endpoints**\
  Interact with the application and capture all privilege-sensitive endpoints (e.g., user settings, admin panels, role changes).
* **Test Direct Access (Forced Browsing)**\
  Try accessing restricted endpoints directly via the browser or tools like Burp. Look for:
  * `/admin`
  * `/deleteUser?user=xyz`
  * `/promoteUser?user=xyz`

> _<mark style="color:red;">When testing for direct access or forced browsing vulnerabilities, it's highly effective to check each endpoint both as an unauthenticated user and as an authenticated user. This helps identify missing or inconsistent access control checks.</mark>_

* **Parameter Tampering**\
  Modify parameters like `userId`, `role`, `account`, `isAdmin`, etc., and observe if unauthorized actions are allowed.
* **Log and Document Everything**\
  Note every bypass attempt, successful or not, and record the expected vs actual behavior for accurate reporting.
