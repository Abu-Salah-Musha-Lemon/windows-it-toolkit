# Repair Windows with DISM and SFC

Before you think about reinstalling Windows, try this combo to fix most system issues without losing your files.

## Step 1: Open Command Prompt as Administrator

Press the Start button, type `cmd`, right-click it and choose **Run as administrator**.

## Step 2: Run these commands one at a time

```cmd
DISM /Online /Cleanup-Image /CheckHealth
DISM /Online /Cleanup-Image /ScanHealth
DISM /Online /Cleanup-Image /RestoreHealth
SFC /scannow
```

These tools scan and repair system corruption and missing files. If errors are found, run the last one (SFC) a couple more times.

## Optional: view the DNS cache

```cmd
ipconfig /displaydns
```

## Pro tip

If DISM has trouble repairing, use a Windows ISO file as a source for a clean repair.
