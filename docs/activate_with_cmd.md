# Activate Windows with CMD

You can activate Windows from Command Prompt if you already have a **valid product key**. A pirated, expired or random key from the internet will not work.

## 1. Open Command Prompt as Administrator

Press `Windows + S`, type `cmd`, right-click **Command Prompt** and click **Run as administrator**.

## 2. Install the product key

Replace the X's with your actual product key:

```cmd
slmgr /ipk XXXXX-XXXXX-XXXXX-XXXXX-XXXXX
```

If successful, you see: "Installed product key XXXXX-XXXXX-XXXXX-XXXXX-XXXXX successfully".

## 3. Set the activation server (KMS/Volume keys only)

Only for organization admins who have their own KMS server. Retail and OEM keys do not need this step, so skip it.

```cmd
slmgr /skms kms.server.com
```

Replace `kms.server.com` with your KMS server.

## 4. Activate Windows

```cmd
slmgr /ato
```

If it worked, you see: "Product activated successfully".

## 5. Check activation status

```cmd
slmgr /xpr
```

If successful, it says: "The machine is permanently activated".

## Troubleshooting

* Make sure you are connected to the internet.
* Errors like `0xC004C003` or `0x8007007B` usually mean the key is invalid or blocked. See [Activation Troubleshooting](activation_troubleshooting.md).
