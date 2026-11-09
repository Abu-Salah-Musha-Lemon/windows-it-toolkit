# Usage Guide

This guide explains how to prepare the repository and run its Batch and
PowerShell utilities.

## 1. Before you start

-   Use a Windows computer.
-   Read the [tool reference](tool-reference.md) to choose the correct
    utility.
-   Read the [safety guide](safety.md).
-   Back up important files.
-   Save open work and close applications that may be affected.
-   Connect a laptop to reliable power before lengthy repair or disk
    operations.
-   Use an administrator account when the selected operation requires
    elevation.

Do not run all scripts as a single repair sequence. Many of them repeat
DISM, SFC, Windows Update cache cleanup, or Disk Cleanup operations.

## 2. Clone the repository

After the GitHub repository is renamed to `windows-it-toolkit`:

``` bash
git clone https://github.com/Abu-Salah-Musha-Lemon/windows-it-toolkit.git
cd windows-it-toolkit
```

If the repository still has its previous name, copy the current HTTPS
clone URL from GitHub and use that URL instead.

## 3. Run a Batch (`.bat`) script

1.  Open **Command Prompt**.
2.  Choose **Run as administrator** if the script needs elevated
    permissions.
3.  Change to the repository directory. Use `/d` if the repository is on
    another drive.
4.  Run only the selected script.

Example:

``` bat
cd /d C:\path\to\windows-it-toolkit
scripts\windows-update\smart-windows-update-cleanup-v2.bat
```

The Full Windows Update Cleanup and Smart Windows Update Cleanup v1/v2
scripts contain administrator checks. The Technician Deep Repair and
Windows Maintenance with Logging scripts do not provide a reliable
self-elevation workflow; start them from an elevated Command Prompt when
performing privileged operations.

## 4. Run a PowerShell (`.ps1`) script

1.  Open **Windows PowerShell**.
2.  Choose **Run as administrator** when the selected operation needs
    elevation.
3.  Change to the repository directory.
4.  Run the script.

Example:

``` powershell
Set-Location "C:\path\to\windows-it-toolkit"
.\scripts\system-repair\enterprise-it-tool.ps1
```

The Enterprise IT Tool does not perform a complete administrator check
before showing its menu. Some menu options may fail if PowerShell is not
elevated or if the relevant Windows feature is unavailable.

### Execution policy

If PowerShell blocks a script, follow your organization's policy or ask
the device administrator. Do not change the machine-wide execution
policy or use a broad bypass simply to run an unfamiliar script. First
confirm that the script is trusted and that you understand its contents.

## 5. System Inventory PDF requirement

The System Inventory script writes TXT and CSV reports, then imports the
`PSWritePDF` module for PDF output.

If PDF output is required:

1.  Review the module and its source before installing it.
2.  Install it only from a source you trust and according to the
    module's official instructions.
3.  Confirm that it is available to the PowerShell session.
4.  Run the inventory script again.

If the module import fails, the TXT and CSV reports may already exist.
Check the Desktop before rerunning the script.

## 6. Output locations

Actual output locations are defined in each script. In the supplied
versions:

  -----------------------------------------------------------------------
  Utility                             Output
  ----------------------------------- -----------------------------------
  Smart Windows Update Cleanup v2     Timestamped `.log` file in a `Logs`
                                      directory beside the script.

  Enterprise IT Tool                  `Enterprise_Repair_Report.txt` on
                                      the current user's Desktop.

  Windows Maintenance with Logging    `Office_Repair_Report.txt` on the
                                      current user's Desktop. The
                                      filename is inherited from the
                                      original script and does not mean
                                      that the script performs Office
                                      repair.

  System Audit                        `Full_System_Audit_Report.txt` on
                                      the current user's Desktop.

  System Inventory                    Timestamped
                                      `Client_System_Report_*.txt`,
                                      `.csv`, and `.pdf` files on the
                                      current user's Desktop. PDF depends
                                      on `PSWritePDF`.
  -----------------------------------------------------------------------

The Full Windows Update Cleanup, Smart Windows Update Cleanup v1, and
Technician Deep Repair scripts do not create a dedicated report file in
the supplied versions.

## 7. After running a script

-   Wait for DISM, SFC, CHKDSK, Defender, and cleanup operations to
    finish.
-   Read the complete console output and any generated log.
-   Check whether the script reported errors or requested a restart.
-   Restart only when appropriate and after saving your work.
-   Confirm that the original Windows issue is resolved.
-   If the problem remains, collect the exact error code and relevant
    log section before trying another repair.

## 8. Troubleshooting common execution problems

### Access is denied

The operation may require administrator privileges. Close the current
window and reopen Command Prompt or PowerShell using **Run as
administrator**. Do not assume that every script automatically requests
elevation.

### A command or PowerShell cmdlet is not recognized

The command may be unavailable on the Windows edition, build, or
PowerShell environment. Check the tool reference and the command's
availability before modifying the script.

### The restore point cannot be created

System Protection may be disabled, a restore point may have been created
too recently, or the command may not have sufficient privileges. A
restore point is not a substitute for a separate backup.

### PDF report generation fails

The System Inventory script requires the `PSWritePDF` module. TXT and
CSV files may still have been created before the PDF step failed.

### Secure Boot or TPM query fails

Some systems use legacy BIOS, have Secure Boot disabled, or do not
expose the relevant cmdlets. Treat the missing value as unavailable
rather than assuming the hardware is faulty.

### Windows Update services cannot be stopped or started

A service may be busy, disabled by policy, or controlled by another
maintenance operation. Do not repeatedly force service changes. Review
the error and the service state before proceeding.

## 9. Report handling

Diagnostic reports can contain personal and device-identifying
information. Redact usernames, serial numbers, MAC addresses, computer
names, and other sensitive values before sharing them. Do not commit raw
reports to GitHub.
