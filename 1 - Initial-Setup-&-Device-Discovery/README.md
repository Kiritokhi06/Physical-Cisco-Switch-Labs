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

```cli
Switch# delete flash:vlan.dat
```

The switch reported that the file didn't exist at that location. Rather than treating this as a failed reset, I continued with the device discovery commands and verified the VLAN state afterward.

![Write erase cmd](<./screenshots/Write_erase_cmd.png>)

## Identifying the Switch and IOS Version

After the reload, I used the **show version** command to identify the switch model, installed IOS version, and hardware specifications.

The output identified the device as a Cisco Catalyst 3750 with the following details:

- Model: WS-C3750-48TS-E
- IOS Version: 12.2(25)SEE2
- IOS Image: C3750-IP SERVICES-M
- FastEthernet Interfaces: 48
- GigabitEthernet Interfaces: 4

The switch was running an older IOS release, which is important for me to consider when selecting commands and security features for future labs. Some features and commands available on newer Cisco platforms may not be supported by this software version.

![Show version cmd](<./screenshots/Show_version_cmd.png>)

## Inspecting Interface Status

I used the **show interfaces status** command to inspect the switch's physical interfaces and review their current status, VLAN assignments, duplex settings, and speeds.

The inspected interfaces were shown as **notconnect**, assigned to VLAN 1, with speed and duplex set to **auto**. This was expected because no end devices were connected to the switch ports during the inspection.

The switch provides 48 FastEthernet ports and 4 GigabitEthernet ports, giving me a baseline for future interface configuration, VLAN assignment, and trunking exercises.

Since the interface results were repetitive, I retained a representative screenshot rather than documenting every port individually.

![Show interfaces status cmd](<./screenshots/show_interfaces_status_cmd.png>)

## Verifying the VLAN Database

To inspect the switch's VLAN database, I ran:

```cli
Switch# show vlan brief
```

The output showed VLAN 1, named **default**, in the active state, with the switch's FastEthernet and GigabitEthernet ports assigned to it. The default legacy VLANs 1002 through 1005 were also present.

No custom VLANs appeared in the output. This confirmed the default VLAN state at the time of inspection and established a baseline for the VLAN creation and port assignment project.

![Show vlan brief cmd](<./screenshots/Show_vlan_brief_cmd.png>)

## Reviewing the Running Configuration

Finally, I used the following command to inspect the active configuration:

```cli
Switch# show running-config
```

The output displayed the switch's current configuration, including the default hostname, service settings, and spanning-tree mode. The hostname remained Switch, and the switch was using Per-VLAN Spanning Tree (PVST).

This provided an additional reference for the switch's starting configuration before making changes in subsequent labs.

![Show running-config cmd](<./screenshots/show_running_config_cmd.png>)

