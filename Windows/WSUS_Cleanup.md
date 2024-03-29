WSUS manuell säubern:

invoke-wsusservercleanup -DeclineExpiredUpdates  
invoke-wsusservercleanup -DeclineSupersededUpdates  
invoke-wsusservercleanup -CleanupUnneededContentFiles  
invoke-wsusservercleanup -DeclineExpiredUpdates  

Sehr hilfreich, wenn obsolete Updates fehlschlagen:  
https://kickthatcomputer.wordpress.com/2017/08/15/wsus-delete-obsolete-updates/  
