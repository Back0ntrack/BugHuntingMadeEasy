---
icon: windows
layout:
  width: default
  title:
    visible: true
  description:
    visible: true
  tableOfContents:
    visible: true
  outline:
    visible: true
  pagination:
    visible: true
  metadata:
    visible: true
  tags:
    visible: true
  actions:
    visible: true
---

# Windows Fundamentals

## Operating System Structure&#x20;

In Windows operating systems, the root directory is \<drive\_letter>:\ (commonly C drive). The root directory (also known as the boot partition) is where the operating system is installed.

<table data-search="false"><thead><tr><th width="129.20001220703125">Directory</th><th width="612.4000854492188">Function</th></tr></thead><tbody><tr><td>Perflogs</td><td>Can hold Windows performance logs but is empty by default.</td></tr><tr><td>Program Files</td><td>On 32-bit systems, all 16-bit and 32-bit programs are installed here. On 64-bit systems, only 64-bit programs are installed here.</td></tr><tr><td>Program Files (x86)</td><td>32-bit and 16-bit programs are installed here on 64-bit editions of Windows.</td></tr><tr><td>ProgramData</td><td>This is a hidden folder that contains data that is essential for certain installed programs to run. This data is accessible by the program no matter what user is running it.</td></tr><tr><td>Users</td><td>This folder contains user profiles for each user that logs onto the system and contains the two folders Public and Default.</td></tr><tr><td>Default</td><td>This is the default user profile template for all created users. Whenever a new user is added to the system, their profile is based on the Default profile.</td></tr><tr><td>Public</td><td>This folder is intended for computer users to share files and is accessible to all users by default. This folder is shared over the network by default but requires a valid network account to access.</td></tr><tr><td>AppData</td><td><p>Per user application data and settings are stored in a hidden user subfolder (i.e., <code>john_doe\AppData</code>). Each of these folders contains three subfolders. </p><ul><li>The <code>Roaming</code> folder contains machine-independent data that should follow the user's profile, such as custom dictionaries. </li><li>The <code>Local</code> folder is specific to the computer itself and is never synchronized across the network. </li><li><code>LocalLow</code> is similar to the Local folder, but it has a lower data integrity level. Therefore it can be used, for example, by a web browser set to protected or safe mode.</li></ul></td></tr><tr><td>Windows</td><td>The majority of the files required for the Windows operating system are contained here.</td></tr><tr><td>System, System32, SysWOW64</td><td>Contains all DLLs required for the core features of Windows and the Windows API. The operating system searches these folders any time a program asks to load a DLL without specifying an absolute path.</td></tr><tr><td>WinSxS</td><td>The Windows Component Store contains a copy of all Windows components, updates, and service packs.</td></tr></tbody></table>

Integrity Control Access Control List (icacls)


**`icacls`** is a Windows command-line tool used to **view and manage NTFS file and folder permissions**, just like the **Security** tab in File Explorer.

<figure><img src="../../.gitbook/assets/image (948).png" alt=""><figcaption></figcaption></figure>

### Common Permission Meanings

<table data-search="false"><thead><tr><th width="103.5999755859375">Permission</th><th width="562">Meaning</th></tr></thead><tbody><tr><td><strong>F</strong></td><td><strong>Full Control</strong> – Read, write, modify, execute, change permissions, and take ownership.</td></tr><tr><td><strong>M</strong></td><td><strong>Modify</strong> – Read, write, execute, and delete files/folders.</td></tr><tr><td><strong>RX</strong></td><td><strong>Read &#x26; Execute</strong> – View contents and run programs.</td></tr><tr><td><strong>R</strong></td><td><strong>Read</strong> – View/open files and folders only.</td></tr><tr><td><strong>W</strong></td><td><strong>Write</strong> – Create or modify files and folders.</td></tr><tr><td><strong>D</strong></td><td><strong>Delete</strong> – Delete the file or folder.</td></tr><tr><td><strong>N</strong></td><td><strong>No Access</strong> – Denies all access.</td></tr></tbody></table>

### Inheritance Settings&#x20;

<table><thead><tr><th width="103.60003662109375">Setting</th><th width="565.2000122070312">Meaning</th></tr></thead><tbody><tr><td><strong>(I)</strong></td><td>Inherited permission (from parent folder).</td></tr><tr><td><strong>(OI)</strong></td><td>Object Inherit – Applies to <strong>files</strong> inside the folder. (not applied on subfolders)</td></tr><tr><td><strong>(CI)</strong></td><td>Container Inherit – Applies to <strong>subfolders</strong>. </td></tr><tr><td><strong>(IO)</strong></td><td>Inherit Only – Doesn't apply to the current object; only to child objects.</td></tr><tr><td><strong>(NP)</strong></td><td>No Propagate – Child objects inherit the permission, but it isn't passed further.</td></tr></tbody></table>

