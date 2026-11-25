# Technical analysis

## BITS notification and process chain

BITS persists transfer jobs independently of a product's services and startup entries. Microsoft documents SetNotifyCmdLine as a notification mechanism for transferred or error states and describes execution in the context of the user who set the command. A service-host parent therefore does not prove SYSTEM elevation for the child. The 13:45 wizard's actual access token was not obtained. An account named in a job is not proof that the person controlling it knowingly configured it. [Windows API reference](https://learn.microsoft.com/en-us/windows/win32/api/bits1_5/nf-bits1_5-ibackgroundcopyjob2-setnotifycmdline)

| Recorded job field | Value |
| --- | --- |
| Name and ID | DotSetup Installer, 094a24cb-36d1-4a62-93df-c3a790a48a92 |
| Owner | `<DEVICE>\<USER>` |
| State and flags | Transferred, JobTransferred and JobError |
| Transfer | GET, Download, Dynamic=True, Foreground, Always, SystemDefault proxy, RedirectPolicyAllowSilent, ACLFlags=None |
| Completion | One file, 2,508,288 of 2,508,288 bytes, no reported transfer error |
| Creation, transfer completion, modification | November 2 at 02:02:41, 02:02:44 and 02:02:45 UTC+08:00, respectively |
| Source | Host shield.reasonsecurity.com, path /ReasonLabs-Suite-Setup-Wizard.exe. The URL query is withheld and the sample is not linked. |
| Notification | %LOCALAPPDATA%\Temp\uTorrent_Web_files\ReasonLabs-Suite-Setup-Wizard.exe, with quoted executable path and /s in stored arguments |
| Additional recorded values | RetryInterval 5, RetryTimeout 1209600, MaxDownloadTime 7776000, as rendered by the collection |

At 13:42:12.964, Procmon sequence **499374** records C:\Windows\System32\svchost.exe -k netsvcs -p, PID 2636, trace process index 31. Later, sequence **14936050**, thread 12568, records that process successfully starting the cached wizard as PID 10068 at **13:45:01.582**. Five user-mode frames mapped to C:\Windows\System32\qmgr.dll by module address ranges. Function names were unavailable. The observed quoted command ended in **/sy**, while the later job output ended in **/s**. The difference and exact trigger state are unresolved.

Sequence **14960637** at **13:45:02.240** records the wizard starting UnifiedStub-installer.exe as PID 9256 from a temporary 7z extraction directory, with **/sa**. Sequence **14951102** records successful access involving extracted rsSyncSvc.exe. Sequence **14969582** references the installed Stub.exe. These are incident observations. There is no need to execute or recreate the launcher to validate the case.

The behavioral parallel is [MITRE ATT&CK T1197, BITS Jobs](https://attack.mitre.org/techniques/T1197/). This mapping describes how persisted jobs can provide execution across reboots. It is independent of any conclusion about intent or a formal malware verdict.

## Service registration and alternative explanations

- System **7045** records establish the creation of service entries, including LocalSystem product registrations around 12:55. They do not independently identify the installer parent. **7034** for the resolver and the demand-start Snow **7045** at 14:29 occurred during removal.
- The earlier boot capture shows services.exe starting existing services and the engine starting UI helpers. That is distinct from the later installer launch. No task or startup entry was tied to the launch. The Task Scheduler operational log was disabled historically and the inventory matched known markers only.
- rsSyncSvc included a product update URL at inspection. It was disabled and its configured image was absent then. A later file operation on a temporary copy does not establish it as the initial launcher. No specific evidence ties rsAssistant or Snow to initiating the reinstall.
- The filtered Security 4688 records cover early Windows startup, with no matching installer parent. No Sysmon operational channel was available. Unrelated OneDrive and Edge BITS transfers near incident times were observed in logs, without causal evidence.
- The vendor's WSC service and passive Defender baseline were observed. Original process tokens, packet traces, detailed content-access telemetry, and the original bundle consent screen are unavailable.

The [sanitized evidence](evidence/README.md) includes the core joins. The later PML conversion reported a processing error. The cited records were recovered from the available local data, with process identifiers and module mappings corroborated across segments. Complete trace coverage and symbolized stacks are not claimed.
