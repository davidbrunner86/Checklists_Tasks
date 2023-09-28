To manually remove VM Tools:

Get the VMware Cleanup utility from the link, see Cleaning Up After Incomplete Uninstallation on a Windows Host (1308) .
Run the utility and reboot the Guest machine.
Log on as Administrator and go to regedit.
Browse to HKEY_LOCAL_MACHINE\Software\Microsoft\Windows\CurrentVersion\uninstall. Search for the branch with a key named DisplayName and has a value of VMware Tools . Delete the branch associated with that entry. Do not delete the entire uninstall branch.
Browse to HKEY_LOCAL_MACHINE\Software\Classes\Installer\Products . Search for the branch with the key named ProductName and has a value of VMware Tools . Delete the branch associated with that entry.
Browse to HKEY_CLASSES_ROOT\Installer\Products . Search for the branch with the key named ProductName and has a value of VMware Tools . Delete the branch associated with that entry.
Browse to HKEY_LOCAL_MACHINE\Software\VMware. Delete the branch called VMware Tools .
Delete the folder “VMware Tools” located under %ProgramFiles%\VMware\
Try re installing VMware Tools if that didnt help folllow the step 3.
Vmware KB Link: http://kb.vmware.com/selfservice/microsites/search.do?language=en_US&cmd=displayKC&externalId=1001354

3. Simply Command Cleanup- setup /c

Mount the VM Tools ISO on the Guest machine.
Open VM Console and goto command prompt.
Go to VM Tools mounted Drive letter and Type setup /c and press Enter to force removal of all registry entries and delete the old version of VMware Tools.
Hopefully VMware Tools installation should proceed without any issues now.
VMware KB Link: http://kb.vmware.com/selfservice/microsites/search.do?language=en_US&cmd=displayKC&externalId=1012693

https://annurkarthik.wordpress.com/2010/07/05/different-ways-to-uninstall-vmware-tools/