#### Grant Permission to a file&#x20;

{% code overflow="wrap" %}
```cmd
C:\htb> icacls c:\users /grant joe:f
processed file: c:\users
Successfully processed 1 files; Failed processing 0 files
```
{% endcode %}

## NTFS File System&#x20;

NTFS (**New Technology File System**) is the default file system used by modern Windows operating systems.

### Share Permissions&#x20;

<table><thead><tr><th width="154.79998779296875">Permission</th><th>Description</th></tr></thead><tbody><tr><td><code>Full Control</code></td><td>Users are permitted to perform all actions given by Change and Read permissions as well as change permissions for NTFS files and subfolders</td></tr><tr><td><code>Change</code></td><td>Users are permitted to read, edit, delete and add files and subfolders</td></tr><tr><td><code>Read</code></td><td>Users are allowed to view file &#x26; subfolder contents</td></tr></tbody></table>

### NTFS Basic Permissions&#x20;

<table data-search="false"><thead><tr><th width="201.20001220703125">Permission</th><th>Description</th></tr></thead><tbody><tr><td><code>Full Control</code></td><td>Users are permitted to add, edit, move, delete files &#x26; folders as well as change NTFS permissions that apply to all allowed folders</td></tr><tr><td><code>Modify</code></td><td>Users are permitted or denied permissions to view and modify files and folders. This includes adding or deleting files</td></tr><tr><td><code>Read &#x26; Execute</code></td><td>Users are permitted or denied permissions to read the contents of files and execute programs</td></tr><tr><td><code>List folder contents</code></td><td>Users are permitted or denied permissions to view a listing of files and subfolders</td></tr><tr><td><code>Read</code></td><td>Users are permitted or denied permissions to read the contents of files</td></tr><tr><td><code>Write</code></td><td>Users are permitted or denied permissions to write changes to a file and add new files to a folder</td></tr><tr><td><code>Special Permissions</code></td><td>A variety of advanced permissions options</td></tr></tbody></table>

### NTFS Special Permissions&#x20;

<table data-search="false"><thead><tr><th width="279.60003662109375">Permission</th><th width="437.99993896484375">Description</th></tr></thead><tbody><tr><td><code>Full control</code></td><td>Users are permitted or denied permissions to add, edit, move, delete files &#x26; folders as well as change NTFS permissions that apply to all permitted folders</td></tr><tr><td><code>Traverse folder / execute file</code></td><td>Users are permitted or denied permissions to access a subfolder within a directory structure even if the user is denied access to contents at the parent folder level. Users may also be permitted or denied permissions to execute programs</td></tr><tr><td><code>List folder/read data</code></td><td>Users are permitted or denied permissions to view files and folders contained in the parent folder. Users can also be permitted to open and view files</td></tr><tr><td><code>Read attributes</code></td><td>Users are permitted or denied permissions to view basic attributes of a file or folder. Examples of basic attributes: system, archive, read-only, and hidden</td></tr><tr><td><code>Read extended attributes</code></td><td>Users are permitted or denied permissions to view extended attributes of a file or folder. Attributes differ depending on the program</td></tr><tr><td><code>Create files/write data</code></td><td>Users are permitted or denied permissions to create files within a folder and make changes to a file</td></tr><tr><td><code>Create folders/append data</code></td><td>Users are permitted or denied permissions to create subfolders within a folder. Data can be added to files but pre-existing content cannot be overwritten</td></tr><tr><td><code>Write attributes</code></td><td>Users are permitted or denied to change file attributes. This permission does not grant access to creating files or folders</td></tr><tr><td><code>Write extended attributes</code></td><td>Users are permitted or denied permissions to change extended attributes on a file or folder. Attributes differ depending on the program</td></tr><tr><td><code>Delete subfolders and files</code></td><td>Users are permitted or denied permissions to delete subfolders and files. Parent folders will not be deleted</td></tr><tr><td><code>Delete</code></td><td>Users are permitted or denied permissions to delete parent folders, subfolders and files.</td></tr><tr><td><code>Read permissions</code></td><td>Users are permitted or denied permissions to read permissions of a folder</td></tr><tr><td><code>Change permissions</code></td><td>Users are permitted or denied permissions to change permissions of a file or folder</td></tr><tr><td><code>Take ownership</code></td><td>Users are permitted or denied permission to take ownership of a file or folder. The owner of a file has full permissions to change any permissions</td></tr></tbody></table>

{% hint style="info" %}
_The permissions at the NTFS level provide administrators much more granular control over what users can do within a folder or file._
{% endhint %}

