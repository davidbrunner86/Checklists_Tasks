#Wo das Kennwort die nächsten 7 Tage abläuft
$AdUserExpiresPwd = Get-ADUser -filter * -properties PasswordNeverExpires,msDS-UserPasswordExpiryTimeComputed,mail | where {$_.enabled -eq $true -and $_.PasswordNeverExpires -eq  $False} | select Name,@{Name="ExpiryDate";Expression={([datetime]::FromFileTime($_."msDS-UserPasswordExpiryTimeComputed")).DateTime}},givenname,mail | where {($_.ExpiryDate | get-date)  -gt (get-date) -and ($_.ExpiryDate | get-date) -lt (get-date).adddays(7) }

#Log Eintrag
"Start Script am  $(Get-Date -F 'dd.MM.yyyy hh:mm')" | out-file "C:\scripts\PwdExpires_Notify.log" -Append


$smtpServer = "mail.gia.co.at"
$smtpFrom = "edv@gia.co.at"

$messageSubject = "Das Windows Passwort läuft bald ab"

foreach ($user in $AdUserExpiresPwd) {
 
$smtpTo = $User.mail
$message = New-Object System.Net.Mail.MailMessage $smtpfrom, $smtpto
$message.Subject = $messageSubject
$message.IsBodyHTML = $true
$Name = $User.givenname
$Ablauf = $User.ExpiryDate
$message.Body = "Hallo $Name, <br><br>
dein Windows Passwort (für Citrix, Outlook, Mail am Handy,...) läuft am $Ablauf ab. <br>
Bitte ändere dein Kennwort bis dahin ab.<br>
Mit STRG+Alt+Entf kannst du das am PC tun. In Citrix geht es über die Leiste oben.
<br><br>

Die IT-Abteilung
"

$smtp = New-Object Net.Mail.SmtpClient($smtpServer)
$smtp.Send($message)

"Mail an " + $smtpTo + " geschickt" | out-file "C:\scripts\PwdExpires_Notify.log" -Append

}

### Zusatz!

<#
Nachdem die Leute ohnehin warten, bis es abläuft und nix tun...

Dieses Script setzt das ablaufende Passwort auf Mitternacht zurück an dem Tag, an dem es abläuft. Der User verliert also höchstens 23 Stunden und 59 Minuten und 59 Sekunden...
Aber beim anmelden am Tag des Ablaufs muss er es einfach beim anmelden ändern

nix mehr mit "plötzlich geht nix mehr, weil es zu Mittag abgelaufen ist
#>

# Get the users, whose passwords will expire in the next 15 days
$ADUserPWDExpires = Get-ADUser -filter * -properties PasswordNeverExpires,msDS-UserPasswordExpiryTimeComputed | where {$_.enabled -eq $true -and $_.PasswordNeverExpires -eq  $False} | select SID,Name,@{Name="ExpiryDate";Expression={([datetime]::FromFileTime($_."msDS-UserPasswordExpiryTimeComputed")).DateTime}} | where {($_.ExpiryDate | get-date)  -gt (get-date) -and ($_.ExpiryDate | get-date) -lt (get-date).adddays(7) }


# loop through them
foreach ($user in $ADUserPWDExpires){

#Hilfsvariable - Datum des Kennwortablaufs -1
$tempdate = (get-date $user.ExpiryDate).Date.AddDays(-1)

" Checke Benutzer " + $user.Name + " wegen Passwortablauf " + $user.ExpiryDate | Out-File "C:\scripts\PwdExpires_Notify.log" -Append

# helper variable with the expiration date of tomorrow (since script runs at 22:00 the day before)

# check if the user expiration date minus 1, reset to midnight is equal to current date, also reset to midnight
if (($tempdate).Date -eq (get-date).Date){

    # here is where the magic happens... it is today, so the script will reset to midnight
    Set-ADUser -Identity $user.SID -ChangePasswordAtLogon $true
    "Benutzer" + $user.Name + "Passwort auf ChangePasswordAtLogon gesetzt" | Out-File "C:\scripts\PwdExpires_Notify.log" -Append

}


}
