- "Microsoft's Active Directory" - It simplifies the management of devices and users within a corporate environment.
- **Windows domain** is a group of users and computers under the administration of a given business. The main idea behind a domain is to centralize the administration of common components of a Windows computer network in a single repository called [[Active Directory]] **(AD)**.
- The server that runs the Active Directory services is known as a **Domain Controller (DC)**.
- You can have policies on the Windows domain network. For example you can restrict the access to "Control Panel".
- In a Windows domain, credentials are stored in a centralized repository called: [[Active Directory]]
- The server in charge of running the Active Directory services is called: **Domain Controller**

------

- A **Windows Domain** can facilitate the management of multiple users in a network.
- A **Windows Domain** is a group of users and computers under the administration of a give business. Centralized administration of a Windows computer network in a single repository called **Active Directory** (**AD**).
- The server that runs the **Active Directory** services is known as a **Domain Controller** (**DC**)
- **AD** is a directory service that provides a framework for managing users, computers, groups, and policies in a domain environment." via ChatGPT
- The main advantages of having a configured Windows domain are:
	- **Centralized identity management**: All users across the network can be configured from Active Directory with minimum effort.
	- **Managing security policies**: You can configure security policies directly from Active Directory and apply them to users and computers across the network as needed.
- The core of any **Windows Domain** is the **Active Directory Domain Service (AD DS)**. A catalogue which holds the information of all "objects" that exist on your network, like:
	- **Users**: also know as **security principals**, one of the most common object types in **Active Directory**.
		-  They can be authenticated by the domain and can be assigned privileges over resources like files or printers.
		- **Users** can be used to represent two types of entities:
			-  **People**: users will generally represent persons in your organisation that need to access the network, like employees.
			- **Services**: services can also be defined as users like **IIS** or **MSSQL**. They will only have the privileges needed to run their specific service.
		
		- **Machines**: another type of object within **Active Directory**. For every computer that joins the **Active directory** domain, a machine object will be created.
			- Identifying machine accounts is relatively easy. They follow a specific naming scheme. The machine account name is the computer's name followed by a dollar sign. For example, a machine named `DC01` will have a machine account called `DC01$`.
		
		- **Security Groups**: 
		![[Pasted image 20241225050225.png]]
###### Active Directory Users and Computers
- **Organizational Units (OUs)** are container objects that allow you to classify users and machines.

###### Security Groups vs OUs
- **OUs** are handy for **applying policies** to users and computers, which include specific configurations that pertain to sets of users depending on their particular role in the enterprise. Remember, a user can only be a member of a single OU at a time, as it wouldn't make sense to try to apply two different sets of policies to a single user.
- **Security Groups**, on the other hand, are used to **grant permissions over resources**. For example, you will use groups if you want to allow some users to access a shared folder or network printer. A user can be a part of many groups, which is needed to grant access to multiple resources.


###### Managing Users in AD
- for protection against accidental deletion:
	- View -> Turn on **Advanced Features**
	- Right click on the Organizational Unit -> **Properties** -> disable "Protect object from accidental deletion"

	**Delegation**
	- One of the nice things you can do in AD is to give specific users some control over some OUs. This process is known as **delegation**
	- Right click on the Organizational Unit -> **Delegate Control**
	
	**Set Active Directory account password for a user with prompt**:
	- Powershell: `Set-ADAccountPassword <user> -Reset -NewPassword (Read-Host -AsSecureString -Prompt 'New Password') -Verbose`
	**Force a user to change their password on next logon**:
	- Powershell: `Set-ADUser -ChangePasswordAtLogon $true -Identity <user> -Verbose`

###### Managing Computers in AD
- While there is no golden rule on how to organise your machines, an excellent starting point is segregating devices according to their use. In general, you'd expect to see devices divided into at least the three following categories:
	**1. Workstations**
	- Workstations are one of the most common devices within an Active Directory domain. Each user in the domain will likely be logging into a workstation. This is the device they will use to do their work or normal browsing activities. These devices should never have a privileged user signed into them.  
	
	**2. Servers**
	- Servers are the second most common device within an Active Directory domain. Servers are generally used to provide services to users or other servers.
	
	**3. Domain Controllers**
	- Domain Controllers are the third most common device within an Active Directory domain. Domain Controllers allow you to manage the Active Directory Domain. These devices are often deemed the most sensitive devices within the network as they contain hashed passwords for all user accounts within the environment.

