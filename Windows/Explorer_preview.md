## Preview in Windows Explorer für custom extensions (text-files)  

[microsoft.win32.registry]::SetValue("HKEY_CURRENT_USER\Software\Classes\.json", "PerceivedType", "text")  

Powershell:
PS C:\Windows\system32> [microsoft.win32.registry]::SetValue("HKEY_CURRENT_USER\Software\Classes\.md", "PerceivedType", "text")  
PS C:\Windows\system32> [microsoft.win32.registry]::SetValue("HKEY_CURRENT_USER\Software\Classes\.yaml", "PerceivedType", "text")  
PS C:\Windows\system32> [microsoft.win32.registry]::SetValue("HKEY_CURRENT_USER\Software\Classes\.py", "PerceivedType", "text")  
PS C:\Windows\system32> [microsoft.win32.registry]::SetValue("HKEY_CURRENT_USER\Software\Classes\.yml", "PerceivedType", "text")  
