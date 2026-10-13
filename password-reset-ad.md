# Password Reset in Active Directory

## Symptoms

- User says they forgot their password or it "stopped working"
- User gets "The user name or password is incorrect" at sign-in

## Quick checks

1. **Verify the caller's identity** using your organization's process (employee ID, manager callback, security questions). Never reset a password for someone you haven't verified.
2. Is **Caps Lock** on? Is the keyboard set to the right language?
3. Is the user typing their **username** correctly (`jsmith`, not `jsmith@gmail.com`)?
4. Is the account **locked out** instead? If so, see [Account lockout](account-lockout.md). Unlocking alone may fix it.

## Fix

### Option A: Active Directory Users and Computers (ADUC)

1. Open **Active Directory Users and Computers**.
2. Right-click the domain and choose **Find**, then search for the user.
3. Right-click the user and choose **Reset Password**.
4. Enter a temporary password that meets the domain password policy.
5. Check **User must change password at next logon**.
6. If the account is locked, check **Unlock the user's account**.
7. Click **OK**.

### Option B: PowerShell

```powershell
$temp = Read-Host "Temporary password" -AsSecureString
Set-ADAccountPassword -Identity jsmith -Reset -NewPassword $temp
Set-ADUser -Identity jsmith -ChangePasswordAtLogon $true
Unlock-ADAccount -Identity jsmith
```

### After the reset

1. Give the temporary password through an approved channel. Don't send it by email to the account you just reset.
2. Stay on the line while the user signs in and sets a new password.
3. Remind them to update the password on their **phone email app** and any other saved places, or they may get locked out again.

## Escalate when

- The account is **disabled** (could mean they were offboarded; check with their manager)
- The user is an **admin** or has elevated access (follow your organization's privileged account process)
- The reset doesn't work after replication time (possible domain controller issue)

## Ticket notes

```
Issue: User unable to sign in, forgot password.
Identity verified via: [method]
Action: Reset password in AD, set "change at next logon", [unlocked account if needed].
Result: User signed in and set a new password while on the call.
Follow-up: Advised user to update saved password on mobile device.
```
