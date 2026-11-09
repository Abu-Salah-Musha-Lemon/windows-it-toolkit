# Windows IT Toolkit

A collection of Windows administration and troubleshooting scripts for
system repair, Windows Update maintenance, diagnostics, and hardware
inventory.

> **Project status:** Work in progress. The scripts are independent
> utilities, not a single installer or fully automated repair
> application. Review each script and its effects before running it.

## Overview

Windows IT Toolkit brings common Windows maintenance and
information-gathering tasks together in one repository. It includes
Batch (`.bat`) and PowerShell (`.ps1`) scripts that use built-in Windows
utilities such as DISM, System File Checker (SFC), CHKDSK, Windows
Defender, and PowerShell CIM commands.

The goal is to make routine troubleshooting tasks easier to run and
their results easier to review through console output and log or report
files.

## Features

-   **Windows Update maintenance:** Check and repair the Windows image,
    run SFC, clear Windows Update cache files, and restart related
    services.
-   **System repair and maintenance:** Run common repair checks, clean
    temporary files, flush DNS, and create a restore point where
    supported.
-   **System diagnostics:** Collect operating-system, hardware, network,
    firmware, and selected security information.
-   **Hardware inventory:** Export a system inventory to TXT and CSV,
    with optional PDF output when the required PowerShell module is
    installed.
-   **Logging and reports:** Save selected maintenance logs and system
    reports for later review.
-   **Technical documentation:** Provide notes about Windows Management
    Instrumentation (WMI), CIM, and WinSAT.

## Repository structure

The README assumes the repository is organized using the structure
below. Move and rename the existing files to these paths before
publishing this version of the README.

``` text
windows-it-toolkit/
├── scripts/
│   ├── windows-update/
│   │   ├── full-windows-update-cleanup.bat
│   │   ├── smart-windows-update-cleanup-v2.bat
│   │   └── technician-deep-repair.bat
│   ├── system-repair/
│   │   ├── enterprise-it-tool.ps1
│   │   └── windows-maintenance-with-logging.bat
│   ├── diagnostics/
│   │   └── system-audit.bat
│   └── inventory/
│       └── system-inventory.ps1
├── docs/
│   ├── windows-wmi-winsat-performance-guide.md
│   ├── usage.md
│   └── safety.md
├── archive/
│   └── smart-windows-update-cleanup-v1.bat
├── .gitignore
├── LICENSE
└── README.md
```

