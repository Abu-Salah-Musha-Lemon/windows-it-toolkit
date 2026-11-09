# Find Your Windows Product Key

The right method depends on how your Windows was installed.

## 1. Check with a command (if stored in BIOS)

Common for laptops that came with Windows pre-installed.

1. Right-click the Start menu and click **Windows PowerShell (Admin)** or **Command Prompt (Admin)**.
2. Paste this command:

```powershell
(Get-WmiObject -query 'select * from SoftwareLicensingService').OA3xOriginalProductKey
```

3. Press Enter. If the key is stored in your BIOS/UEFI, it shows up right there.

## 2. Use a key finder tool

If the command shows nothing, use a free tool:

* ProduKey (by NirSoft)
* ShowKeyPlus (available from Microsoft Store)

These read the key from your system (BIOS or registry). They are especially useful for older Windows versions or upgrades.

## 3. Check for a physical sticker

On older laptops (especially with Windows 7 or early Windows 10):

* Look under the laptop, inside the battery compartment, or under an access panel.
* You might see a COA (Certificate of Authenticity) label with the key printed on it.

## Notes

* If your PC uses digital licensing (for example after a free Windows 10/11 upgrade), there may be no product key shown. The system activates automatically when online.
* If you reinstalled Windows and skipped entering the key, it might still activate because it is linked to your device.

To check what type of key you have, see [Activation Troubleshooting](activation_troubleshooting.md).
