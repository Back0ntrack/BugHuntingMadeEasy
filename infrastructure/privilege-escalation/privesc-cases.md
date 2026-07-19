# Privesc Cases

## Bypassing UAC (msconfig)

### Scenario&#x20;

* Machine: **BAKASURA**
* Two local user accounts are present:
  * **arjun** → Member of the **Administrators** group.
  * **backup** → Member of the **Remote Desktop Users** group (non-administrative user).
* The **backup** user does **not** have local administrator privileges.
* Interactive logon credentials for the **arjun** account have been stored on the system (saved credentials / Windows Credential Manager).
* After obtaining access as **backup**, the saved credentials are abused to launch a command prompt as **arjun**.
* Although the new command prompt is running under the **arjun** account, it is **not elevated** because **User Account Control (UAC)** starts administrator accounts with a filtered access token by default.
* As a result, administrative-only tools such as **Mimikatz** cannot be used successfully from the initial command prompt.
* An **auto-elevated Windows binary (`msconfig.exe`)** is then used to obtain an **elevated Administrator Command Prompt**.
* The elevated command prompt now possesses the full administrative access token.
* **Mimikatz** is executed from the elevated command prompt.
* Credentials stored on the system are successfully extracted.

### Key Learning&#x20;

* Membership in the **Administrators** group does **not** automatically provide an elevated process.
* **UAC** separates a user's identity from the privileges assigned to a running process.
* Saved credentials can be leveraged to execute commands as another user, but additional elevation may still be required.
* Auto-elevated Windows binaries can be abused in certain UAC configurations to obtain an elevated administrative shell.

### Practical&#x20;

1. We got RDP into `backup` user and we can see that we need arjun credentials for getting administrator cmd access.&#x20;

<figure><img src="../../.gitbook/assets/image (1678).png" alt=""><figcaption></figcaption></figure>

2. `backup` user didn't have any high level privileges.&#x20;

<figure><img src="../../.gitbook/assets/image (1679).png" alt=""><figcaption></figcaption></figure>

3. We've interactive logon credentials for the user `administrator`. But since our user didn't have elevated privileges mimikatz can't dump the password of the administrator.&#x20;

<figure><img src="../../.gitbook/assets/image (1680).png" alt=""><figcaption></figcaption></figure>

4. We cannot dump the credentials.&#x20;

<figure><img src="../../.gitbook/assets/image (1681).png" alt=""><figcaption></figcaption></figure>

5. Using `runas` we got cmd of arjun.&#x20;

<figure><img src="../../.gitbook/assets/image (1682).png" alt=""><figcaption></figcaption></figure>

6. Even though we're running shell for ajun we didn't get high privileges due to UAC.&#x20;

<figure><img src="../../.gitbook/assets/image (1683).png" alt=""><figcaption></figcaption></figure>

7. Since we didn't have `SeDebugPrivilege` we're not able to execute and dump credential manager.&#x20;

<figure><img src="../../.gitbook/assets/image (1684).png" alt=""><figcaption></figcaption></figure>

8. Using `msconfig` ⇒ `Tools` ⇒ `Select Command Prompt` ⇒ `Launch`. Thus using msconfi utility we're able to execute elevated shell.&#x20;

<figure><img src="../../.gitbook/assets/image (1685).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../.gitbook/assets/image (1686).png" alt=""><figcaption></figcaption></figure>

9. Executing mimikatz with elevated privileges.&#x20;

<figure><img src="../../.gitbook/assets/image (1687).png" alt=""><figcaption></figcaption></figure>

10. We can see that in the shell gained from `backup` user we can't change the password of the `arjun` user but with the elevated shell from msconfig we can change the password of the `arjun` user.&#x20;

<figure><img src="../../.gitbook/assets/image (1689).png" alt=""><figcaption></figcaption></figure>
