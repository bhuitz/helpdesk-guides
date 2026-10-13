# Map a Network Drive

## Symptoms

- A shared drive (like `S:` or `H:`) is missing from File Explorer
- Drive shows a red X or "disconnected"
- "Access is denied" when opening a shared folder

## Quick checks

1. Is the user **connected to the company network** or VPN? Network drives don't work without it.
2. Get the **exact share path** from the user or documentation, for example `\\fileserver\shared`.
3. Can the user open the path directly? Press **Win + R**, type the path, and press Enter.
   - Opens → the drive mapping is the problem. Remap it.
   - "Access is denied" → a permissions problem. See *Escalate when*.
   - "Windows cannot access" → a network or server problem.

## Fix

### Option A: File Explorer

1. Open **File Explorer** and click **This PC**.
2. Click **⋯ (See more)** → **Map network drive**.
3. Choose a drive letter.
4. Enter the folder path, for example `\\fileserver\shared`.
5. Check **Reconnect at sign-in**.
6. Click **Finish**. Enter credentials if asked.

### Option B: Command line

```cmd
net use S: \\fileserver\shared /persistent:yes
```

To remove a broken mapping first:

```cmd
net use S: /delete
```

### Option C: PowerShell

```powershell
New-PSDrive -Name S -PSProvider FileSystem -Root \\fileserver\shared -Persist
```

### If the drive keeps disconnecting

- Clear old saved credentials in **Credential Manager** → Windows Credentials
- Check whether the drive is supposed to be mapped by **Group Policy**. Run `gpupdate /force`, then sign out and back in.

## Escalate when

- The user gets "Access is denied." Permission changes need approval from the data owner or manager.
- The server can't be reached by anyone
- The drive should come from Group Policy but doesn't appear after `gpupdate`

## Ticket notes

```
Issue: [Drive letter] drive missing / not connecting.
Path: \\server\share
Findings: [opens directly / access denied / server unreachable]
Action: [remapped drive / cleared saved credentials / ran gpupdate]
Result: User can open the drive and access files.
```
