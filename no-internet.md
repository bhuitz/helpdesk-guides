# No Internet Connection

## Symptoms

- Websites won't load
- Network icon shows "No internet" or a globe
- Some sites work but others don't

## Quick checks

1. Is it **just this user** or **everyone nearby**? If many people are affected, it's likely a network outage. Escalate instead of troubleshooting one PC.
2. Is the **Ethernet cable** plugged in at both ends, or is **Wi-Fi** turned on and connected to the right network?
3. Is **airplane mode** off?
4. Has the user **restarted the computer**?

## Fix

Work from the bottom of the network up. Open **Command Prompt** for these steps.

### 1. Check the IP address

```cmd
ipconfig
```

- Address starts with **169.254** → the PC didn't get an address from DHCP. Go to step 2.
- No address at all → the adapter is disabled or disconnected. Check the cable and Wi-Fi.
- Normal address (like 192.168.x.x or 10.x.x.x) → go to step 3.

### 2. Get a new address

```cmd
ipconfig /release
ipconfig /renew
```

### 3. Test each layer

```cmd
ping <default gateway from ipconfig>   :: Can it reach the router?
ping 8.8.8.8                           :: Can it reach the internet by IP address?
ping google.com                        :: Does name lookup (DNS) work?
```

- Gateway fails → local network problem (cable, switch port, Wi-Fi)
- Gateway works, 8.8.8.8 fails → problem past the router (firewall or ISP)
- 8.8.8.8 works, google.com fails → **DNS problem**. Go to step 4.

### 4. Fix DNS

```cmd
ipconfig /flushdns
nslookup google.com
```

If `nslookup` fails, check which DNS server the PC is using (`ipconfig /all`). On a company network it should be the company DNS server, not one the user set by hand.

### 5. Reset the network stack (last resort)

```cmd
netsh winsock reset
netsh int ip reset
```

Then restart the computer.

## Escalate when

- Multiple users or a whole area is down
- The gateway can't be reached from a known-good cable and port
- DHCP isn't handing out addresses (possible server issue)

## Ticket notes

```
Issue: No internet access on [computer name].
Scope: [single user / multiple users]
Findings: [ipconfig result, which ping step failed]
Action: [e.g., released/renewed IP, flushed DNS, reseated cable]
Result: Connectivity restored; confirmed websites load.
```
