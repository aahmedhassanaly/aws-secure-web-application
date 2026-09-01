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
<img width="1882" height="849" alt="image" src="https://github.com/user-attachments/assets/1768349d-3fd1-4e98-a715-b03ccf19046f" />