## Available tools

  ------------------------------------------------------------------------------------------------------------------------------------------------------
  Tool                    File                                                                                                   Description
  ----------------------- ------------------------------------------------------------------------------------------------------ -----------------------
  Full Windows Update     [`full-windows-update-cleanup.bat`](scripts/windows-update/full-windows-update-cleanup.bat)            Checks Windows image
  Cleanup                                                                                                                        health with DISM, runs
                                                                                                                                 SFC, stops Windows
                                                                                                                                 Update services,
                                                                                                                                 removes selected update
                                                                                                                                 cache folders, restarts
                                                                                                                                 services, and launches
                                                                                                                                 Disk Cleanup.

  Smart Windows Update    [`smart-windows-update-cleanup-v2.bat`](scripts/windows-update/smart-windows-update-cleanup-v2.bat)    Runs DISM health checks
  Cleanup v2                                                                                                                     and repair,
                                                                                                                                 component-store
                                                                                                                                 cleanup, SFC, Windows
                                                                                                                                 Update service/cache
                                                                                                                                 maintenance, and Disk
                                                                                                                                 Cleanup. Writes a
                                                                                                                                 timestamped log and can
                                                                                                                                 prompt for a restart
                                                                                                                                 after detected SFC
                                                                                                                                 repairs.

  Technician Deep Repair  [`technician-deep-repair.bat`](scripts/windows-update/technician-deep-repair.bat)                      Performs a more
                                                                                                                                 extensive Windows
                                                                                                                                 Update component reset,
                                                                                                                                 re-registers selected
                                                                                                                                 update DLLs, runs SFC
                                                                                                                                 and DISM, and runs
                                                                                                                                 CHKDSK on drive C:.
                                                                                                                                 Intended for
                                                                                                                                 experienced users and
                                                                                                                                 technicians.

  Enterprise IT Tool      [`enterprise-it-tool.ps1`](scripts/system-repair/enterprise-it-tool.ps1)                               Menu-driven PowerShell
                                                                                                                                 utility for
                                                                                                                                 restore-point creation,
                                                                                                                                 SFC/DISM repair,
                                                                                                                                 temporary-file cleanup,
                                                                                                                                 Windows Update
                                                                                                                                 download-cache cleanup,
                                                                                                                                 Defender scanning, and
                                                                                                                                 opening the Control
                                                                                                                                 Panel workflow for
                                                                                                                                 Office Quick Repair.

  Windows Maintenance     [`windows-maintenance-with-logging.bat`](scripts/system-repair/windows-maintenance-with-logging.bat)   Runs a restore-point
  with Logging                                                                                                                   attempt, SFC, DISM
                                                                                                                                 checks, CHKDSK scan,
                                                                                                                                 temporary-file cleanup,
                                                                                                                                 Windows Update
                                                                                                                                 download-cache cleanup,
                                                                                                                                 DNS flush, and Disk
                                                                                                                                 Cleanup. Saves a log on
                                                                                                                                 the Desktop.

  System Audit            [`system-audit.bat`](scripts/diagnostics/system-audit.bat)                                             Collects selected OS,
                                                                                                                                 CPU, RAM, GPU,
                                                                                                                                 motherboard, storage,
                                                                                                                                 network, BIOS/UEFI,
                                                                                                                                 monitor, firewall,
                                                                                                                                 Defender, Secure Boot,
                                                                                                                                 and TPM information
                                                                                                                                 into a report on the
                                                                                                                                 Desktop.

  System Inventory        [`system-inventory.ps1`](scripts/inventory/system-inventory.ps1)                                       Collects
                                                                                                                                 operating-system and
                                                                                                                                 hardware details and
                                                                                                                                 exports TXT and CSV
                                                                                                                                 reports. PDF output
                                                                                                                                 requires the
                                                                                                                                 `PSWritePDF` PowerShell
                                                                                                                                 module.

  WMI & WinSAT            [`windows-wmi-winsat-performance-guide.md`](docs/windows-wmi-winsat-performance-guide.md)              Explains WMI/CIM
  Performance Guide                                                                                                              queries and how to
                                                                                                                                 retrieve available
                                                                                                                                 `Win32_WinSAT`
                                                                                                                                 performance scores.
  ------------------------------------------------------------------------------------------------------------------------------------------------------

The original Smart Windows Update Cleanup v1 script is kept in
`archive/` for reference. Use the v2 script as the maintained version
unless the project documentation says otherwise.

## Requirements

-   A Windows computer. The scripts are intended for Windows 10 and
    Windows 11, but compatibility can vary by Windows edition, build,
    and available features.
-   Administrator privileges for scripts that repair the Windows image,
    manage system services, clear protected folders, or perform disk
    operations.
-   Windows PowerShell for the `.ps1` scripts.
-   An internet connection may be needed for DISM image repair and
    Windows Defender signature updates.
-   The System Inventory script imports the `PSWritePDF` module to
    create its PDF report. TXT and CSV reports are generated before the
    PDF section runs.

Some commands or data sources may be unavailable on particular devices
or Windows editions. Review errors in the console and generated logs
rather than assuming every step succeeded.

## Getting started

Clone the repository:

``` bash
git clone https://github.com/Abu-Salah-Musha-Lemon/windows-it-toolkit.git
cd windows-it-toolkit
```

Choose the script that matches the task. Do not run every repair script
one after another; several tools perform overlapping operations.

### Run a Batch script

1.  Read the script and its description.
2.  Open **Command Prompt as Administrator** if the script requires
    elevated permissions.
3.  Run the selected script. For example:

``` bat
scripts\windows-update\smart-windows-update-cleanup-v2.bat
```

### Run a PowerShell script

1.  Review the script before execution.
2.  Open **Windows PowerShell as Administrator** when elevated
    permissions are required.
