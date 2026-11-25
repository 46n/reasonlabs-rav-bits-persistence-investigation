# Privacy and security review

| Data class | Publication treatment |
| --- | --- |
| Account identity, device hostname, paths | Replace with `<USER>`, `<DEVICE>`, %LOCALAPPDATA% or generic Windows paths. Omit serials, SIDs and profile directory names. |
| Tokens, passwords, recovery keys and session IDs | No such fields are carried into the drafts. Installer URL query is omitted in full. Unrelated account and network configurations are excluded. |
| Process environments | Excluded wholesale. Only matching process IDs, image paths, timestamps and technically relevant arguments are transcribed. |
| Raw reports, traces and executables | Keep private. The public directory contains authored Markdown excerpts and hashes only. |
| Screenshots and metadata | No images were copied. This avoids visibility of other apps and filenames and image metadata exposure. |
| Deliberate technical identifiers | Retain the incident job GUID, event record and sequence numbers, timestamps in UTC+08:00, publisher, package version and hashes. Together they may allow correlation with another copy of the same case. |
| GitHub account and commit metadata | A public repository shows its owner. Confirm whether to use a personal or research account, review profile visibility and configure the intended Git commit email before publishing. |
| Microsoft sample | A separate restricted submission decision. The sample itself is excluded from every draft and archive. Review destination and retention before approving disclosure. |

The review covers the material in this draft. It does not attest that private originals are suitable for sharing. A fresh check is needed if an image, raw event, upload, repository link or other artifact is added.
