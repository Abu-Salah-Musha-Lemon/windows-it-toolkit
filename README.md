# Windows IT Toolkit

A collection of Windows administration and troubleshooting scripts for
system repair, Windows Update maintenance, diagnostics, and hardware
inventory.

> **Status:** Work in progress. The scripts are separate utilities, not
> a single installer or a fully automated repair application. Review
> each script and its effects before running it.

## Overview

Windows IT Toolkit groups practical Batch (`.bat`) and PowerShell
(`.ps1`) utilities for common Windows maintenance and
information-gathering tasks. The tools use built-in Windows components
such as DISM, System File Checker (SFC), CHKDSK, Windows Defender
cmdlets, PowerShell CIM queries, and Disk Cleanup.

The project aims to make selected troubleshooting tasks easier to run
and their results easier to review through console output, logs, and
reports.

## Features

-   **Windows Update maintenance:** Check and repair the Windows image,
    run SFC, reset selected Windows Update cache folders, and restart
    related services.
-   **System maintenance:** Run repair checks, clean temporary files,
    flush DNS, and attempt to create a restore point where supported.
-   **System diagnostics:** Collect operating-system, hardware, network,
    firmware, and selected security information.
-   **Hardware inventory:** Export selected system information to TXT
    and CSV, with optional PDF output.
-   **Logging and reports:** Save maintenance logs and system reports
    for later review.
-   **Technical references:** Explain selected WMI/CIM and WinSAT
    commands, Windows activation checks, and safe tool usage.

## Repository structure

