# Incident timeline

Dates and times below are Windows local time, **UTC+08:00**. The evidence keys point to the [evidence index](EVIDENCE_INDEX.md).

| Date and time | Observation | Evidence |
| --- | --- | --- |
| 2026-11-02 02:02:41–02:02:45 | Job created at 02:02:41. Transfer and cache creation at 02:02:44. Job modified at 02:02:45. | E1, E5 |
| 2026-11-19 to 2026-11-25 | Repeated Safer Web DNS Resolver registrations appear in the System query. Its 179 matches span multiple event IDs and do not measure reinstalls. | E3 |
| 2026-11-25 09:53:39 | rsAssistant registered as automatic LocalSystem. | E3, 7045 / 514753 |
| 12:54:07–12:54:14 | Security 4688 captured early boot processes. No matching installer parent. | E4 |
| 12:54:37 | BITS loaded its saved job list. | E2, 306 / 54857 |
| 12:54:42 | ReasonLabs directory and Program Files stub created. | E5 |
| 12:55:17–12:55:56 | Multiple ReasonLabs services registered. Vendor logs also show installation transactions for engine, WSC, EDR. | E3 |
| 13:05:39 | DNS Resolver re-registered during later component-related activity. | E3, 7045 / 515274 |
| 13:22 | Elevated report captured event-log and service baseline. Sysmon channel unavailable. Task Scheduler Operational logging disabled. | E3, E4 |
| 13:27:07–13:32:09 | Earlier Procmon trace showed Windows starting existing services and the engine starting UI helpers. | E6 |
| 13:42:12.964 | Procmon recorded service-host creation with PID 2636 and process index 31. | E7, sequence 499374 |
| 13:44:57 | BITS loaded saved jobs. | E2, 306 / 54937 |
| 13:45:01.582 | Service host launched cached wizard, PID 10068. Five call-stack addresses resolved to qmgr.dll. | E7, sequence 14936050 |
| 13:45:02.240 | Wizard started UnifiedStub-installer.exe, PID 9256. | E7, sequence 14960637 |
| 14:03–14:05 | Elevated queries collected BITS event and job reports. | E1, E2 |
| After 14:05, before 14:11 | Owner canceled the exact confirmed BITS job. | E8 |
| Around 14:11–14:29 | Quarantined matching installer copies. After silent uninstall exit, restored only the verified Program Files helper and completed vendor uninstall. | E5, E8 |
| 14:28:25 and 14:29:10 | DNS resolver exit and temporary Snow terminate helper during cleanup. | E3, 7034 / 515948, 7045 / 515950 |
| Around 14:35–14:39 | Removed rsSyncSvc, moved data remnants and confirmed Defender active. One disabled Snow entry remained. | E8, E9 |
| Estimated 14:46:33 boot, checked about 15:07 | Product directories, cached path, targeted active service keys and uninstall entry still absent. Elevated repeat checks remained pending. | E9 |

The scope ends after a targeted check of one later normal restart. No longitudinal recurrence monitoring was performed.
