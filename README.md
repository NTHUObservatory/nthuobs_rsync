# nthuobs_rsync
This file explains how to synchronize observation data on the workstation (Win 10) in NTHU Observatory to CICA cluster (Linux) in NTHU IoA by rsync.

## Install OpenSSH server and client on Windows 10 1809
We use Windows PowerShell as our shell so we can run Linux commands.

### Install OpenSSH
1. Find `Windows PowerShell` in Windows App, run as administrator.
1. Check if OpenSSH features are avaliable for install
    ```
    Get-WindowsCapability -Online | ? Name -like 'OpenSSH*'
    
    # This should return the following output:
    
    Name  : OpenSSH.Client~~~~0.0.1.0
    State : NotPresent
    Name  : OpenSSH.Server~~~~0.0.1.0
    State : NotPresent
    ```
1. Install the server and/or client features
    ```# Install the OpenSSH Client
    Add-WindowsCapability -Online -Name OpenSSH.Client~~~~0.0.1.0

    # Install the OpenSSH Server
    Add-WindowsCapability -Online -Name OpenSSH.Server~~~~0.0.1.0

    # Both of these should return the following output:
    
    Path          :
    Online        : True
    RestartNeeded : False
    ```
### Initialize SSH server
1. `Start-Service sshd`
1. Start up sshd service when reboot: `Set-Service -Name sshd -StartupType 'Automatic'`
1. Set firewall rule
    ```
    Get-NetFirewallRule -Name *ssh*
    New-NetFirewallRule -Name sshd -DisplayName 'OpenSSH Server (sshd)' -Enabled True -Direction Inbound -Protocol TCP -Action Allow -LocalPort 22
    ```
One could connect to workstation by ssh now.
```
ssh observatory@140.114.80.236
```

#### Set user password
PowerShell inherits Window users, setup users and password there.
1. Right click on `This PC` -> `Manage`.
1. `Local Users and Groups` -> `Users`.
1. Right click on the user you want to set, here `observatory` -> `Set Password` -> enter your new password.
