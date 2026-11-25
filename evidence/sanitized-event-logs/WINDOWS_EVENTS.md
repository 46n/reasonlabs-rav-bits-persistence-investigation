# Windows event evidence excerpts

| Channel and event ID | Record ID | Relevant evidence |
| --- | --- | --- |
| Security 4688 | Selected rows 6197032–6197046 | 12:54:07–12:54:14 early boot processes, ending with winlogon.exe. No matching installer parent in the queried period. Range includes gaps. |
| System 7045 | 514753 | 09:53:39, rsAssistant automatic as LocalSystem |
| System 7045 | 515168, 515169, 515170 | 12:55, WSC, client, and engine registrations |
| System 7045 | 515171, 515172 | 12:55, demand-start Snow helper, harden and enable operations |
| System 7045 | 515204, 515212, 515215, 515216 | 12:55, EDR, VPN, DNS resolver, DNS service registrations |
| System 7045 | 515274 | 13:05:39, DNS resolver re-registered |
| System 7034 | 515948 | 14:28:25, DNS resolver exit during removal |
| System 7045 | 515950 | 14:29:10, demand-start Snow helper with terminate argument during removal |

The Sysmon channel was unavailable. Event IDs and record IDs are distinct. Time zone UTC+08:00.
