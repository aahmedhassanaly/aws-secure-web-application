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

# IAM - Admin User

## Goal

Create a separate IAM user for daily AWS administration.

## What I Did

- Created an IAM group named `aws-admins`.
- Attached `AdministratorAccess` to the group.
- Created an IAM user named `aws-admin`.
- Added the user to `aws-admins`.
- Enabled console access.
- Enabled MFA for the user.

## Why?

Using a separate IAM user is safer than using the Root User for daily tasks.

Permissions are assigned through the group instead of directly to the user.

## Verification

- Console access: Enabled with MFA
- Permission: AdministratorAccess
- Permission source: Group `aws-admins`

## Security Note

For production environments, permissions should follow the principle of least privilege.
<img width="1488" height="734" alt="image" src="https://github.com/user-attachments/assets/6b4858ce-67dc-4f26-8c73-02e7707feb4a" />
