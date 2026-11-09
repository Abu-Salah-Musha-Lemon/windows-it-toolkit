# Safety Guide

Windows IT Toolkit includes scripts that can modify Windows services,
delete cache or temporary files, change cleanup settings, run system
repair commands, and schedule disk checks. These actions can affect the
operating system and user data.

**Use the scripts at your own risk.** This guide is not a guarantee of
safety and does not replace a backup or a technician's judgment.

## 1. Before running any script

-   Read the complete script, not only its filename or README
    description.
-   Confirm that the script came from the repository you intended to
    use.
-   Back up important files to a separate location.
-   Ensure that you have a way to recover the computer if Windows does
    not start normally.
-   Save open work and close applications that may be affected.
-   Connect laptops to reliable power.
-   Check free disk space before repair or cleanup operations.
-   Use a test computer or virtual machine snapshot before testing
    changes to services, registry settings, or disk state.
-   Run only the one utility that addresses the problem.

A System Restore point can be useful, but it is not a full backup. It
may be disabled, fail to create, or be unavailable when Windows cannot
boot.

## 2. Relative risk by tool

The labels below are practical warnings, not formal safety
certifications.

  -----------------------------------------------------------------------
  Tool                    Relative risk           Main reason
  ----------------------- ----------------------- -----------------------
  System Audit            Low to moderate         Primarily collects
                                                  system information, but
                                                  the report contains
                                                  identifying hardware
                                                  and network details.

  System Inventory        Low to moderate         Primarily collects
                                                  information, but
                                                  exports sensitive
                                                  device details and may
                                                  fail on unsupported
                                                  queries.

  Enterprise IT Tool      Varies by menu option   Can create restore
                                                  points, repair Windows,
                                                  delete temporary/cache
                                                  files, stop services,
                                                  and run a full Defender
                                                  scan.

  Windows Maintenance     Moderate to high        Runs repair commands
  with Logging                                    and deletes temporary
                                                  and Windows Update
                                                  download-cache files.

  Full Windows Update     Moderate to high        Stops services and
  Cleanup                                         deletes Windows Update
                                                  cache folders.

  Smart Windows Update    Moderate to high        Stops services, deletes
  Cleanup v1                                      update cache folders,
                                                  and launches Disk
                                                  Cleanup.

  Smart Windows Update    High                    Performs component
  Cleanup v2                                      cleanup, stops several
                                                  services, removes
                                                  update cache folders,
                                                  changes Disk Cleanup
                                                  registry flags, and may
                                                  prompt for restart.

  Technician Deep Repair  High                    Deletes entire Windows
                                                  Update component
                                                  folders, re-registers
                                                  system DLLs, and runs
                                                  `chkdsk /f`.
  -----------------------------------------------------------------------

## 3. Understand the main operations

### DISM and SFC

-   DISM checks and repairs the Windows component image.
-   SFC checks protected Windows system files and attempts repairs.
-   These operations can take a long time. Do not close the console or
    power off the computer while they are running.
-   A completed command does not guarantee that every Windows issue is
    fixed.
-   Review the actual command output and error codes.

### Windows Update cache cleanup

The scripts may stop services and delete one or more of these locations:

-   `C:\Windows\SoftwareDistribution\Download`
-   `C:\Windows\SoftwareDistribution\DataStore`
-   `C:\Windows\SoftwareDistribution`
-   `C:\Windows\System32\catroot2`

Windows may recreate these folders and download update files again.
Clearing the DataStore cache can affect the locally displayed update
history. Deleting the entire folders is more extensive than clearing
only downloaded update files.

Do not delete these folders manually while the related services are
running. Use a documented procedure and confirm that the services return
to their expected state.

### Disk Cleanup and registry flags

Smart Windows Update Cleanup v2 writes `StateFlags0123` values below:

`HKLM\SOFTWARE\Microsoft\Windows\CurrentVersion\Explorer\VolumeCaches`

It then runs Disk Cleanup using the selected profile. The configured
categories include items such as Recycle Bin contents, previous Windows
installations, temporary files, and other cleanup categories. These can
include data a user may want to retain.

Review the selected categories and the cleanup profile before running
this script. Do not assume that every selected category is disposable.

### CHKDSK

The Technician Deep Repair script runs:

``` bat
chkdsk C: /f
```

The `/f` option requests filesystem repairs. If the system volume is in
use, Windows may ask to schedule the check at the next restart. Make
sure important data is backed up before allowing filesystem repair.

The Windows Maintenance with Logging script uses `chkdsk C: /scan`,
which is an online scan mode. It is still a diagnostic operation and its
output should be reviewed.

### Temporary-file deletion

The Enterprise IT Tool and Windows Maintenance with Logging script
remove items from temporary directories. Some files may be in use, and
deletion may fail. Do not redirect these commands to broader directories
or change their target paths without careful review.

### Defender scan

The Enterprise IT Tool can update Microsoft Defender security
intelligence and run a full scan. A full scan may take a long time and
use system resources. Do not disable security protection to make the
scan complete faster.

## 4. Administrator privileges

Administrator privileges allow a script to make system-wide changes. Use
elevation only for a script and task that require it.

Some supplied scripts check for administrator privileges; others do not.
A script that lacks an administrator check may fail partway through if
it is started without elevation. See the [tool
reference](tool-reference.md).

Do not treat an administrator prompt as proof that a script is safe.

## 5. Protect reports and logs

Reports may contain:

-   Windows username and computer name
-   Motherboard or BIOS serial numbers
-   MAC addresses and network adapter details
-   Hardware and storage configuration
-   TPM, Secure Boot, Defender, and firewall status
-   File paths or other environment details

Before sharing a report:

1.  Open it and review its contents.
2.  Remove personal or device-identifying information.
3.  Share only the sections needed to troubleshoot the issue.
4.  Do not commit raw reports to a public repository.

The repository `.gitignore` excludes common report names and log files,
but ignored files can still be force-added. Always inspect `git status`
before committing.

## 6. Stop and seek help if

-   A command reports an unexpected error and the script continues with
    destructive steps.
-   A service does not restart.
-   The disk reports errors or unusual noises.
-   Windows asks to schedule a disk repair and you do not have a backup.
-   The system becomes unstable or loses power during repair.
-   You do not understand a registry change or deletion target.

Do not repeatedly run increasingly aggressive repair scripts without
identifying the underlying problem.

## 7. Project limitations

These scripts are provided for educational and administrative use. They
are not a substitute for Microsoft support, a verified enterprise
deployment process, or professional data-recovery procedures. The
project does not guarantee that a script will resolve a particular issue
or prevent data loss.
