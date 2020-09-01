# nthuobs_rsync
This file explains how to synchronize observation data on the workstation (Win 10) in NTHU Observatory to CICA cluster (Linux) in NTHU IoA by rsync.

## Installation of Cygwin on Windows
Download Cygwin (https://cygwin.com/index.html, setup-x86_64.exe) to workstation.
1. Double click on `setup-x86_64.exe` to start installation.
1. Click `Next`.
1. Choose `Install from Internet`.
1. Root Directory `C:\cygwin64`, install for `All Users`.
1. Local Package Directory `C:\Users\observatory\Downloads`.
1. Select Your Internet Connections `Use System Proxy Settings`.
1. Choose A Download Site `http://ftp.ntu.edu.tw`.
1. Choose plugins `Net > openssh, Devel > git, Editors > vim, Admin > cygrunsrv` in their newest version.
1. Install & Finish.

