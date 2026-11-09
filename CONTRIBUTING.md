# Contributing

Thank you for considering a contribution to Windows IT Toolkit.

The project contains Windows maintenance, repair, diagnostics, and
inventory scripts. Changes should prioritize correctness, transparency,
recoverability, and protection of user data.

## Before opening an issue

-   Search existing issues to avoid duplicates.
-   State the Windows edition, version, and build where the problem
    occurred.
-   Include the exact command or menu option used.
-   Include the error message and relevant sanitized log lines.
-   Remove usernames, computer names, serial numbers, MAC addresses,
    product keys, tokens, and other private information.

## Before submitting a pull request

1.  Explain the problem the change addresses.
2.  Describe the behavior before and after the change.
3.  Identify the affected script and documentation.
4.  State the Windows versions and PowerShell versions tested.
5.  Test on a virtual machine or non-production computer where possible.
6.  Update the tool reference, usage guide, and safety guide if behavior
    changes.
7.  Do not include raw system reports, generated logs, credentials,
    activation keys, or personal data.

## Script guidelines

-   Use descriptive filenames and consistent naming.
-   Quote file paths and variables in Batch and PowerShell commands
    where appropriate.
-   Check command exit codes and handle failures explicitly.
-   Do not report success when an operation has failed or has not been
    verified.
-   Avoid deleting broad directories or changing registry values without
    documenting the exact effect.
-   Stop services only when necessary and verify that they are restarted
    or report why they could not be restarted.
-   Make restart behavior explicit. Do not restart a computer without
    clear user confirmation.
-   Explain administrator requirements.
-   Prefer built-in Windows tools when they are sufficient.
-   Do not hardcode credentials, SMTP passwords, API keys, product keys,
    or private endpoints.
-   Keep generated logs and reports outside tracked source files.

## Documentation guidelines

-   Describe what the script actually does, not what its filename
    suggests.
-   List prerequisites, administrator requirements, output locations,
    side effects, and known limitations.
-   Distinguish tested behavior from expected behavior.
-   Keep warnings close to the operation they describe.
-   Use relative links for files within the repository.

## Testing checklist

Before submitting a repair-script change, consider testing:

-   Normal completion
-   Missing administrator privileges
-   Missing or unavailable Windows features
-   Service stop/start failures
-   DISM or SFC error conditions
-   Paths containing spaces
-   Windows versions or editions that do not support a queried feature
-   Whether generated logs contain useful error details
-   Whether a restart is required and whether the user is clearly
    informed

Do not test destructive operations on a primary computer containing
important data.

## Pull request checklist

-   [ ] The purpose and impact of the change are explained.
-   [ ] The affected scripts were reviewed.
-   [ ] Windows and PowerShell versions tested are stated.
-   [ ] Errors and failure paths were considered.
-   [ ] Documentation was updated.
-   [ ] No credentials, product keys, or personal reports are included.
-   [ ] `git status` was checked before committing.

## License

The repository does not currently specify a license. Until the project
owner adds one, do not assume that contributions or repository contents
may be redistributed under an open-source license.
