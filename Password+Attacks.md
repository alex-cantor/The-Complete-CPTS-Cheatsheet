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
    - SAM (Security Account Manager)
      - TODO
