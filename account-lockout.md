# Account Lockout

## Symptoms

- "The referenced account is currently locked out and may not be logged on to"
- User is locked out again shortly after being unlocked

## Quick checks

1. **Verify the caller's identity.**
2. Confirm the account is actually locked:
   ```powershell
   Get-ADUser jsmith -Properties LockedOut, BadLogonCount, PasswordLastSet |
       Select-Object Name, LockedOut, BadLogonCount, PasswordLastSet
   ```
3. Did the user **recently change their password**? A repeat lockout right after a change almost always means an old password is saved somewhere.

## Fix

### One-time lockout

1. Unlock the account:
   ```powershell
   Unlock-ADAccount -Identity jsmith
   ```
   Or in ADUC: user properties → **Account** tab → check **Unlock account**.
2. Have the user sign in while you're on the call.
3. If they don't remember the password, follow [Password reset](password-reset-ad.md).

### Lockout keeps coming back

Something is retrying the old password. Check these, most common first:

1. **Phone or tablet email app** still using the old password
2. **Mapped network drives** saved with old credentials
3. **Windows Credential Manager:** Control Panel → Credential Manager → Windows Credentials. Remove old entries for company servers.
4. **A second computer** the user is still signed in to
5. **Scheduled tasks or services** running under the user's account

To find which device is causing it, look on a domain controller for **Event ID 4740** in the Security log. The event shows the "Caller Computer Name" that sent the bad password.

```powershell
Get-WinEvent -FilterHashtable @{LogName='Security'; Id=4740} -MaxEvents 20 |
    Where-Object { $_.Message -match 'jsmith' } |
    Select-Object TimeCreated, Message
```

## Escalate when

- Lockouts keep happening and no saved password can be found
- Many accounts are being locked out at once (could be a password-spraying attack; escalate to security right away)
- The lockouts are coming from a device you don't recognize

## Ticket notes

```
Issue: Account locked out [first time / repeat].
Identity verified via: [method]
Cause: [e.g., old password saved in phone mail app / Credential Manager]
Action: Unlocked account, [removed saved credentials / updated phone].
Result: User signed in successfully; no further lockouts after [time].
```
