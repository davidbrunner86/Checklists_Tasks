**
How to simulate Persistent Memory (PMem) in vSphere 6.7 for educational purposes?  
https://williamlam.com/2018/05/how-to-simulate-persistent-memory-pmem-in-vsphere-6-7-for-educational-purposes.html

 

Konfig:  

-esxcli:  
     esxcli system settings kernel set -s fakePmemPct -v 25  
  (25 %)  
-reboot

 

 

Check:  
    esxcli system settings kernel list -o fakePmemPct****
