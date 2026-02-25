# MSSQL (1433)

(MSSQL) is a **relational database management system (RDBMS)** developed by Microsoft.

## Main Components

* **Database Engine**
  * Query processor
  * Storage engine
* **SQL Server Agent**
  * Job scheduler
* **SSMS**
  * GUI client for administration
  * SQL Server Management Studio
* **SQL Server Browser**
  * Helps clients find named instances

## Instance-Centric Architecture&#x20;

A MSSQL instance is an independent, named installation of the SQL Server database engine service running on a machine, managing its own unique set of system and user databases, security, and dedicated system resources.

```
Windows Service
   └── SQL Instance
         └── Databases
               └── Schemas
                     └── Objects
```

**Each instance:**

* Runs as its own Windows service
* Has its own memory space
* Has its own tempdb
* Has its own security boundary
* Has its own network port

```sql
-- Idenitfy the current instance
select @@servername;
```

<figure><img src="../../.gitbook/assets/image (10) (1).png" alt=""><figcaption></figcaption></figure>

## The Core 4 Databases&#x20;

Every MSSQL instance starts with four system databases that manage the server itself:

* **master**: The boss database; stores server-wide info like logins and configurations. If it corrupts, the server won't start.
* **model**: Template for new databases; any changes here apply to future ones.
* **msdb**: Handles jobs, alerts, and backups via SQL Server Agent.
* **tempdb**: Temporary workspace for sorting and intermediate results; recreated on server restart.

```sql
select name from sys.databases; 
```

<figure><img src="../../.gitbook/assets/image (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

## Authorization Hierarchy

<figure><img src="../../.gitbook/assets/image (2) (1) (1).png" alt=""><figcaption></figcaption></figure>

{% hint style="warning" %}
**Important:**

* _A login can exist without database access._
* _A user cannot exist without a login (except contained DB users)._
{% endhint %}

### Server Level&#x20;

All server level metadata lives in `master` database. Server level catalog views:&#x20;

```
sys.server_principals
sys.server_permissions
sys.server_role_members
```

This is the **entire SQL Server instance level**.

It controls:

* Logins
* Server roles
* Linked servers
* Endpoints
* All databases inside the instance

#### **Examples:**&#x20;

**Create Login:**&#x20;

```sql
CREATE LOGIN app_login WITH PASSWORD = 'StrongPassword123!';
```

**Grant server permission:**&#x20;

```sql
GRANT VIEW SERVER STATE TO app_login;
```

**Add to server role:**&#x20;

```sql
ALTER SERVER ROLE sysadmin ADD MEMBER app_login;
```

### Database Scope&#x20;

Database level catalog view:&#x20;

```
sys.database_principals
sys.database_permissions
sys.database_role_members
```

Inside the server, each database has its own security boundary.

Controls:

* Database users
* Database roles
* Schemas
* Database objects

A login must map to a **database user** to access a database.

#### Examples:&#x20;

**Create Database User**

```sql
USE MyDatabase;
CREATE USER app_user FOR LOGIN app_login;
```

**Grant Operation Permission**&#x20;

```sql
GRANT CREATE TABLE TO app_user;
```

**Add to DB Owner role**&#x20;

```sql
ALTER ROLE db_owner ADD MEMBER app_user;
```

Going down we'll get separate permission on tables, rows and columns.&#x20;

## Authentication

### Windows Authentication&#x20;

* Uses AD
* Uses Kerberos/NTLM
* Identity = Domain user

### SQL Authentication&#x20;

* Local to SQL Server
* Username/Password stored inside SQL
* Example: `sa` user

### Mixed Mode

Mixed Mode = Windows + SQL&#x20;

Thus Mixed Mode Involves:\
✅ Windows Auth\
✅ SQL Auth

## Fixed Server Roles&#x20;

| Role              | What It Controls             | What It Can Do                                                                                                                                        | Security Risk Level                     |
| ----------------- | ---------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------- | --------------------------------------- |
| **sysadmin**      | Entire SQL Server instance   | Full unrestricted control. Bypasses all permission checks. Can read/write any database, enable dangerous features (xp\_cmdshell), impersonate anyone. | 🔥 Critical (Full Server Compromise)    |
| **serveradmin**   | Server configuration         | Change server-wide settings via `sp_configure`, manage memory, shutdown server.                                                                       | High (Configuration Abuse)              |
| **securityadmin** | Logins & server security     | Create/alter/drop logins, reset passwords, grant server permissions, add members to server roles. Can escalate to sysadmin.                           | 🔥 Critical (Privilege Escalation)      |
| **setupadmin**    | Linked servers & replication | Add/remove linked servers and manage replication settings.                                                                                            | High (Lateral Movement Risk)            |
| **processadmin**  | SQL processes                | View and kill running sessions using `KILL`.                                                                                                          | Medium (Operational Impact)             |
| **diskadmin**     | Database files (legacy)      | Manage disk files used by databases. Rarely used in modern deployments.                                                                               | Low                                     |
| **dbcreator**     | Database creation            | Create, alter, drop databases. Can create a DB and grant self db\_owner inside it.                                                                    | High (Controlled DB Ownership)          |
| **bulkadmin**     | Bulk operations              | Execute `BULK INSERT` to import large data files.                                                                                                     | Medium–High (File-based abuse possible) |

