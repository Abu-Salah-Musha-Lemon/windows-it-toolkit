# Windows Activation Troubleshooting

This guide explains how to inspect Windows activation status and
understand common license channels. It does not provide product keys or
unofficial activation methods.

> **Important:** A product key displayed by a command or utility is not,
> by itself, proof that you own a valid Windows license. Activation
> status, license entitlement, and transfer rights are different
> questions.

## 1. Check activation in Settings

On Windows 11:

1.  Open **Settings**.
2.  Select **System**.
3.  Select **Activation**.
4.  Review the activation status and any displayed error message.

On Windows 10:

1.  Open **Settings**.
2.  Select **Update & Security**.
3.  Select **Activation**.
4.  Review the status and any displayed error message.

The exact wording may vary by Windows version and license channel.

## 2. Inspect the installed license information

Open Command Prompt and run:

``` bat
slmgr /dli
```

For more detailed licensing information, run:

``` bat
slmgr /dlv
```

These commands open the Windows Script Host licensing dialog. They can
show information such as the installed license channel and partial
product key.

To view the activation expiration information, run:

``` bat
slmgr /xpr
```

`/xpr` reports whether activation is permanent or has an expiration
date. It does **not** independently prove that a license was purchased
legitimately, that it is transferable, or that the user owns the
entitlement.

Do not post a full product key, license identifier, or unredacted
licensing output in a public issue.

## 3. Understand common license channels

  -----------------------------------------------------------------------
  Channel or status       General meaning         What to check
  ----------------------- ----------------------- -----------------------
  Retail                  A license obtained      Keep proof of purchase
                          through a retail        and check the license
                          purchase or authorized  terms for transfer
                          seller.                 rights.

  OEM                     A license supplied with Check the device's
                          a device by its         original license
                          manufacturer or system  documentation. OEM
                          builder.                transfer rights are
                                                  generally restricted to
                                                  the licensed device,
                                                  subject to the
                                                  applicable terms.

  Volume / KMS client     A device configured to  If it is a work or
                          activate through an     school device, contact
                          organization's          the organization's IT
                          volume-licensing        administrator.
                          infrastructure.         

  MAK                     A Multiple Activation   Contact the
                          Key used under an       organization that owns
                          organization's          and administers the
                          volume-licensing        volume license.
                          agreement.              

  Digital license         An activation           Confirm the installed
                          entitlement associated  Windows edition matches
                          with the device and     the edition for which
                          Microsoft's activation  the device is licensed.
                          service.                
  -----------------------------------------------------------------------

A license channel is not a complete legal determination. The purchase
record, license terms, device history, and organization agreement
matter.

## 4. What organization-managed activation means

If Settings says Windows is activated using an organization's activation
service, the device may be using an organization-managed volume
activation method such as KMS.

-   On an organization-owned device, contact the organization's IT team.
-   On a personal device, do not assume that an organization-managed
    activation is a personal retail license.
-   Do not use public KMS servers, activation scripts, cracks, or
    third-party activators.
-   If you need a personal license, obtain it from Microsoft or an
    authorized seller and follow the official activation process.

KMS is a legitimate technology when used by an organization under the
appropriate licensing agreement. Using an unauthorized server or
activator is not a substitute for a valid license.

## 5. If Windows activated automatically after installation

Automatic activation can occur when Microsoft's activation service
recognizes an existing digital entitlement for the device, or when a
valid firmware-embedded OEM key applies to the installed edition.

It does not necessarily mean that a new retail product key was issued
during installation.

If activation later changes or fails:

1.  Confirm that the installed edition is the same edition covered by
    the device's license.
2.  Connect to the internet and check activation again.
3.  Review the exact error code in Settings.
4.  If available, use the Activation Troubleshooter.
5.  If the device belongs to an organization, contact its IT
    administrator.
6.  If the device was purchased with Windows, contact the manufacturer
    or seller with proof of purchase.
7.  If you purchased a separate license, contact the seller or Microsoft
    support.

A major hardware change, edition mismatch, organization policy, or
license-channel issue may affect activation. Do not assume that
reinstalling Windows will resolve every activation problem.

## 6. Protect your product key

-   Never publish a full product key in a GitHub repository, screenshot,
    issue, or public chat.
-   Do not send a full key to an online key-checking website.
-   Do not commit keys, activation tokens, credentials, or licensing
    records to source control.
-   Redact partial keys and device identifiers before sharing command
    output.

## 7. About generic installation keys

Generic installation keys may be used for edition selection or
installation scenarios, but they do not grant a license or prove
activation entitlement. This project intentionally does not reproduce
generic product-key lists.

Use the license supplied with the device or a license obtained through
an authorized purchase. Follow Microsoft's official activation and
licensing guidance for the applicable Windows edition.

## 8. Useful commands

  Command        Purpose
  -------------- --------------------------------------------------
  `slmgr /dli`   Displays basic installed license information.
  `slmgr /dlv`   Displays detailed installed license information.
  `slmgr /xpr`   Displays activation expiration information.

These commands inspect licensing state. They do not create a license or
independently certify that a license is genuine or transferable.
