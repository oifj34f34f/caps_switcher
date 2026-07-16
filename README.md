# capswitch
Switch keyboard layouts using the <kbd>Caps Lock</kbd>, and use the standard Caps Lock function by pressing <kbd>Shift</kbd>+<kbd>Caps Lock</kbd>.

Supports Windows only.

## Quick Start
Downloads, registers a logon task (requesting admin elevation via UAC), and launches automatically:
```powershell
Start-Process powershell -Verb RunAs -WindowStyle Hidden -ArgumentList '-NoProfile -Command "New-Item -ItemType Directory -Force $env:LOCALAPPDATA\capswitch | Out-Null; iwr https://github.com/oifj34f34f/capswitch/releases/latest/download/capswitch.exe -OutFile $env:LOCALAPPDATA\capswitch\capswitch.exe; schtasks /create /tn Capswitch /sc ONLOGON /tr $env:LOCALAPPDATA\capswitch\capswitch.exe /rl HIGHEST /delay 0000:30 /f; Start-Process $env:LOCALAPPDATA\capswitch\capswitch.exe"'
```
A UAC prompt will appear — approve it to allow the elevated install.

<details>
<summary>Install manually</summary>

Download the binary from the [releases page](https://github.com/oifj34f34f/capswitch/releases/latest), place it at `%LOCALAPPDATA%\capswitch\capswitch.exe`, then open PowerShell **as Administrator** and create a logon task:
```powershell
schtasks /create /tn Capswitch /sc ONLOGON /tr "%LOCALAPPDATA%\capswitch\capswitch.exe" /rl HIGHEST /delay 0000:30 /f
```
</details>

## Build
Requires Visual Studio Build Tools with MSVC and Windows 11 SDK. Install via winget:
```powershell
winget install -e --id Microsoft.VisualStudio.BuildTools --override "--passive --wait --add Microsoft.VisualStudio.Component.VC.Tools.x86.x64 --add Microsoft.VisualStudio.Component.Windows11SDK.28000"
```
Then run in Developer PowerShell:
```powershell
cl /O1 /Os /GS- /GL capswitch.c /link /SUBSYSTEM:WINDOWS /NODEFAULTLIB /ENTRY:RawEntryPoint /STACK:65536 /LTCG /OPT:REF,ICF /MERGE:.rdata=.text kernel32.lib user32.lib
```

## Uninstall
```powershell
Stop-Process -Name capswitch -Force -ErrorAction SilentlyContinue; schtasks /Delete /TN "Capswitch" /F *>$null; Remove-Item "$env:LOCALAPPDATA\capswitch" -Recurse -Force -ErrorAction SilentlyContinue
```

---
*Written by AI.*
