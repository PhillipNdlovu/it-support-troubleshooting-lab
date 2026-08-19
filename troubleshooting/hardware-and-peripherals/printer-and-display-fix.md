# Hardware & Peripherals Troubleshooting Guide

## Overview
This document logs practical resolution procedures for physical hardware, printing infrastructure, and peripheral display issues common in modern office environments.

---

## Case 1 — Network Printer Stuck in "Offline" Status / Spooler Error

### Problem
Multiple users in the finance department were unable to print documents to a shared network printer. Jobs were queuing up in the Windows Print Queue, and the printer status was showing as "Offline."

### Action Taken
1. **Network Connectivity Test:** Pings sent to the printer's assigned static IP address (`10.10.40.15`) timed out, confirming network disconnection.
2. **Physical / ARP Inspection:** Checked physical Ethernet cable and switch port link LEDs. Rebooted the printer to re-initialize its NIC.
3. **Print Spooler Reset:**
   - Opened Command Prompt as Administrator on the print server/workstation.
   - Stopped the print spooler service: `net stop spooler`
   - Cleared stuck spool files from: `C:\Windows\System32\spool\PRINTERS\`
   - Restarted the print spooler service: `net start spooler`
4. **Port Reconfiguration:** Verified the Standard TCP/IP Port configuration in Printer Properties and disabled "SNMP Status Enabled" to prevent false offline reporting.

### Result
The printer reconnected successfully, clear print queues resumed processing automatically, and end-users confirmed document output.

---

## Case 2 — Dual-Monitor Detection and Resolution Scaling Issues

### Problem
A workstation was upgraded to a dual-monitor setup. The second monitor displayed "No Signal," and the primary monitor's resolution was stretched and blurry after a Windows update.

### Action Taken
1. **Physical Cable & Port Inspection:** Verified DP/HDMI cable connections. Swapped port outputs on the graphics card to rule out bad physical ports.
2. **Display Detection:** Navigated to **Display Settings** ➔ **Multiple displays** and clicked **Detect**.
3. **Driver Rollback / Reinstallation:**
   - Opened **Device Manager** (`devmgmt.msc`).
   - Expanded **Display adapters**, right-clicked the graphics driver, and noticed it had reverted to the generic "Microsoft Basic Display Adapter."
   - Downloaded and installed the official manufacturer display driver package.
4. **Resolution Adjustments:** Configured native resolutions (1920x1080) for both monitors and selected "Extend these displays" under Windows Display options.

### Result
Both monitors rendered correctly in native resolution with extended desktop functionality restored.

---

## 🛠️ Summary of Commands Used

| Command | Purpose |
| :--- | :--- |
| `net stop spooler` | Halts the Windows Print Spooler service to unlock queue files |
| `net start spooler` | Restarts the Windows Print Spooler service after cleanup |
| `devmgmt.msc` | Opens Device Manager to manage hardware drivers |
| `printmanagement.msc` | Opens Print Management console for driver and port auditing |

