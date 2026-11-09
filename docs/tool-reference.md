# Tool Reference

This document describes the behavior of the scripts supplied in the
original project ZIP. File paths below are the recommended paths after
applying [the structure migration](structure-migration.md).

> Read the actual script before execution. This reference is
> descriptive, not a guarantee that every command will succeed on every
> Windows system.

## Windows Update tools

### Full Windows Update Cleanup

**File:** `scripts/windows-update/full-windows-update-cleanup.bat`

**What it does:**

1.  Checks for administrator privileges using `NET SESSION`.
2.  Runs DISM `CheckHealth`, `ScanHealth`, and `RestoreHealth`.
3.  Runs `SFC /scannow`.
4.  Stops the Windows Update (`wuauserv`) and Background Intelligent
    Transfer (`BITS`) services.
5.  Deletes the `SoftwareDistribution\Download` and
    `SoftwareDistribution\DataStore` folders.
6.  Starts the two services again.
7.  Opens Disk Cleanup configuration with `cleanmgr /sageset:123`, waits
    briefly, and runs the selected cleanup profile with
    `cleanmgr /sagerun:123`.

**Permissions:** Administrator required; the script checks for elevation
but does not automatically elevate itself.

**Output:** Console output only; no dedicated log file.

**Notes:** The `sageset` command may display a configuration dialog.
Deleting the DataStore cache can affect the local Windows Update history
display, and downloaded update files may need to be downloaded again.
The script does not implement robust recovery if a service fails to
restart.

### Smart Windows Update Cleanup v2

**File:** `scripts/windows-update/smart-windows-update-cleanup-v2.bat`

**What it does:**

-   Checks for administrator privileges.
-   Runs DISM health checks, DISM image repair, component-store cleanup,
    and SFC.
-   Stops `wuauserv`, `bits`, `cryptsvc`, and `msiserver`.
-   Removes the `SoftwareDistribution\Download` and
    `SoftwareDistribution\DataStore` folders and recreates the Download
    folder.
-   Starts the selected services again.
-   Writes Disk Cleanup `StateFlags0123` values under the Windows
    `VolumeCaches` registry key and runs `cleanmgr /sagerun:123`.
-   Writes a timestamped log to a `Logs` directory beside the script.
-   Uses its SFC return-code handling to decide whether to show a
    restart prompt.

**Permissions:** Administrator required; the script checks for elevation
but does not automatically elevate itself.

**Output:** `Logs/Cleanup_<timestamp>.log`, relative to the script
directory.

**Important notes:**

-   The cleanup profile includes categories such as Recycle Bin,
    previous Windows installations, temporary files, and other cleanup
    areas. Review the configured categories and their consequences
    before use.
-   The script writes registry values to configure Disk Cleanup. This is
    a system configuration change.
-   The script logs command output, but it does not fully validate that
    every service stop, deletion, registry write, or service restart
    succeeded.
-   Timestamp construction parses the localized `%DATE%` value using
    slash-separated fields. On systems with a different date format, the
    generated log filename may be incorrect. This should be tested and
    made locale-independent before broad distribution.
-   The SFC result handling is based on the return codes interpreted in
    the script. Review the actual SFC output if the reported result is
    unexpected.

### Technician Deep Repair

**File:** `scripts/windows-update/technician-deep-repair.bat`

**What it does:**

1.  Attempts to stop Windows Update, BITS, Cryptographic Services, and
    Windows Installer services.
2.  Deletes the entire `SoftwareDistribution` folder and the `catroot2`
    folder.
3.  Re-registers a selected set of Windows Update DLL files with
    `regsvr32`.
4.  Starts the selected services.
5.  Runs SFC and DISM image repair.
6.  Runs `chkdsk C: /f`.

**Permissions:** Must be started from an elevated Command Prompt. The
supplied script does not contain a reliable administrator check or
self-elevation routine.

**Output:** Console output only; no dedicated log file.

**Risk:** High. This is an advanced reset operation. `chkdsk C: /f` may
need to schedule a check for the next restart if the volume is in use.
Do not run it casually or on a production computer without a backup and
a recovery plan.

### Smart Windows Update Cleanup v1 (archived)

**File:** `archive/smart-windows-update-cleanup-v1.bat`

This is the older version of the Smart Windows Update Cleanup utility.
It runs DISM and SFC, stops Windows Update and BITS, deletes selected
update cache folders, restarts the services, and runs Disk Cleanup.

It includes an administrator check but does not provide the logging and
additional cleanup handling found in v2. Keep it for history or
comparison; prefer reviewing v2 before choosing a maintained version.

## System repair and maintenance tools

### Enterprise IT Tool

**File:** `scripts/system-repair/enterprise-it-tool.ps1`

