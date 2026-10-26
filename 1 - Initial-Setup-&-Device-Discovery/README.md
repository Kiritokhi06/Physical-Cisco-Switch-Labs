## Initial Setup and Device Discovery

### Overview

The goal of this project was to establish console access to a physical Cisco Catalyst 3750 switch, clear its previous saved configuration, and inspect its hardware, IOS version, interfaces, and default VLAN database. This established a baseline for the switching and management labs that follow.

> **Additional Documentation:**  
> More detailed screenshots from each stage of the process are available in the [`screenshots`](./screenshots/) directory.

## Console Access and Initial Setup

I connected my Windows laptop to the Cisco Catalyst 3750 using a USB-to-RJ45 console cable. I used Windows Device Manager to identify the assigned COM port and configured PuTTY to establish a serial connection to the switch.

The console connection used the following PuTTY serial settings:

- Serial Line: COM3
- Speed: 9600 baud
- Data Bits: 8
- Stop Bits: 1
- Parity: None
- Flow Control: None

These settings establish the standard serial connection parameters used to access the switch's command-line interface (CLI).

![PuTTY(1)](<./screenshots/PuTTY(1).png>)

## Clearing the Previous Configuration

Before beginning the switching labs, I wanted to start with a clean configuration rather than inherit settings left by a previous user. From privileged EXEC mode, I used the following command to erase the saved configuration in NVRAM:

```cli
Switch# write erase
```

After confirming the operation, the switch reported that the NVRAM erase was complete. I then reloaded the switch to restart it with the saved configuration cleared.

I also attempted to delete the VLAN database file using the following command:
