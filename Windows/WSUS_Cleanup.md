WSUS manuell säubern:

invoke-wsusservercleanup -DeclineExpiredUpdates  
invoke-wsusservercleanup -DeclineSupersededUpdates  
invoke-wsusservercleanup -CleanupUnneededContentFiles  
invoke-wsusservercleanup -DeclineExpiredUpdates  

Sehr hilfreich, wenn obsolete Updates fehlschlagen:  
https://kickthatcomputer.wordpress.com/2017/08/15/wsus-delete-obsolete-updates/  




# Webfund:

have been researching this for days and probably have about 10 hours invested in trying to come to a conclusion and now want some feedback from this group. Below you will find everything I think you might need about my current configuration. I originally installed my first WSUS server in 2006.Over the years I added new products as needed, changed to express files (seems to have been a bad decision), unselected products no longer needed, and when version 3 came out began running the cleanup wizard monthly. I have never declined any updates myself and only approved those showing as needed. I watched my content grow to about 65 GB then decided to migrate to a new server. I followed this procedure which worked very well:

http://exchangeserverpro.com/how-to-move-wsus-30-to-a-new-server

After the migration I tested a round of updates and everything was working fine. Then I added Windows Server 2008 (only have one server with this OS) and SQL Server 2008 R2 (only have one server with this OS) products and was amazed when my content went up to over 100 GB. I decided to remove those two products since only one server needs them and I could update it direct from MS instead thinking it would be easy to recover the space. Was I in for a surprise. No real easy way to do that. From my research these seem to be the options to do a good clean up on all unneeded content. What do you suggest as the best way for me to proceed?
 
Option 1: uninstall and re-install fresh

Option 2: reset the content following this procedure:

http://blogs.technet.com/b/gborger/archive/2009/02/27/what-to-do-when-your-wsuscontent-folder-grows-too-large.aspx

Option 3: reset the content following this procedure: (I tested this on my old server and steps 1-4 took about 90 minutes)


1. Decline all previously approved updates. (Yes, all of them. Even the ones you want.)

2. Run the cleanup wizard -- this will remove files in the WsusContent folder.

3. Change the approval of all DECLINED updates from declined to "Not Approved" (and apply inheritance).

               27xxx ?

4. Run the cleanup wizard again -- this will decline all expired updates.

5. Re-approve needed updates. Content will be re-downloaded.

I do plan to remove the express files option first to reduce the amount of space needed for storage. If you see anything else in my config that should be tweaked please let me know. Thanks for any and all feedback. I appreciate you taking the time to read this and respond.

My configuration follows:
 
1 Gbps switched network infrastructure with 10/10 fiber internet access
 
WSUS – current environment – 1 physical server also used as a file share server for user files
 
Windows Server 2008 Enterprise (x86)
 
Dual Intel Xeon 2.8 Ghz
 
2 GB Ram
 
1 Gbps Nic
 
136 GB hard drive for DB and WSUS Content files (currently DB=1.5 GB, Content = 108 GB)
 
WSUS Server version: 3.2.7600.226
 
Using the Windows Internal Database (I do have a SQL Server 2008 R2 server available now)
 
128 Computers in 13 groups manually assigned but computers are directed to this server via GP
 
(all Servers and 90% of workstations are current on updates through last month)
 
3189 Updates installed/NA
 
139 Updates needed
 
919 Updates with no status
 
Configuration options:
 
Update source – Microsoft
 
Products – Office 2003, Office 2010, Windows 7, Windows Defender, Windows Server 2003, Windows Server 2008 R2, Windows XP (previously selected but no longer needed: Office 2002/XP, SQL Server 2008 R2, Windows 2000, Windows Server 2008)
 
Classifications – critical, definition, security, service packs, update rollups, updates
 
Update files and languages – store locally, download only when approved, download express installation files, English
 
Automatic approvals – above classifications for test group which includes one computer with each of the following OS’s: Windows 7, Windows Server 2003, Windows Server 2008 R2, Windows XP
 
After those machines are tested then I approve and install needed updates for the server group (8 machines total) which include Windows Server 2003, Windows Server 2003 R2, Windows Server 2008 R2, Windows XP
 
After those machines are tested then I approve needed updates for all computers and let the users install them
