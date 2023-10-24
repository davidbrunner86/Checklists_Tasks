# Diverse AD Scripts  

# Aktivierte AD User mit letztem LoginDatum, das länger als 3 Monate zurück liegt  
Import-Module ActiveDirectory  
get-aduser -filter 'Enabled -eq $true' -Properties LastLogonDate | ?{$_.LastLogonDate -lt (get-date).AddDays(-90)} | select Name,SamAccountName,UserPrincipalName,LastLogonDate | Sort-Object LastLogonDate | ft -AutoSize  

#Dasselbe mit Computerobjekten  
get-adcomputer -filter 'Enabled -eq $true' -Properties * | ?{$_.LastLogonDate -lt (get-date).AddDays(-90)} | select Name,SamAccountName,LastLogonDate,managedby #| Out-GridView  

#Aktive Benutzer mit Info, wann das Passwort abläuft  
Get-ADUser -filter {Enabled -eq $True -and PasswordNeverExpires -eq $False} –Properties "DisplayName", "msDS-UserPasswordExpiryTimeComputed" |  
Select-Object -Property "Displayname","userprincipalname","samaccountname",@{Name="ExpiryDate";Expression={[datetime]::FromFileTime($_."msDS-UserPasswordExpiryTimeComputed")}} | Sort-Object ExpiryDate  
