# Einfache Liste hilfreicher Cisco Befehle  

### Liste Ports, die durch Fehler deaktiviert wurden:  

sh int status err-disabled  

### Liste Port-Status, nehme die einfachen Gigabit Ports raus (und zeige nur 10GbE)  

sh int status | exclude Gi1


### Zeige Interfaces außer die, die auto-verbunden und full duplex sind. Ergebnis sind also zb. 100Mbit oder nur halb-duplex

sh int status  | exc a-full|notconnect  
