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
