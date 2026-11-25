# Verification and recurrence checks

Run a baseline after cleanup, record the local time and repeat the same checks after a normal restart. Record another sample after a day of typical use if recurrence was previously delayed. Save output locally. An access-denied result is an evidence gap.

## Read-only Windows checks

In Administrator Windows PowerShell, use the following to inspect defender state, known paths and startup markers. Check installed apps in Settings and installed extensions in each browser using its extensions page. Product names can vary.

~~~powershell
Get-MpComputerStatus |
    Select-Object AMRunningMode, AntivirusEnabled, RealTimeProtectionEnabled,
        AntivirusSignatureLastUpdated

$markers = 'ReasonLabs|Reason Cyber|Reason\.Snow|Safer Web|RAV VPN'
Get-CimInstance Win32_Service |
    Where-Object { "$($_.Name) $($_.DisplayName) $($_.PathName)" -match $markers } |
    Select-Object Name, State, StartMode, PathName
Get-CimInstance Win32_SystemDriver |
    Where-Object { "$($_.Name) $($_.DisplayName) $($_.PathName)" -match $markers } |
    Select-Object Name, State, StartMode, PathName

Get-ScheduledTask | Where-Object {
    "$($_.TaskName) $($_.TaskPath) $($_.Actions.Execute) $($_.Actions.Arguments)" -match $markers
} | Select-Object TaskName, TaskPath, State
Get-CimInstance Win32_StartupCommand |
    Where-Object { "$($_.Name) $($_.Command)" -match $markers } |
    Select-Object Name, Command, Location

@(
    'C:\Program Files\ReasonLabs'
    'C:\ProgramData\ReasonLabs'
    "$env:LOCALAPPDATA\Temp\uTorrent_Web_files\ReasonLabs-Suite-Setup-Wizard.exe"
) | ForEach-Object { [pscustomobject]@{Path=$_; Present=(Test-Path -LiteralPath $_)} }

Get-BitsTransfer -AllUsers -ErrorAction Stop |
    Select-Object DisplayName, JobId, JobState, NotifyCmdLine | Format-List
Get-NetAdapter | Select-Object Name, InterfaceDescription, Status
Get-DnsClientServerAddress | Select-Object InterfaceAlias, AddressFamily, ServerAddresses
~~~

If Defender is your intended primary antivirus, look for **Normal** and **True** for antivirus and real-time protection. Compare DNS and adapters against your own intended setup. A loopback DNS address or third-party VPN entry needs context. Redact network configuration and owner information before posting output.

For service registration activity, set the query start to the actual cleanup timestamp:

~~~powershell
$since = [datetime](Read-Host 'Cleanup timestamp in Windows local time, yyyy-MM-ddTHH:mm:ss')
Get-WinEvent -FilterHashtable @{LogName='System'; Id=7045; StartTime=$since} |
    Where-Object Message -match 'ReasonLabs|Reason Snow|RAV VPN|Safer Web' |
    Select-Object TimeCreated, RecordId, Message
~~~

A registered service helper during uninstall can have a legitimate cleanup purpose. Compare command, start mode, time and current file state together. An empty query or a snapshot shortly after restart cannot rule out subsequent recurrence.

## What was verified in the case

| Check | Result |
| --- | --- |
| Elevated 14:39 snapshot | Defender Normal, antivirus True, real-time True. No matching installed active product services, startup commands, scheduled tasks or vendor drivers in the targeted enumeration. |
| Later boot estimated 14:46:33 | Rechecked active service registry keys, installed-app entry, main directories and cache path. They remained absent. |
| Residual | Disabled, stopped Reason.SnowService.90bb83e5 with missing executable. A second Snow helper recorded during uninstall was no longer present. |
| Network / browser | No matching product adapter in the 14:39 inventory. DNS worked. Seven accessible Chromium manifests were spot-checked, with no known product markers. This was limited coverage. |
| Outstanding | No elevated post-restart Defender, all-users BITS queue or event log snapshot. No complete browser policy, network-filter, or driver-store audit. No day-long follow-up. |
