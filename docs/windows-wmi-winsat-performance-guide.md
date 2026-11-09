# Windows WMI, CIM, and WinSAT Performance Guide

This guide introduces PowerShell WMI/CIM queries and the `Win32_WinSAT`
class. It shows how to retrieve available Windows System Assessment Tool
(WinSAT) scores and explains what those scores can and cannot tell you.

## 1. Overview

Windows Management Instrumentation (WMI) and the Common Information
Model (CIM) provide ways to query information about Windows, hardware,
software, and system configuration.

PowerShell can retrieve this information through commands such as
`Get-CimInstance`.

The `Win32_WinSAT` class may expose results from the Windows System
Assessment Tool. Availability and freshness of the data depend on the
Windows version and whether an assessment has been run.

## 2. WMI and CIM

**WMI** is a Windows management infrastructure that exposes management
information through classes and providers.

**CIM** is a standard information model used by management systems.
Windows PowerShell provides CIM cmdlets to query WMI-backed information.

### Legacy command: Get-WmiObject

``` powershell
Get-WmiObject -Class Win32_OperatingSystem
```

`Get-WmiObject` is a legacy Windows PowerShell cmdlet. It is not
available in PowerShell 7 by default.

### Recommended command: Get-CimInstance

``` powershell
Get-CimInstance -ClassName Win32_OperatingSystem
```

For a short query:

``` powershell
Get-CimInstance Win32_OperatingSystem
```

For modern PowerShell scripts, prefer CIM cmdlets unless a specific
compatibility requirement calls for a legacy WMI command.

## 3. Query WinSAT information

Try:

``` powershell
Get-CimInstance -ClassName Win32_WinSAT
```

You can also select only the commonly referenced score properties:

``` powershell
Get-CimInstance -ClassName Win32_WinSAT |
    Select-Object CPUScore, MemoryScore, DiskScore, GraphicsScore, D3DScore, WinSPRLevel
```

If the query returns no instance or an error, WinSAT data may not be
available on that system. Do not interpret missing values as a hardware
failure.

## 4. Common WinSAT properties

  Property          General description
  ----------------- ----------------------------------------
  `CPUScore`        Processor assessment score.
  `MemoryScore`     Memory assessment score.
  `DiskScore`       Primary disk assessment score.
  `GraphicsScore`   Desktop graphics assessment score.
  `D3DScore`        3D graphics assessment score.
  `WinSPRLevel`     Overall base score reported by WinSAT.

The exact values and availability depend on the Windows installation and
assessment data.

## 5. Example output

The following values are illustrative only:

``` text
CPUScore      : 8.7
MemoryScore   : 8.5
DiskScore     : 8.2
GraphicsScore : 8.9
D3DScore      : 9.1
WinSPRLevel   : 8.2
```

Do not expect every computer to return these properties or values.

## 6. Refreshing assessment data

On Windows versions where the WinSAT command is available, an
administrator can attempt to run a formal assessment from an elevated
Command Prompt:

``` bat
winsat formal
```

This can take time and may place load on the system. The command may be
unavailable or behave differently on some Windows editions and builds.
Check the command's availability before relying on it.

After the assessment completes, query `Win32_WinSAT` again. If the class
is unavailable or returns no usable data, use a current
hardware-information or benchmarking tool instead.

## 7. Important limitations

WinSAT scores are historical Windows system-assessment indicators. They
are **not** a modern, complete gaming, workstation, or video-editing
benchmark.

Do not use a single `WinSPRLevel` value to claim that a computer is
suitable for 4K editing, high-end gaming, or a particular professional
workload.

Real-world performance depends on many factors, including:

-   Exact CPU model and sustained power limits
-   GPU model, architecture, and video memory
-   RAM capacity, speed, and memory-channel configuration
-   SSD or HDD model and performance
-   Cooling and thermal throttling
-   Graphics drivers
-   Application and game optimization
-   Workload, resolution, and settings

The score ranges sometimes circulated online (for example, labels such
as "high-end gaming" or "basic editing") are informal interpretations,
not official Microsoft performance categories. This guide does not treat
them as authoritative classifications.

## 8. Quick reference

``` text
PowerShell
   |
   +-- Get-WmiObject
   |      Legacy Windows PowerShell WMI cmdlet
   |
   +-- Get-CimInstance
          Modern CIM query cmdlet
              |
              +-- Win32_WinSAT
                     |
                     +-- CPUScore
                     +-- MemoryScore
                     +-- DiskScore
                     +-- GraphicsScore
                     +-- D3DScore
                     +-- WinSPRLevel
```

## 9. Recommended command

For a quick query of the commonly referenced values:

``` powershell
Get-CimInstance -ClassName Win32_WinSAT |
    Select-Object CPUScore, MemoryScore, DiskScore, GraphicsScore, D3DScore, WinSPRLevel
```

## 10. Disclaimer

This guide is for educational and informational use. WinSAT data may be
missing, stale, or unavailable. Use current, workload-specific
benchmarks when making hardware purchasing or performance decisions.
