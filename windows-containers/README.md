# Windows Containers #

## Install Windows Server 2019 VM ##
### Doẃnload eval version and create VM ###
* https://www.microsoft.com/de-de/evalcenter/evaluate-windows-server-2019?filetype=ISO
* Upload ISO to Proxmox
* Start VM (4 Cores, 16GB Ram)

### Windows installer ###
* Select 'Windows Server 2019 Datacenter Evaluation'
* Custom Install Windows only
* Reboot VM

### Install Docker ###

https://blog.sixeyed.com/getting-started-with-docker-on-windows-server-2019/

``` 
powershell
Install-WindowsFeature -Name Containers
Uninstall-WindowsFeature Windows-Defender
Restart-Computer -Force
```

### Enable WinRM ###

```
powershell

Enable-PsRemoting -Force
netsh advfirewall firewall add rule name="WinRM-HTTP" dir=in localport=5985 protocol=TCP action=allow
netsh advfirewall firewall add rule name="WinRM-HTTPS" dir=in localport=5986 protocol=TCP action=allow

winrm set winrm/config/service/auth '@{Basic="true"}'
winrm set winrm/config/client/auth '@{Basic="true"}'
winrm set winrm/config/service '@{AllowUnencrypted="true"}'

```


## Create Fedora 32 server VM ##


