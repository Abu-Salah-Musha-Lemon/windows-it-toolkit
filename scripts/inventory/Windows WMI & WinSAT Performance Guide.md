# Windows WMI & WinSAT Performance Guide

A simple guide to using **PowerShell, WMI/CIM, and `Win32_WinSAT`** to retrieve Windows performance scores and classify general system performance.

---

## 📋 Table of Contents

* [Overview](#overview)
* [What is WMI?](#what-is-wmi)
* [Get-WmiObject](#get-wmiobject)
* [Get-CimInstance](#get-ciminstance)
* [Win32_WinSAT](#win32_winsat)
* [WinSAT Properties](#winsat-properties)
* [Get WinSAT Information](#get-winsat-information)
* [Display Important Scores](#display-important-scores)
* [Performance Classification](#performance-classification)
* [Performance Table](#performance-table)
* [Important Limitations](#important-limitations)
* [Quick Reference](#quick-reference)
* [Recommended Command](#recommended-command)
* [Disclaimer](#disclaimer)

---

## Overview

Windows provides several ways to retrieve hardware and system information through **Windows Management Instrumentation (WMI)** and **Common Information Model (CIM)**.

The `Win32_WinSAT` class can be used to retrieve Windows System Assessment Tool (WinSAT) performance scores.

These scores can provide a quick overview of system performance.

---

## What is WMI?

**WMI (Windows Management Instrumentation)** is a Windows management framework that provides access to information about the operating system, hardware, software, and system configuration.

PowerShell can use WMI/CIM to query this information.

---

## Get-WmiObject

`Get-WmiObject` is a legacy PowerShell command used to query WMI classes.

### Example

```powershell
Get-WmiObject -Class Win32_WinSAT
```

This command queries the `Win32_WinSAT` WMI class.

> **Note:** `Get-WmiObject` is a legacy command. Modern PowerShell scripts should generally use `Get-CimInstance` instead.

---

## Get-CimInstance

`Get-CimInstance` is the modern PowerShell command used to retrieve CIM/WMI information.

### Example

```powershell
Get-CimInstance -ClassName Win32_WinSAT
```

Short version:

```powershell
Get-CimInstance Win32_WinSAT
```

---

## Win32_WinSAT

`Win32_WinSAT` is a WMI/CIM class associated with the **Windows System Assessment Tool (WinSAT)**.

It can provide performance scores for different parts of a computer, including:

* CPU
* Memory
* Disk
* Graphics
* 3D Graphics
* Overall/Base Performance

---

## WinSAT Properties

| Property        | Description                        |
| --------------- | ---------------------------------- |
| `CPUScore`      | CPU performance score              |
| `MemoryScore`   | RAM/Memory performance score       |
| `DiskScore`     | Primary disk performance score     |
| `GraphicsScore` | Desktop graphics performance score |
| `D3DScore`      | 3D/Gaming graphics score           |
| `WinSPRLevel`   | Overall/Base performance score     |

---

## Get WinSAT Information

To display the available `Win32_WinSAT` information:

```powershell
Get-CimInstance Win32_WinSAT
```

### Example Output

```text
CPUScore       : 8.7
MemoryScore    : 8.5
DiskScore      : 8.2
GraphicsScore  : 8.9
D3DScore       : 9.1
WinSPRLevel    : 8.2
```

> The values above are examples only.

---

## Display Important Scores

To display only the main performance scores:

```powershell
Get-CimInstance Win32_WinSAT |
    Select-Object CPUScore, MemoryScore, DiskScore, GraphicsScore, D3DScore, WinSPRLevel
```

### Example Output

```text
CPUScore      : 8.7
MemoryScore   : 8.5
DiskScore     : 8.2
GraphicsScore : 8.9
D3DScore      : 9.1
WinSPRLevel   : 8.2
```

---

# Performance Classification

The following classification uses `WinSPRLevel` as a **general performance guideline**.

## 🔥 8.0–9.9 — High-End Gaming & 4K Editing

### Suitable For

* High-end gaming
* 4K video editing
* Heavy multitasking
* Advanced creative workloads
* Demanding applications

```text
8.0–9.9
HIGH-END GAMING & 4K EDITING
```

---

## 🟢 6.5–7.9 — Mid-Level Gaming & 1080p Editing

### Suitable For

* Mid-level gaming
* 1080p video editing
* Multitasking
* General productivity
* Smooth everyday performance

```text
6.5–7.9
MID-LEVEL GAMING & 1080p EDITING
```

---

## 🟡 5.0–6.4 — Daily Use & Basic Editing

### Suitable For

* Daily computer use
* Basic video editing
* Basic image editing
* Office work
* Study
* Web browsing
* Media consumption

```text
5.0–6.4
DAILY USE & BASIC EDITING
```

---

## 🔵 Below 5.0 — Normal Browsing & YouTube

### Suitable For

* Web browsing
* YouTube
* Email
* Basic office work
* Lightweight applications
* Basic everyday tasks

```text
<5.0
NORMAL BROWSING & YOUTUBE
```

---

## Performance Table

| WinSPR Level | Performance Level | Recommended Usage                                   |
| :----------: | ----------------- | --------------------------------------------------- |
|  **8.0–9.9** | 🔥 High-End       | High-end gaming, 4K editing, heavy multitasking     |
|  **6.5–7.9** | 🟢 Mid-Level      | Mid-level gaming, 1080p editing, smooth performance |
|  **5.0–6.4** | 🟡 Basic/Mid      | Daily use, basic editing, office & study            |
|   **<5.0**   | 🔵 Entry-Level    | Normal browsing, YouTube, light tasks               |

---

# Important Limitations

`WinSPRLevel` should **not** be treated as a complete gaming or video-editing benchmark.

Actual system performance depends on many factors, including:

* CPU model
* GPU model
* GPU VRAM
* RAM capacity
* RAM speed
* SSD/HDD performance
* Cooling
* Power limits
* Graphics drivers
* Game optimization
* Video-editing software
* Resolution
* Graphics settings

For example, a system with a high WinSAT score may still perform poorly in demanding games if the GPU is not powerful enough.

Likewise, a powerful GPU may not provide good editing performance if the system has insufficient RAM or CPU performance.

> **WinSPRLevel is best used as a general system performance indicator, not as a professional benchmark.**

---

# Quick Reference

```text
WMI
│
├── Get-WmiObject
│      └── Legacy PowerShell WMI command
│
└── Get-CimInstance
       └── Modern PowerShell CIM command
               │
               └── Win32_WinSAT
                       │
                       ├── CPUScore
                       ├── MemoryScore
                       ├── DiskScore
                       ├── GraphicsScore
                       ├── D3DScore
                       └── WinSPRLevel
```

---

# Score Classification

```text
┌─────────────┬─────────────────────────────────────────┐
│ Score       │ Classification                          │
├─────────────┼─────────────────────────────────────────┤
│ 8.0 – 9.9   │ High-End Gaming & 4K Editing            │
│ 6.5 – 7.9   │ Mid-Level Gaming & 1080p Editing        │
│ 5.0 – 6.4   │ Daily Use & Basic Editing               │
│ < 5.0       │ Normal Browsing & YouTube               │
└─────────────┴─────────────────────────────────────────┘
```

---

# Recommended Command

For a quick WinSAT performance check:

```powershell
Get-CimInstance Win32_WinSAT |
    Select-Object CPUScore, MemoryScore, DiskScore, GraphicsScore, D3DScore, WinSPRLevel
```

---

# Requirements

* Windows
* PowerShell
* `Win32_WinSAT` availability
* WinSAT assessment data

---

# Disclaimer

This project is intended for **educational and informational purposes**.

The performance categories in this guide are approximate guidelines and are **not official Microsoft performance classifications**.

For accurate gaming or video-editing performance, use dedicated benchmarks and consider the complete hardware configuration.

---

## License

You can add your preferred open-source license here, such as **MIT**, **Apache-2.0**, or **GPL-3.0**.

---

## Author

**[Abu Salah Musha Lemon](https://github.com/Abu-Salah-Musha-Lemon)**

If you find this guide useful, consider giving the repository a ⭐ on GitHub.