## Fixed Database roles&#x20;

| Role                   | Scope                      | What It Can Do                                                                                     | Security Risk Level               |
| ---------------------- | -------------------------- | -------------------------------------------------------------------------------------------------- | --------------------------------- |
| **db\_owner**          | Entire database            | Full control over the database: create/drop objects, manage permissions, backup/restore, run DBCC. | 🔥 Critical (Full DB Compromise)  |
| **db\_datareader**     | All tables/views           | Read (SELECT) all data in the database.                                                            | Medium (Data Exposure)            |
| **db\_datawriter**     | All tables                 | Insert, update, delete data in all user tables.                                                    | Medium–High (Data Integrity Risk) |
| **db\_ddladmin**       | Schema-level changes       | Create, alter, drop database objects (tables, views, procs). Cannot manage security.               | High (Structural Manipulation)    |
| **db\_securityadmin**  | Database security          | Manage users and role membership inside the database. Can escalate to db\_owner.                   | 🔥 High (Privilege Escalation)    |
| **db\_backupoperator** | Backup operations          | Perform database backups. Cannot modify data.                                                      | Medium (Data Exfil via Backup)    |
| **public**             | Default role for all users | Minimal default permissions. Every user belongs to it automatically.                               | Low (unless misconfigured)        |



## Impersonation&#x20;

### IMPERSONATE

`IMPERSONATE` is a permission granted to a principal allowing them to assume another principal’s identity.

It does NOT switch context by itself.\
It only allows switching.

#### Server Level Impersonation&#x20;

```sql
GRANT IMPERSONATE ON LOGIN::sa TO lowpriv_login;
```

This allows `lowpriv_login` to impersonate `sa`.

#### Database Level Impersonation&#x20;

```sql
GRANT IMPERSONATE ON USER::dbo TO app_user;
```

This allows `app_user` to impersonate `dbo` inside that database.

### EXECUTE AS

This is the actual command that switches execution context.

**Switch Login Context:**&#x20;

1. Server Level&#x20;

```sql
EXECUTE AS LOGIN = 'sa';
```

2. Database Level&#x20;

```sql
EXECUTE AS USER = 'dbo';
```

### REVERT

To return back to our normal or low privileged user we can use&#x20;

```sql
REVERT
```

#### Why does SQL allow this ?

Because:

* Developers use it
* Stored procedures need it
* Applications need privilege separation

## Server Configuration

**Server Surface Area control Includes:**&#x20;

* xp\_cmdshell&#x20;
* OLE Automation Procedures&#x20;
* CLR enabled&#x20;
* Cross db ownership chaining&#x20;
* Contained database authentication

All the Server Surface Area Control in MSSQL are controlled using&#x20;

```sql
sp_configure (Server Configuration Mechanism)
```

It is used to view and modify instance-level settings.

**View Advanced Options:**&#x20;

```sql
EXEC sp_configure 'show advanced options', 1;
RECONFIGURE;
```

**View all configurations:**&#x20;

```sql
EXEC sp_configure;
```

**Change a setting:**&#x20;

```sql
EXEC sp_configure 'option_name', value;
RECONFIGURE;

EXEC sp_configure 'xp_cmdshell', 1;
RECONFIGURE;
```



## Linked Servers&#x20;

A **Linked Server** in Microsoft SQL Server allows one SQL Server instance to execute queries against another data source as if it were local.

That data source can be:

* Another SQL Server instance
* Oracle
* MySQL
* PostgreSQL
* Excel
* OLE DB provider
* Even another instance on the same machine

### Creating a Linked Server&#x20;

#### Create Linked Server Object

```sql
EXEC sp_addlinkedserver  
   @server = 'RemoteServer',
   @srvproduct = '',
   @provider = 'SQLNCLI',
   @datasrc = '192.168.1.10';
```

#### Configure Authentication mapping

```sql
EXEC sp_addlinkedsrvlogin  
   @rmtsrvname = 'RemoteServer',
   @useself = 'false',
   @rmtuser = 'sa',
   @rmtpassword = 'Password123';
```

