# Evidence index

These are sanitized transcriptions, not copies of the event logs or a substitute for chain-of-custody originals. Incident-specific confidence describes the available evidence, not conclusions about every installation.

| Key | Private origin and method | Public material and weight |
| --- | --- | --- |
| E1 | Elevated Windows PowerShell Get-BitsTransfer -AllUsers, collected about 14:05 | [BITS inventory excerpt](evidence/sanitized-event-logs/BITS_AND_PROCMON.md). Direct evidence of job, owner category, state and notification. |
| E2 | Microsoft-Windows-Bits-Client/Operational via Get-WinEvent, window 12:40–13:46 | [BITS/Procmon excerpt](evidence/sanitized-event-logs/BITS_AND_PROCMON.md). Event 306 indicates queue reload, not the job GUID that triggered a program. |
| E3 | Elevated System log and installation logs | [Windows excerpt](evidence/sanitized-event-logs/WINDOWS_EVENTS.md). Strong evidence of service registration and uninstall-time activity. |
| E4 | Elevated Security 4688 query, selected pre/post boot windows | [Windows excerpt](evidence/sanitized-event-logs/WINDOWS_EVENTS.md). Shows boot timing and reporting gaps. |
| E5 | Windows file metadata, Get-FileHash, Get-AuthenticodeSignature and repeat checks of retained copies | [Hash inventory](evidence/hashes/SHA256.md). Strong file identity and signature evidence. |
| E6 | Earlier local Procmon boot capture, indexed segments | [Timeline](TIMELINE.md). Existing installed services started at boot. |
| E7 | Later local Procmon boot capture, selected raw records recovered after a conversion error | [BITS/Procmon excerpt](evidence/sanitized-event-logs/BITS_AND_PROCMON.md). Direct process chain. The stack module mapping is reconstructed, without function symbols or complete coverage. |
| E8 | Owner screenshots, restricted command results and targeted quarantine checks | [Incident report](INCIDENT_REPORT.md). Successful cancellation and uninstall, including failed steps and the orphan deletion error. Screenshots stay private. |
| E9 | Elevated 14:39 verification and restricted checks after later boot | [Verification](VERIFICATION.md). Before/after evidence windows are distinct. |

File identifiers, event record numbers and Procmon sequence numbers belong to separate namespaces. PIDs are specific to the captured boot. The job GUID is retained for correlation and does not identify a job on another machine.
