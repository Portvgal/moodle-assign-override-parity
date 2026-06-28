# Security Policy

## Supported Versions

This repository contains patch files for Moodle Assignment overrides. Security fixes are considered for the patch versions currently maintained in this repository.

| Target Moodle version | Patch file | Supported |
| --- | --- | --- |
| Moodle 5.2.x | `patches/assign-override-maxattempts-moodle52.patch` | Yes |
| Moodle 4.5.x | `patches/assign-override-maxattempts.patch` | Yes |
| Other Moodle versions | Not provided | No |

## Reporting a Vulnerability

Please do not report security vulnerabilities in public issues, discussions, or pull requests.

To report a vulnerability, use GitHub private vulnerability reporting if available for this repository. If private reporting is not available, open a minimal issue asking for a private security contact, without including exploit details, private data, credentials, or reproduction steps.

Please include:

- A clear description of the issue.
- The affected Moodle version.
- Which patch file was applied.
- Steps to reproduce, if safe to share privately.
- Expected and actual behaviour.
- Any relevant error messages or logs with sensitive data removed.

I will aim to acknowledge valid reports within 7 days.

## Scope

Reports are in scope when they relate to this repository's patch behaviour, including Assignment user or group overrides, allowed attempts, backup/restore handling, upgrade behaviour, privacy export handling, or permission-sensitive Assignment workflows.

General Moodle core vulnerabilities should also be reported through Moodle's official security process, because this repository is not the upstream authority for Moodle core.

## Public Disclosure

Please allow time for investigation and, where needed, coordination with Moodle upstream before publicly disclosing a vulnerability.