3.  Run the selected script. For example:

``` powershell
.\scripts\system-repair\enterprise-it-tool.ps1
```

If your organization's execution policy blocks a script, follow the
applicable administrator or organizational policy. Do not weaken
system-wide security settings just to run an unfamiliar script.

## Logs and report locations

Output locations depend on the script:

-   Smart Windows Update Cleanup v2 creates a `Logs` folder beside the
    script and writes a timestamped log there.
-   The Enterprise IT Tool writes `Enterprise_Repair_Report.txt` to the
    current user's Desktop.
-   The Windows Maintenance with Logging script writes
    `Office_Repair_Report.txt` to the Desktop. The filename is retained
    from the original script.
-   The System Audit script writes `Full_System_Audit_Report.txt` to the
    Desktop.
-   The System Inventory script writes timestamped TXT, CSV, and PDF
    reports to the Desktop. PDF creation depends on `PSWritePDF`.

System reports can contain usernames, computer names, hardware
identifiers, serial numbers, MAC addresses, and security configuration
details. Review and redact sensitive information before sharing a report
or adding it to an issue.

## Important notes and limitations

-   **Back up important data first.** Repair and cleanup operations can
    affect system services, caches, temporary files, and disk state.
-   **Create a restore point where possible.** Not every script creates
    one, and Windows may have System Protection disabled.
-   **Do not interrupt DISM or SFC.** These operations can take a long
    time. Wait for the command to finish.
-   **Windows Update cache cleanup:** Some scripts stop services and
    remove files under `SoftwareDistribution`. Windows may rebuild these
    folders and download update data again.
-   **Technician Deep Repair:** This script removes Windows Update
    component folders, re-registers DLLs, and runs `chkdsk C: /f`.
    CHKDSK may require a restart. Use it only when you understand the
    consequences and have a backup.
-   **Office repair:** The Enterprise IT Tool opens the Control Panel
    applet and asks the user to select Microsoft Office and start Quick
    Repair. It does not perform the Office repair automatically.
-   **Email reporting:** The Enterprise IT Tool contains example SMTP
    host and email addresses. The email feature will not work until
    valid mail-server settings are configured. Do not commit passwords,
    tokens, or other secrets to the repository.
-   **PDF inventory:** The System Inventory script requires `PSWritePDF`
    for its PDF-export section. If the module is unavailable, the PDF
    step can fail even though the TXT and CSV reports have already been
    written.
-   **No universal repair guarantee:** Results depend on the device,
    Windows build, permissions, disk condition, and underlying issue.

## Safety guidance

1.  Read the entire script before running it, especially scripts that
    stop services, delete files, change registry settings, or schedule
    disk repairs.
2.  Use a test machine or virtual machine before using a script on a
    production computer.
3.  Keep a current backup of important files.
4.  Connect laptops to reliable power before lengthy repair operations.
5.  Close applications and save your work before maintenance.
6.  Run only the specific tool you need.
7.  Never run scripts from a source you do not trust.
8.  Do not publish raw diagnostic reports without removing personal and
    device-identifying information.

These scripts are provided for learning and administration. Use them at
your own risk. The project does not guarantee that a script will resolve
a particular Windows problem or prevent data loss.

## Contributing

Contributions are welcome. Before opening a pull request:

-   Describe the issue or improvement clearly.
-   Explain which Windows versions and editions you tested.
-   Include relevant error messages or sanitized logs.
-   Avoid committing personal diagnostic reports, credentials, API keys,
    or other sensitive data.
-   Update the documentation when a script's behavior or requirements
    change.

For larger changes, open an issue first to discuss the proposed
approach.

## License

A license has not yet been specified for this project. Add a `LICENSE`
file before inviting reuse or redistribution. Until a license is added,
do not assume that the repository grants permission to copy, modify, or
redistribute its contents.

## Related project

For command references and technical cheatsheets covering Windows,
Linux, and other tools, see the [Tech Command
Handbook](https://github.com/Abu-Salah-Musha-Lemon/quick-command).

## Author

**Abu Salah Musha Lemon**

If you find a problem or have a suggestion, please open an issue in this
repository.