### Authentication Modes in Linked Server&#x20;

Linked Servers support 4 mapping modes:

1. **Use Current Login’s Security Context**
   1. Pass-through authentication
   2. Works with Windows authentication
   3. Uses Kerberos delegation (if configured)
2. **Be Made Without Security Context**
   1. Anonymous
   2. Rarely useful for SQL Server
3. **Be Made Using This Security Context**
   1. Hardcoded username/password
   2. Most common configuration

### Querying Linked Servers

```sql
SELECT * 
FROM OPENQUERY(RemoteServer, 
     'SELECT name FROM master.sys.databases');

EXEC ('SELECT @@version') AT RemoteServer;
```

## Command Execution

In Microsoft SQL Server, operating system command execution from within the database engine is **not enabled by default** and is tightly controlled. The three primary built-in mechanisms that can execute OS-level commands are:

1. `xp_cmdshell`
2. OLE Automation Procedures
3. SQL Server Agent (via Job Steps)

### Xp\_CmdShell

`xp_cmdshell` is an **extended stored procedure** that allows execution of Windows command shell commands (`cmd.exe`) directly from T-SQL.

#### Command Execution User&#x20;

<figure><img src="../../.gitbook/assets/image (3) (1) (1).png" alt=""><figcaption></figcaption></figure>

#### Enable Xp\_CmdShell

```sql
EXEC sp_configure 'show advanced options', 1;
RECONFIGURE;

EXEC sp_configure 'xp_cmdshell', 1;
RECONFIGURE;
```

#### Command Execution&#x20;

```sql
EXEC xp_cmdshell 'whoami';
```

<figure><img src="../../.gitbook/assets/image (5) (1) (1).png" alt=""><figcaption></figcaption></figure>

### OLE Automation Procedures

OLE Automation allows SQL Server to instantiate **COM objects** from T-SQL.

Main procedures:

* `sp_OACreate`
* `sp_OAMethod`
* `sp_OAGetProperty`
* `sp_OADestroy`

Commands executed using this method always runs under SQL Service account. Because&#x20;

* The SQL Server process instantiates the COM object.
* The COM object runs inside SQL Server's process space.

#### Enable OLE Automation Procedures&#x20;

```sql
EXEC sp_configure 'show advanced options', 1;
RECONFIGURE;

EXEC sp_configure 'Ole Automation Procedures', 1;
RECONFIGURE;
```

#### Command Execution

1. **Create COM Object**

```sql
DECLARE @obj INT;
EXEC sp_OACreate 'WScript.Shell', @obj OUTPUT;
```

2. **Call a Method**&#x20;

```sql
EXEC sp_OAMethod @obj, 'Run', NULL, 'cmd.exe /c whoami';
```

<figure><img src="../../.gitbook/assets/image (4) (1) (1).png" alt=""><figcaption></figcaption></figure>

### SQL Server Agent&#x20;

SQL Server Agent is a **Windows service** used for:

* Scheduled jobs
* Maintenance tasks
* Automation

It can execute:

* T-SQL
* PowerShell
* CmdExec (OS commands)
* SSIS packages

{% hint style="danger" %}
_❗ SQL Server Agent is **NOT available in SQL Server Express Edition**._
{% endhint %}

Commands executed using this method always runs under SQL Service account.

#### Command Execution&#x20;

1. **Confirm Agent is running.**&#x20;

```sql
SELECT servicename, status_desc 
FROM sys.dm_server_services
WHERE servicename LIKE '%Agent%';
```

2. **Check which account.**&#x20;

```sql
SELECT servicename, service_account 
FROM sys.dm_server_services;
```

<figure><img src="../../.gitbook/assets/image (6) (1) (1).png" alt=""><figcaption></figcaption></figure>

3. **Create a job.**

```sql
USE msdb;
GO

EXEC sp_add_job 
    @job_name = 'Run_PowerShell_Script';
```

4. **Add PowerShell Job Step**

```sql
EXEC sp_add_jobstep
    @job_name = 'Run_PowerShell_Script',
    @step_name = 'Execute_PS1',
    @subsystem = 'PowerShell',
    @command = 'C:\Users\provideInfo.ps1';
```

5. **Attach Job to server and execute**

```sql
EXEC sp_add_jobserver 
    @job_name = 'Run_PowerShell_Script';

EXEC sp_start_job @job_name = 'Run_PowerShell_Script';
```

<figure><img src="../../.gitbook/assets/image (7) (1) (1).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../.gitbook/assets/image (8) (1) (1).png" alt=""><figcaption></figcaption></figure>
