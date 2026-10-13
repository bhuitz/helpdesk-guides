# Printer Not Connecting

## Symptoms

- Printer shows **Offline**
- Documents get stuck in the print queue
- Printer is missing from the user's list

## Quick checks

1. Is the printer **on**, with no error on its screen (paper jam, out of toner, out of paper)?
2. Can **other people** print to it? If not, the problem is the printer or network, not the user's PC.
3. Is the user's computer on the **right network**?

## Fix

### 1. Clear the print queue

1. Open **Settings → Bluetooth & devices → Printers & scanners**, select the printer, and click **Open print queue**.
2. Cancel all stuck documents.

If documents won't clear, restart the Print Spooler service in an admin Command Prompt:

```cmd
net stop spooler
del /Q /F %systemroot%\System32\spool\PRINTERS\*
net start spooler
```

### 2. Check the connection

Find the printer's IP address (usually on its screen or a configuration page you can print from the printer menu), then:

```cmd
ping <printer IP>
```

- Ping fails → the printer is off the network. Check its cable or Wi-Fi, or restart the printer.
- Ping works → go to step 3.

### 3. Remove and re-add the printer

1. In **Printers & scanners**, select the printer and click **Remove**.
2. Click **Add device**. If it doesn't appear, choose **Add manually** and enter the IP address or the shared path (for example `\\printserver\HR-Printer`).
3. Print a test page.

### 4. Update the driver

Download the latest driver from the manufacturer's website for the exact model, and reinstall.

## Escalate when

- Nobody can print to the printer (hardware or network issue)
- The printer needs a repair or part replacement
- The print server is down

## Ticket notes

```
Issue: Unable to print to [printer name / location].
Scope: [single user / all users]
Findings: [queue stuck / printer offline / ping failed]
Action: [cleared queue, restarted spooler, re-added printer, updated driver]
Result: Test page printed successfully.
```
