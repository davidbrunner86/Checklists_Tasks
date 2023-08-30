### Einfach nur ein paar simple Scripts, die sofort zu nutzen sind und hilfreich sind  

## Installationen nacheinander mit Wartezeit dazwischen:  
\\softwaredepot\install1.exe /auto
timeout 60 /nobreak
start /wait \\softwaredepot\Install-Update.exe /auto
timeout 30 /nobreak
xcopy \\vorlage\shortcut.lnk C:\Users\Public\Desktop

