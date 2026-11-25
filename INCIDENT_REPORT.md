# Incident report

## Scope and assessment

On one Huawei Windows 11 laptop, the owner reported that RAV Endpoint Protection repeatedly reappeared after removal. Their initial installation of uTorrent Web and previous attempts using the vendor uninstaller, Revo, Autoruns and Safe Mode are owner observations. The original offer screen and exact consent choices were unavailable. The tested RAV product version was **7.7.12**. Scope included the core antivirus, associated EDR, RAV VPN and Safer Web DNS components.

| Finding | Evidentiary status |
| --- | --- |
| Files and multiple automatic LocalSystem services appeared at about 12:54–12:56 on November 25 | Confirmed using file timestamps, System 7045 records and vendor installation logs |
| BITS launched the cached setup wizard at 13:45:01.582 | Confirmed by recovered Procmon process-create, parent identity and five call-stack addresses in qmgr.dll |
| DotSetup Installer was the job behind the 13:45 launch | High confidence. The stored notification uniquely matched the traced executable. There is no explicit job-GUID-to-process join, and the reported arguments differ. |
| The same job caused the 12:54 reinstall | Supported hypothesis. BITS had just loaded its persisted queue and the installed stub matches the cache. The parent was not captured. |
| uTorrent Web offered or staged this package | Strong association from cache path, timestamps, job name and package identity. Original creator process and bundling consent were not captured. |
| A complete second reinstall occurred at 13:45 | Unresolved. Setup execution and follow-on file activity were captured. Existing core file creation times did not reset. |

## Distribution, authenticity and permissions

The job records a completed 2,508,288-byte download from the vendor-associated host and the executable under the uTorrent Web cache. A matching Program Files stub carried setup version 7.3.3. Both SHA-256 hashes match, and Authenticode validated Reason Cybersecurity Inc. signatures. The hash and signature checks were also repeated on the quarantined copies. See [hash inventory](evidence/hashes/SHA256.md).

System records document automatic LocalSystem services for EPP, WSC, EDR, VPN and DNS. Snow helper commands included harden and enable during installation and terminate during uninstall. An elevated deletion attempt on one Snow orphan returned error 5. The underlying ACL or self-protection rule responsible was not established. Installed access and service permissions give capabilities, including file inspection and mediation of DNS or VPN traffic. The investigation did not collect evidence demonstrating private content capture, unauthorized access or exfiltration, and did not comprehensively audit those possibilities.

The supported characterization is **persistent unwanted bundled software** on this endpoint, with PUA evaluation appropriate for independent review. Digital signatures are evidence of publisher identity and signed-file integrity. They do not answer the user-consent question or validate the application's warnings, behavior, or any Microsoft endorsement claim. No deception, impersonation, ad injection, credential theft, or malicious intent was independently demonstrated. During installation the baseline report showed Microsoft Defender real-time protection disabled in side-by-side passive mode. The investigation did not establish deliberate security-control tampering.

## Remediation and result

1. An elevated query recorded the job, owner, state and notification. The owner canceled only the verified job. An earlier restricted cancel could not see the job.
2. Both signed installer copies were quarantined. The registered uninstaller then exited silently. A read-only wrapper inspection found references to the Program Files stub and uninstall argument.
3. The owner temporarily restored only the hash-matched Program Files stub under separate approval. The original BITS notification path stayed empty. The registered vendor uninstaller then ran and removed the active suite.
4. The owner removed the disabled, missing-image rsSyncSvc registration and moved residual data folders. Windows refused deletion of Reason.SnowService.90bb83e5. It was stopped and disabled, and its executable was missing. No permissions were overridden.
5. An elevated report at 14:39 recorded Defender Normal, antivirus enabled, and real-time protection enabled. A later boot estimated at 14:46:33 occurred after removal. Targeted post-restart checks found the main directories, cached path, active product service keys and installed-app entry absent.

The elevated Defender and event-log snapshot predates the final restart. A post-restart all-users BITS inventory, elevated Defender/log verification, comprehensive browser-policy and driver-store inventory, and day-long monitoring were not completed. This documents a successful removal and one targeted post-restart check on the affected endpoint. See [timeline](TIMELINE.md), [technical analysis](TECHNICAL_ANALYSIS.md), and [verification](VERIFICATION.md).
