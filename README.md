# nthuobs_rsync

This file explains how to synchronize observation data on the workstation & monitor (Win 10) in NTHU Observatory to CICA cluster (Linux) in NTHU IoA by rsync.

## Installation of Cygwin on Windows
Download Cygwin (https://cygwin.com/index.html, setup-x86_64.exe) to workstation.
1. Double click on `setup-x86_64.exe` to start installation.
1. Click `Next`.
1. Choose `Install from Internet`.
1. Root Directory `C:\cygwin64`, install for `All Users`.
1. Local Package Directory `C:\cygwin64`.
1. Select Your Internet Connections `Use System Proxy Settings`.
1. Choose A Download Site `http://ftp.ntu.edu.tw`.
1. There will be some default plugins, choose additional plugins `Net > openssh, Devel > git, Editors > vim, Admin > cygrunsrv, Net > rsync` in their newest version.
1. Install & Finish.

`Note:` If you want to install other functions/packages in the future, run `setup-x86_64.exe` again and choose the ones you need.

### Setup environment variables on Windows
#### Set user password
`Note:` If you have set user password already, skip this step.
1. Right click on "This PC" -> `Manage`.
1. `System Tools` -> `Local Users and Groups` -> `Users`.
1. Right click on your user, here `observatory` -> `Set Password` -> `Proceed` -> type new password.

#### Environment variable
1. Right click on "This PC" -> `Properties` -> `Advanced System Settings`.
1. `Advanced` tab -> `Environment Variables`.
1. `System variables` -> `New` -> Variable name: CYGWIN, Variable value: ntsec tty.
1. In `System variables`, find `PATH` -> `Edit` -> `New` -> `C:\cygwin64\bin\`.

### Setup of SSH
1. Run Cygwin as administrator.
1. Type `ssh-host-config`, it will generate some key files automatically.
1. It will ask `Should StrictModes be used? (yes/no)` -> `yes`.
1. Activate ssh `net start sshd`.
1. Syncronize users and password of Cygwin and Windows.
    ```
    mkpasswd -cl > /etc/passwd
    mkgroup --local > /etc/group
    ```
1. Test if ssh works `ssh localhost`.

#### SSH server on Windows
(https://virtualizationreview.com/articles/2020/05/21/ssh-server-on-windows-10.aspx)
If `net start sshd` doesn't work, it means you haven't install ssh server on windows.
1. Go to `Windows Settings`, `Apps`, `Optional features`, click `Add a feature`, select `OpenSSH Server`, and click `Install`.
2. Launch Powershell as Administrator.
3. See the status of the service: `Get-Service sshd`.
4. Start the service: `Start-Service sshd`.
5. Verify that the service was running: `Get-Service sshd` and make sure the status is 'Running'.
6. Let SSH service to start every time the system booted up: `Set-Service -Name sshd -StartupType 'Automatic'`.
7. To check and make sure that the port for the SSH server was open: `Get-NetFirewallRule -Name *ssh*`.
8. Test if ssh works `ssh localhost`.

### Firewall setup
Allow connection from outside.

1. `Control Panel` -> `System and Security` -> `Windows Defender Firewall` -> `Advanced settings`.
1. `Inbound Rules` ->  create `New Rule`.
1. `Rule type: Port`, `Protocol and Ports: TCP, Specific local ports: 22`, `Action: Allow the connection`, `Profile: Domain, Private, Public`, `Name: OpenSSH SSH Server (sshd)`

## Rsync
`Note:` Rsync requires installation on both machines. If rsync is not installed on your backup destination (perhaps a Linux machine), please install it.

Rsync is a useful tool in doing remote backup. There are different types of backup that can be choose using rsync while scp can only do full backup.  
* Full backup copies the full data set to the destination which consumes time and storage space.  
* Incremental backup only back up the files that are changed since the last backup.   
* Differential backup is similar to incremental backup while it compares with the last full backup rather than any type of backup.  

This is how rsync works:
```
rsync -[option] source destination
```
If it requires login to other machines, it will ask you for password after this command.
We want to backup files from workstation (Windows) to CICA (Linux).
For safety issues, we give commands on CICA:
```
rsync -avP observatory@140.114.80.236:/[directory]/ /data/nthuobs/[directory]/

-a, –archive        archive files and directory while synchronizing ( -a equal to following options -rlptgoD)
-v, –verbose        Verbose output
-P, –progress       show the sync progress during transfer
```

## Cron
The cron service can schedule tasks on a repetitive basis. We can put our .sh files in cron job to run daily backups.
To do automatic backups, we don't want to type password every time so we use sshpass.
```
sshpass -p [password] rsync -avP observatory@140.114.80.236:/[directory]/ /data/nthuobs/[directory]/
```
Put the command lines in a .sh file, set the path of .sh file to cron and it will do automatic backup every certain time.  
`Note:` Give Andrew this .sh file and he will help with the cron part.

## References
1. https://faqbook.net/cygwin-install-ssh
1. https://searchdatabackup.techtarget.com/tip/Data-backup-types-explained-Full-incremental-differential-and-incremental-forever-backup
1. https://www.linuxtechi.com/rsync-command-examples-linux/

Created on 2020/08/16.
Last revised on 2021/03/29.