The paths below are the **recommended target paths**. The original ZIP
still has the files in its root directory and some files do not have
extensions. Follow [the structure migration
guide](docs/structure-migration.md) to move and rename them before
relying on the links in this README.

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
│   ├── README.md
│   ├── usage.md
│   ├── tool-reference.md
│   ├── safety.md
│   ├── windows-activation.md
│   ├── windows-wmi-winsat-performance-guide.md
│   └── structure-migration.md
├── archive/
│   └── smart-windows-update-cleanup-v1.bat
├── .gitignore
├── CONTRIBUTING.md
└── README.md
```

## Available tools

  ----------------------------------------------------------------------------------------------------------------------
  Tool                                                                   Purpose                 Recommended audience
  ---------------------------------------------------------------------- ----------------------- -----------------------
  [Full Windows Update                                                   Runs DISM and SFC,      Users comfortable with
  Cleanup](scripts/windows-update/full-windows-update-cleanup.bat)       stops selected update   elevated Windows
                                                                         services, removes       maintenance
                                                                         selected update cache   
                                                                         folders, restarts       
                                                                         services, and launches  
                                                                         Disk Cleanup.           

  [Smart Windows Update Cleanup                                          Adds timestamped        Experienced users and
  v2](scripts/windows-update/smart-windows-update-cleanup-v2.bat)        logging,                technicians
                                                                         component-store         
                                                                         cleanup, selected       
                                                                         update-service resets,  
                                                                         Disk Cleanup            
                                                                         configuration, and a    
                                                                         restart prompt when its 
                                                                         SFC handling detects    
                                                                         repairs.                

  [Technician Deep                                                       Resets more Windows     Experienced technicians
  Repair](scripts/windows-update/technician-deep-repair.bat)             Update components,      only
                                                                         re-registers selected   
                                                                         update DLLs, runs SFC   
                                                                         and DISM, and runs      
                                                                         `chkdsk C: /f`.         

  [Enterprise IT Tool](scripts/system-repair/enterprise-it-tool.ps1)     Menu-driven PowerShell  IT support users who
                                                                         utility for restore     can review each menu
                                                                         points, SFC/DISM,       action
                                                                         temporary-file cleanup, 
                                                                         Windows Update          
                                                                         download-cache cleanup, 
                                                                         Defender scanning, and  
                                                                         opening the Office      
                                                                         repair workflow.        

  [Windows Maintenance with                                              Runs restore-point,     Users comfortable with
  Logging](scripts/system-repair/windows-maintenance-with-logging.bat)   SFC, DISM, CHKDSK scan, elevated maintenance
                                                                         temporary-file cleanup, 
                                                                         update download-cache   
                                                                         cleanup, DNS flush, and 
                                                                         Disk Cleanup steps;     
                                                                         writes a Desktop log.   

  [System Audit](scripts/diagnostics/system-audit.bat)                   Creates a report with   IT inventory and
                                                                         selected OS, CPU,       troubleshooting
                                                                         memory, GPU,            
                                                                         motherboard, disk,      
                                                                         network, BIOS,          
                                                                         firewall, Defender,     
                                                                         Secure Boot, and TPM    
                                                                         information.            

  [System Inventory](scripts/inventory/system-inventory.ps1)             Exports selected system IT inventory and
                                                                         and hardware            reporting
                                                                         information to TXT and  
                                                                         CSV, and attempts PDF   
                                                                         output using            
                                                                         `PSWritePDF`.           
  ----------------------------------------------------------------------------------------------------------------------

For exact behavior, required permissions, outputs, and known
limitations, read the [tool reference](docs/tool-reference.md).

## Requirements

-   A Windows computer. The scripts are intended for Windows client
    systems, but compatibility depends on the Windows edition, build,
    installed components, and permissions.
-   Administrator privileges for operations that repair the Windows
    image, manage protected services or folders, create restore points,
    change cleanup settings, or perform disk maintenance.
-   Windows PowerShell for the `.ps1` utilities.
-   Internet access may be needed by DISM to obtain repair files and by
    Microsoft Defender to update security intelligence.
-   The System Inventory script requires the `PSWritePDF` PowerShell
    module for its PDF export step. TXT and CSV output are written
    before the PDF section runs.

Some commands may be unavailable on a particular device. For example,
Secure Boot queries require compatible UEFI firmware, and Defender or
TPM cmdlets may not be available in every environment.

## Getting started

After renaming the GitHub repository to `windows-it-toolkit`, clone it
with:

``` bash
git clone https://github.com/Abu-Salah-Musha-Lemon/windows-it-toolkit.git
cd windows-it-toolkit
```

If the repository has not yet been renamed, use its current GitHub clone
URL instead.

1.  Read the [usage guide](docs/usage.md).
2.  Read the [safety guide](docs/safety.md).
3.  Choose one tool that matches the problem.
4.  Review the script before running it.
5.  Run it with the required permissions.
6.  Review the generated log or report.

Do not run every repair script one after another. Several tools perform
overlapping operations.

## Documentation

-   [Documentation index](docs/README.md)
-   [Usage guide](docs/usage.md)
-   [Tool reference](docs/tool-reference.md)
-   [Safety guide](docs/safety.md)
-   [Windows activation troubleshooting](docs/windows-activation.md)
-   [WMI and WinSAT performance
    guide](docs/windows-wmi-winsat-performance-guide.md)
-   [Repository structure migration](docs/structure-migration.md)
-   [Contributing guidelines](CONTRIBUTING.md)

## Logs and report privacy

Some tools write logs to a `Logs` folder or reports to the current
user's Desktop. Reports can contain usernames, computer names, hardware
identifiers, serial numbers, MAC addresses, and security configuration
details.

Review and redact sensitive information before sharing reports or
attaching them to a public GitHub issue. The included `.gitignore`
excludes common generated logs and report filenames, but it is not a
substitute for checking `git status` before committing.

## Limitations

-   The scripts have not been presented with a complete compatibility or
    automated test matrix.
-   A successful command exit does not guarantee that the original
    Windows problem is resolved.
-   Some operations may take a long time or require a restart.
-   The Enterprise IT Tool's Office option opens the Control Panel
    program list and asks the user to perform Quick Repair manually; it
    does not repair Office automatically.
-   The Enterprise IT Tool's email feature contains placeholder SMTP
    settings and must not be used until it is deliberately configured
    and reviewed.
-   The System Inventory PDF export depends on `PSWritePDF`.

See [known behavior and limitations](docs/tool-reference.md) and [safety
guidance](docs/safety.md) before use.

## Contributing

Contributions are welcome. Read [CONTRIBUTING.md](CONTRIBUTING.md)
before submitting changes. Include the Windows version tested, describe
the change, and remove personal data from logs and reports.

## License

No license has been selected for this repository yet. Until a license is
added, do not assume that the repository grants permission to copy,
modify, or redistribute its contents.

## Related project

For command references and technical cheatsheets covering Windows,
Linux, and other tools, visit the [Tech Command
Handbook](https://github.com/Abu-Salah-Musha-Lemon/quick-command).

## Author

**[Abu Salah Musha Lemon](https://github.com/Abu-Salah-Musha-Lemon)**
