# windows10-11_shared_printer_solve
# 1.
```
reg add "HKEY_LOCAL_MACHINE\SOFTWARE\Policies\Microsoft\Windows NT\Printers\RPC" /v RpcUseNamedPipeProtocol /t REG_DWORD /d 1 /f
```
by Registry Edit.
```
Windows Registry Editor Version 5.00

[HKEY_LOCAL_MACHINE\System\CurrentControlSet\Control\Print]
"RpcAuthnLevelPrivacyEnabled"=dword:00000000
```



# 2.

Enable via Group Policy:
```
Computer Configuration > Administrative Templates > Printers > Configure RPC listener settings
Enable and set protocols allowed to be used to RpcOverNamedPipesAndTcp.
```

Enable the setting via the registry:
```
Run reg add "HKEY_LOCAL_MACHINE\SOFTWARE\Policies\Microsoft\Windows NT\Printers\RPC" /v RpcProtocols /t REG_DWORD /d 0x7 /f
```

Final script in reg file 
```
Windows Registry Editor Version 5.00

[HKEY_LOCAL_MACHINE\SOFTWARE\Policies\Microsoft\Windows NT\Printers\RPC]
"RpcUseNamedPipeProtocol"=dword:00000001
"RpcProtocols"=dword:00000007
"ForceKerberosForRpc"=dword:00000001

[HKEY_LOCAL_MACHINE\System\CurrentControlSet\Control\Print]
"RpcAuthnLevelPrivacyEnabled"=dword:00000000
```
# 3.
   A. Disable the SMB client signing requirement:
```
gpedit.msc

b. In the console tree, select Computer Configuration > Windows Settings > Security Settings> Local Policies > Security Options.
c. Double-click Microsoft network client: Digitally sign communications (always).
d. Select Disabled > OK.
```
 
# 4.
   B. Disable the guest fallback protection:
```
gpedit.msc

b. In the console tree, select Computer Configuration > Administrative Templates> Network > Lanman Workstation.
c. Double-click Enable insecure guest logons
d. Select Enabled > OK.
```

# 5.
```
HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\LanmanWorkstation\Parameters

Registry DWORD 32-bit key value = AllowInsecureGuestAuth

'1'
```

# 6. On the Start Menu search, type powershell then under the Windows PowerShell app, click Run as administrator. Accept the elevation prompt.

```
powershell -Command "Start-Process PowerShell -Verb RunAs"
```

b. To disable SMB signing requirement, type:

 
```
Set-SmbClientConfiguration -RequireSecuritySignature $false
```
d. Hit enter, then hit Y to accept.

c. To disable guest fallback, type: 

 
```
Set-SmbClientConfiguration -EnableInsecureGuestLogons $true
```
e. Hit enter, then hit Y to accept.

 



# Final PowerShell ( Run all these on command on PowerShell as Administrator )
```
powershell -Command "Start-Process PowerShell -Verb RunAs"
```
Copy these line 
```
reg add "HKEY_LOCAL_MACHINE\SOFTWARE\Policies\Microsoft\Windows NT\Printers\RPC" /v RpcUseNamedPipeProtocol /t REG_DWORD /d 1 /f
reg add "HKEY_LOCAL_MACHINE\SOFTWARE\Policies\Microsoft\Windows NT\Printers\RPC" /v RpcProtocols /t REG_DWORD /d 0x7 /f
reg add "HKEY_LOCAL_MACHINE\SOFTWARE\Policies\Microsoft\Windows NT\Printers\RPC" /v ForceKerberosForRpc /t REG_DWORD /d 1 /f

reg add "HKEY_LOCAL_MACHINE\System\CurrentControlSet\Control\Print" /v RpcAuthnLevelPrivacyEnabled /t REG_DWORD /d 0 /f
reg add "HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\LanmanWorkstation\Parameters" /v AllowInsecureGuestAuth /t REG_DWORD /d 1 /f

Set-SmbClientConfiguration -RequireSecuritySignature $false -Force
Set-SmbClientConfiguration -EnableInsecureGuestLogons $true -Force
wusa /uninstall /kb:5072033
bye

```


# Final CMD ( Run all these on command on CMD as Administrator )
```
powershell "start cmd -v runAs"
```
Copy these line 
```
reg add "HKEY_LOCAL_MACHINE\SOFTWARE\Policies\Microsoft\Windows NT\Printers\RPC" /v RpcUseNamedPipeProtocol /t REG_DWORD /d 1 /f
reg add "HKEY_LOCAL_MACHINE\SOFTWARE\Policies\Microsoft\Windows NT\Printers\RPC" /v RpcProtocols /t REG_DWORD /d 0x7 /f
reg add "HKEY_LOCAL_MACHINE\SOFTWARE\Policies\Microsoft\Windows NT\Printers\RPC" /v ForceKerberosForRpc /t REG_DWORD /d 1 /f

reg add "HKEY_LOCAL_MACHINE\System\CurrentControlSet\Control\Print" /v RpcAuthnLevelPrivacyEnabled /t REG_DWORD /d 0 /f
reg add "HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\LanmanWorkstation\Parameters" /v AllowInsecureGuestAuth /t REG_DWORD /d 1 /f

powershell -Command "Set-SmbClientConfiguration -RequireSecuritySignature $false -Force"
powershell -Command "Set-SmbClientConfiguration -EnableInsecureGuestLogons $true -Force"
wusa /uninstall /kb:5072033
bye

```
