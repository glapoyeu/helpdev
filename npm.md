# How can I update npm on Windows?
First, ensure that you can execute scripts on your system by running the following command from an elevated PowerShell. To run PowerShell as Administrator, click Start, search for PowerShell, right-click PowerShell and select Run. Note (Run PowerShell administrator)
```
Set-ExecutionPolicy Unrestricted -Scope CurrentUser -Force


npm install --global --production npm-windows-upgrade
npm-windows-upgrade

npm-windows-upgrade --npm-version latest

Set-ExecutionPolicy Unrestricted -Scope CurrentUser -Force
npm install -g npm-windows-upgrade

```