This menu-driven PowerShell script offers:

  -----------------------------------------------------------------------
  Menu option                         Action
  ----------------------------------- -----------------------------------
  1                                   Attempts to create a System Restore
                                      point using `Checkpoint-Computer`.

  2                                   Runs SFC and DISM image repair.

  3                                   Removes items under the current
                                      user's temporary directory.

  4                                   Stops Windows Update and removes
                                      files under
                                      `SoftwareDistribution\Download`,
                                      then starts the service.

  5                                   Updates Microsoft Defender
                                      signatures and starts a full scan.

  6                                   Opens `appwiz.cpl` and asks the
                                      user to select Microsoft Office and
                                      start Quick Repair manually.

  7                                   Attempts to email the report using
                                      placeholder SMTP settings.

  0                                   Exits the menu.
  -----------------------------------------------------------------------

**Permissions:** No complete administrator check is implemented.
Restore-point creation, system repair, service management, and some
Defender operations may require elevation.

**Output:** `Enterprise_Repair_Report.txt` on the current user's
Desktop.

**Important notes:**

-   Option 6 does not automate Office repair; the user must complete the
    steps in the Control Panel interface.
-   Option 7 contains placeholder server and email addresses. Configure
    and test an approved mail solution before enabling it. The script
    uses `Send-MailMessage`, a legacy/deprecated PowerShell cmdlet; do
    not treat the supplied email function as production-ready.
-   The script does not provide comprehensive error handling for every
    menu action.

### Windows Maintenance with Logging

**File:** `scripts/system-repair/windows-maintenance-with-logging.bat`

This file was originally named
`Office Professional Repair Tool (With Logging)`. Its actual behavior is
general Windows maintenance, not Office repair.

**What it does:**

-   Attempts to create a restore point.
-   Runs SFC, DISM `ScanHealth`, and DISM `RestoreHealth`.
-   Runs `chkdsk C: /scan`.
-   Deletes items under the current user's temporary directory and
    `C:\Windows\Temp`.
-   Stops Windows Update, deletes files under
    `SoftwareDistribution\Download`, and starts the service.
-   Flushes the DNS resolver cache.
-   Runs Disk Cleanup using `cleanmgr /verylowdisk`.

**Permissions:** The supplied script does not include an administrator
check. Some operations require elevation.

**Output:** `Office_Repair_Report.txt` on the current user's Desktop.

**Notes:** The output filename is retained from the original script and
should ideally be renamed in a future code revision. Deletion commands
can fail for files that are in use or protected.

## Diagnostics and inventory

### System Audit

**File:** `scripts/diagnostics/system-audit.bat`

Creates a text report containing selected information about:

-   Windows edition and version
-   CPU model, cores, and logical processors
-   Physical memory modules and total memory
-   GPU name and driver version
-   Motherboard model, manufacturer, and serial number
-   Disk model, media type, size, and status
-   Enabled network adapters and MAC addresses
-   BIOS/UEFI information
-   Display resolution reported by the video controller
-   Firewall profile state
-   Microsoft Defender status, when available
-   Secure Boot status, when supported
-   TPM information, when available

**Permissions:** No explicit administrator check. Some information may
be unavailable without elevation or on unsupported devices.

**Output:** `Full_System_Audit_Report.txt` on the current user's
Desktop.

**Privacy:** The report can contain serial numbers, MAC addresses, and
other identifying details. Redact before sharing.

### System Inventory

**File:** `scripts/inventory/system-inventory.ps1`

Collects selected system details including computer name, username,
Windows version/build, architecture, BIOS, motherboard, CPU, RAM, GPU,
storage, active network adapters, monitor data, TPM, Secure Boot,
Defender, and firewall status.

It attempts to export the data in three formats:

-   TXT
-   CSV
-   PDF

**Permissions:** Some queries may require elevation or may be
unavailable on particular systems.

**Dependency:** The PDF section imports the `PSWritePDF` PowerShell
module. TXT and CSV files are written before the PDF section. If the
module is not installed or its commands differ from those expected by
the script, PDF generation can fail.

**Output:** Timestamped `Client_System_Report_*` files on the current
user's Desktop.

**Known compatibility concerns:**

-   `Confirm-SecureBootUEFI` may fail on legacy BIOS systems or devices
    that do not support the command.
-   `Get-Tpm`, `Get-MpComputerStatus`, `Get-NetAdapter`, and storage
    cmdlets may be unavailable or return incomplete information
    depending on Windows features and device configuration.
-   The script does not wrap every hardware query in independent error
    handling. One failed query may interrupt object construction.

## Technical guide

### WMI and WinSAT Performance Guide

**File:** `docs/windows-wmi-winsat-performance-guide.md`

Explains PowerShell WMI/CIM queries and the `Win32_WinSAT` class. WinSAT
scores are historical system-assessment indicators, not modern gaming or
content-creation benchmarks. Read the guide's limitations before using
the values to compare computers.
