# nthuobs_rsync

This file explains how to synchronize observation data on the workstation (Win 10) in NTHU Observatory to CICA cluster (Linux) in NTHU IoA by rsync.

## Installation of Cygwin on Windows
Download Cygwin (https://cygwin.com/index.html, setup-x86_64.exe) to workstation.
1. Double click on `setup-x86_64.exe` to start installation.
1. Click `Next`.
1. Choose `Install from Internet`.
1. Root Directory `C:\cygwin64`, install for `All Users`.
1. Local Package Directory `C:\cygwin64`.
1. Select Your Internet Connections `Use System Proxy Settings`.
1. Choose A Download Site `http://ftp.ntu.edu.tw`.
1. Choose plugins `Net > openssh, Devel > git, Editors > vim, Admin > cygrunsrv` in their newest version.
1. Install & Finish.

## Setup environment variables on Windows
### Set user password
1. Right click on "This PC" -> `Manage`.
1. `System Tools` -> `Local Users and Groups` -> `Users`.
1. Right click on your user, here `observatory` -> `Set Password` -> `Proceed` -> type new password.

### Environment variable
1. Right click on "This PC" -> `Properties` -> `Advanced System Settings`.
1. `Advanced` tab -> `Environment Variables`.
1. `System variables` -> `New` -> Variable name: CYGWIN, Variable value: ntsec tty.
1. In `System variables`, find `PATH` -> `Edit` -> `New` -> `C:\cygwin64\bin\`.

## Setup of SSH
1. Run Cygwin as administrator.
1. Type `ssh-host-config`, it will create some files automatically.
1. It will ask `Should StrictModes be used? (yes/no)` -> `yes`.
1. Activate ssh `net start sshd`.
1. Syncronize users and password of Cygwin and Windows.
    1. `mkpasswd-cl > /etc/passwd`
    1. `mkgroup --local > /etc/group`
1. Test if ssh works `ssh localhost`.

## Firewall setup
