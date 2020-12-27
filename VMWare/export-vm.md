* Open PowerShell terminal
* ```
  Set-ExecutionPolicy Unrestricted
  ```
* ```
  Import-Module VMware.VimAutomation.Core
  ```
* ```
  Set-PowerCLIConfiguration -InvalidCertificateAction Ignore -Confirm:$false
  ```  
* ```
  Connect-VIServer vcenter.sepp.net
  ```  
  Username: administrator@vsphere.local
* ```
  Get-VM -Name <VM NAME> | Shutdown-VMGuest -confirm:$false
  Get-VM -Name <VM NAME> | Get-CDDrive | Set-CDDrive -NoMedia -confirm:$false
  Get-VM -Name <VM NAME> | Export-VApp -Destination '.' -Format OVA
  ```