- It is recommended to have a `Workstations` [[OU (Organizational Unit)]] and a `Servers` [[OU (Organizational Unit)]] 

###### Group Policies
- GPOs are simply a collection of settings that can be applied to [[OU (Organizational Unit)]]s. 
- You can create a **group policy object** under "**Group Policy Objects**" and then link it to the [[OU (Organizational Unit)]] yhere you want the policies to apply.
![[gpo.png]]
- Right-click on **Default Group Policy** -> **Edit**: `Computer Configurations -> Policies -> Windows Setting -> Security Settings -> Account Policies -> Password Policy`

###### [[GPO (Group Policy Objects)]] distribution
-  **GPOs** are distributed to the network via a network share called `SYSVOL`GPOs are distributed to the network via a network share called `SYSVOL`
- The **SYSVOL** share points by default to the `C:\Windows\SYSVOL\sysvol\` directory on each of the **DCs** in our network
- `gpupdate /force` - force to update the GPO of a specific machine.
- right click on [[Active Directory]] object -> link existing [[GPO (Group Policy Objects)]] or create a new one
- right click on [[GPO (Group Policy Objects)]] -> edit..

###### Authentication methods
- The credentials are stored in the **Domain Controllers**.
- The service will need to ask the **Domain Controller** to verify if they are correct.
- Two protocols can be used for network authentication in windows domains:
	-  [[Kerberos]]: The default protocol in any recent domain.
	-  NetNTLM: Legacy protocol kept for compatibility purposes.
- [[Kerberos]] uses tickets. A user can use a ticket as proof of a previous authentication.
- [[Kerberos]] authentication process:
	1. The user sends their username and a timestamp encrypted using a key derived from their password to the **Key Distribution Center (KDC)**, a service usually installed on the Domain Controller in charge of creating [[Kerberos]] tickets on the network. The **Key Distribution Center** (**KDC**) will create and send back a [[TGT (Ticket Granting Ticket)]] which will allow the user to request additional tickets to access specific services (from now on, the user can request service tickets without passing their credentials every time they want to connect to a service). The user also receives a **Session Key**, which is used to generate the following requests.
	 ![[kerberos_authentication_process.png]]
	 2. User uses their [[TGT (Ticket Granting Ticket)]] to ask the **Key Distribution Center (KDC)** for a **Ticket Granting Service (TGS)**. To request a TGS, the user will send their username and a timestamp encrypted using the Session Key, along with the TGT and a **Service Principal Name (SPN)**, which indicates the service and server name we intend to access. As a result, the KDC will send us a TGS along with a **Service Session Key**, which we will need to authenticate to the service we want to access. The TGS is encrypted using a key derived from the **Service Owner Hash**. The Service Owner is the user or machine account that the service runs under. The TGS contains a copy of the Service Session Key on its encrypted contents so that the Service Owner can access it by decrypting the TGS.![[kerberos_ticket_granting_service.png]]
	 3. The TGS can then be sent to the desired service to authenticate and establish a connection. The service will use its configured account's password hash to decrypt the TGS and validate the Service Session Key.![[kerberos_authentication_to_the_service.png]]
- **NetNTLM** Authentication: 
	- uses a "challenge-response" mechanism.
	![[AD_NetNTLM_authentication.png]]
	1. The client sends an authentication request to the server they want to access.
	2. The server generates a random number and sends it as a challenge to the client.
	3. The client combines their NTLM password hash with the challenge (and other known data) to generate a response to the challenge and sends it back to the server for verification.
	4. The server forwards the challenge and the response to the Domain Controller for verification.
	5. The domain controller uses the challenge to recalculate the response and compares it to the original response sent by the client. If they both match, the client is authenticated; otherwise, access is denied. The authentication result is sent back to the server.
	6. The server forwards the authentication result to the client.

######  Trees, Forests and Trusts

- Tree structure:  ![[AD_Tree.png]]
- Forest structure:  ![[AD_Forest.png]]

- With trust relationships you allow users to have access to resources from different domains/trees. For example: a user at THM UK can access a shared file in one of MHT ASIA servers if the forests are joined together by **trust relationships**.

![[AD_one_way_trust_relationship.png]]