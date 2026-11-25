# BITS and Procmon evidence excerpts

Normalized excerpts with placeholders. Dates and times use Windows UTC+08:00.

| Source | Record or sequence | Observation |
| --- | --- | --- |
| BITS Operational | 306 / 54857 | 2026-11-25 12:54:37, saved jobs loaded |
| BITS Operational | 306 / 54937 | 2026-11-25 13:44:57, saved jobs loaded |
| Procmon | 499374 | 13:42:12.964, svchost.exe PID 2636, process index 31, C:\Windows\System32\svchost.exe -k netsvcs -p |
| Procmon | 14936050 | 13:45:01.582, successful Process Create, parent PID 2636, child PID 10068, thread 12568. Image %LOCALAPPDATA%\Temp\uTorrent_Web_files\ReasonLabs-Suite-Setup-Wizard.exe. Quoted image with /sy observed. Five user-mode frames map to qmgr.dll. |
| Procmon | 14951102 | 13:45:02.002, operation on extracted rsSyncSvc.exe from PID 10068's trace process index. |
| Procmon | 14960637 | 13:45:02.240, successful Process Create, PID 10068 to PID 9256, UnifiedStub-installer.exe with /sa. |
| Procmon | 14969582 | 13:45:02.862, child access referring to C:\Program Files\ReasonLabs\Common\Stub\v7.3.3\Stub.exe. |

Elevated BITS inventory independently showed DotSetup Installer, GUID 094a24cb-36d1-4a62-93df-c3a790a48a92, owner `<DEVICE>\<USER>`, Transferred, and notification flags JobTransferred and JobError. The notification path matches sequence 14936050. Its stored argument ended in /s. The trace does not show a direct GUID-to-PID join. The [technical analysis](../../TECHNICAL_ANALYSIS.md) lists the remaining fields and limits.
