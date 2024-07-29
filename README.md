# windows10-11_shared_printer_solve

reg add "HKEY_LOCAL_MACHINE\SOFTWARE\Policies\Microsoft\Windows NT\Printers\RPC" /v RpcUseNamedPipeProtocol /t REG_DWORD /d 1 /f

Windows Registry Editor Version 5.00

[HKEY_LOCAL_MACHINE\System\CurrentControlSet\Control\Print]
"RpcAuthnLevelPrivacyEnabled"=dword:00000000




Enable via Group Policy:

Path: Computer Configuration > Administrative Templates > Printers > Configure RPC listener settings
Enable and set protocols allowed to be used to RpcOverNamedPipesAndTcp.
Enable the setting via the registry:

Run reg add "HKEY_LOCAL_MACHINE\SOFTWARE\Policies\Microsoft\Windows NT\Printers\RPC" /v RpcProtocols /t REG_DWORD /d 0x7 /f
