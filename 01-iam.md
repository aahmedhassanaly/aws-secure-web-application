# IAM - Root MFA

## Goal

Secure the AWS Root User with MFA.

## What I Did

- Enabled a Virtual MFA device.
- Verified that MFA is active.

## Why?

MFA adds an extra security step when logging in.

The Root User has full access to the AWS account, so MFA is important.

## Verification

MFA is enabled and the device appears in the AWS security credentials page.

## Security Note

I will not use the Root User for daily AWS tasks.

<img width="1882" height="849" alt="Screenshot 2026-09-01 172546" src="https://github.com/user-attachments/assets/b1fab9fc-7c50-414f-b6bb-1c048330dea5" />
