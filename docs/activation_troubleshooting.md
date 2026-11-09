# Activation Troubleshooting

## Check your activation status

Go to **Settings > System > Activation** (on older builds: **Settings > Update & Security > Activation**).

| Message | Meaning |
| --- | --- |
| Windows is activated with a digital license | Activated automatically using a valid license stored on Microsoft's servers. |
| Windows is activated with a digital license linked to your Microsoft account | The license is also tied to your Microsoft account. After a reinstall or hard drive change it can reactivate when you sign in. |
| Windows is activated using your organization's activation service | Probably a KMS (volume) license. See below. |

If activation fails, click **Troubleshoot** on the same page. It can restore activation if you had a digital license.

Common error codes:

* `0xC004F034`: No valid key found
* `0xC004C003`: Blocked or invalid key
* `0x803F7001`: No product key found
* `0x8007007B`: Usually an invalid or blocked key

## Why Windows can deactivate after a while

1. **Digital license lost.** Activation is tied to your hardware (such as the motherboard) or your Microsoft account. Changing key hardware, or reinstalling without signing into the same Microsoft account, can break it. Sometimes Windows forgets the license after a glitch or update.
2. **Temporary activation via a generic or KMS key.** KMS activation lasts about 180 days unless it renews. After that you see "Windows is not activated".
3. **Unauthorized or cracked activation.** Keys from third party tools or activators (sometimes bundled in modded ISOs) can be detected and revoked.
4. **Expired evaluation or Insider Preview.** These versions expire after a trial period, usually 90 to 180 days.

## What is KMS activation?

KMS (Key Management Service) is used by businesses, schools and government organizations to activate many PCs through a central server. A KMS client:

* is set up with a KMS key
* contacts the KMS server regularly
* deactivates if it cannot reach the server for around 180 days

If the laptop is a work or school device, keep it connected to their network from time to time. If it is a personal device that shows this message, someone may have used an unofficial activation method, which is not a valid personal license. In that case, buy a retail license key, or use Windows unactivated (you lose personalization settings, but it still works).

## Check your license type

Open Command Prompt as Administrator and run:

```cmd
slmgr /dli
```

Look for these phrases in the result:

* **Retail:** bought directly from Microsoft or an authorized seller.
* **OEM:** pre-installed on a laptop or desktop. Valid, but tied to that device.
* **Volume:GVLK / VOLUME_KMSCLIENT:** used by companies and schools. Only valid if it checks in with their servers.
* **MAK:** a limited-use key used by some organizations.

## Check whether Windows is permanently activated

```cmd
slmgr /xpr
```

* "The machine is permanently activated" means the license is permanent.
* "Volume activation will expire on..." means it is not permanently activated.

## License types at a glance

| Type | Description | Transferable? | Genuine? |
| --- | --- | --- | --- |
| Retail | Bought directly or online | Yes | Yes |
| OEM | Comes with the PC or laptop | No | Yes |
| Volume | Used in organizations (KMS/MAK) | No (usually) | Yes, if used legally |
| KMS crack | Activated via an unauthorized tool | No | No |

## Will activation last for life?

Usually yes, if:

* the activation is linked to your Microsoft account, or the device came with Windows pre-installed
* you do not make major hardware changes (such as replacing the motherboard)
* you do not change editions (such as Home to Pro)

You may lose activation if you do a clean install on a new drive without signing in with your Microsoft account, change the motherboard, or the system used a volume or KMS license that expires.

## Online key checkers

Avoid giving your full product key to third party checker sites. Most are not safe or trustworthy.
