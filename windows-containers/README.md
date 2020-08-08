# Windows Containers #

## Create Ubuntu VM ##
* Enable Guest Agent in VM options
* sudo apt-get install qemu-guest-agent
* sudo systemctl start qemu-guest-agent

## Enable WinRM ##

```
powershell

Enable-PsRemoting -Force
netsh advfirewall firewall add rule name="WinRM-HTTP" dir=in localport=5985 protocol=TCP action=allow
netsh advfirewall firewall add rule name="WinRM-HTTPS" dir=in localport=5986 protocol=TCP action=allow

winrm set winrm/config/service/auth '@{Basic="true"}'
winrm set winrm/config/client/auth '@{Basic="true"}'
winrm set winrm/config/service '@{AllowUnencrypted="true"}'

```
