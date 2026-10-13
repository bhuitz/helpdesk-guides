# GPU Instability on a Custom PC

*From my own build: an AMD Ryzen 7 9800X3D system with a Radeon RX 9070 XT. About three months after the build, the GPU became unstable. I ruled out the power supply, updated the BIOS, set the memory to EXPO, and replaced the faulty card. This guide puts those steps together with the other standard checks into one process.*

## Symptoms

- Games or GPU-heavy programs crash
- Screen goes black, flickers, or shows colored artifacts
- Driver errors, such as "the display driver stopped responding"

## Quick checks

1. **Driver:** Is the graphics driver current? Was it updated right before the problems started?
2. **Temperatures:** Check GPU temperature under load with the driver's software. Overheating causes crashes.
3. **Cables:** Are the PCIe power cables fully seated at both the GPU and the power supply?
4. **Recent changes:** Any new hardware, overclock, or BIOS change?

## Fix

Rule out the cheap and easy causes before deciding the card is bad.

### 1. Clean-install the driver

Remove the current driver completely (a tool like DDU in Safe Mode works well), then install the latest version from the GPU maker's website.

### 2. Rule out the power supply

A failing or underpowered PSU can look exactly like a bad graphics card.

- Confirm the PSU wattage meets the card's requirement. My 1000 W Corsair RM1000x had plenty of headroom.
- Use **separate PCIe cables** for each power connector on the GPU instead of one split cable.
- Reseat the cables at both ends.
- If possible, test with a known-good power supply.

### 3. Make sure the platform is stable

- **Update the BIOS** to the latest version from the motherboard maker.
- Set memory with **EXPO** (AMD's memory profile) so the RAM runs at its rated speed and timings, instead of a manual overclock.
- Reset any GPU overclock or undervolt to stock settings.

### 4. Isolate the GPU

If it's still unstable with a clean driver, a good PSU, stock settings, and an updated BIOS, test the card in another PC or test a known-good card in this PC. Whichever way the problem follows tells you where the fault is.

### 5. Replace the faulty part

In my case, the instability stayed after ruling out the PSU and the platform, so the graphics card was the fault. I replaced it, and the system has been stable since.

## Escalate when

In a workplace, Tier 1 usually does the quick checks and driver reinstall. Hardware swaps and warranty claims usually go to desktop support or the vendor.

## Ticket notes

```
Issue: GPU crashes / artifacts under load on [computer name].
Checks: Driver clean-installed; temps normal; PSU cables reseated; PSU ruled out.
Platform: BIOS updated; memory set to EXPO; GPU at stock settings.
Result: Instability continued; GPU identified as faulty and replaced.
Follow-up: Monitored after replacement; system stable.
```
