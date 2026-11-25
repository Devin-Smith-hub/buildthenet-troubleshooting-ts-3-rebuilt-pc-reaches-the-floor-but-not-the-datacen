# Troubleshooting #3: Rebuilt PC Reaches the Floor but Not the Datacenter

_Level: Easy — BuildTheNet 3D Sandbox troubleshooting — Host configuration_

**Status: Verified.** Completed on 2026-11-25; every requirement was machine-checked against the network built in BuildTheNet and passed.

Snapshot taken 2026-11-25. Built in a simulation — no physical or production equipment was involved.

## The ticket

**Reported symptom:** PC-1 opens shares on PC-2 across the desk, but every datacenter app times out.

Helpdesk Ticket #103: PC-1 was rebuilt from an old image this morning. The user can open shares on PC-2 at the next desk, but every application on DC-Server (10.0.0.100) times out. PC-2 has no problems reaching DC-Server.

_Area: Host configuration_

## Required resolution

Fix PC-1 so it reaches DC-Server again. PC-2 and the router are working and must stay as they are.

## Checks

| # | Check | Status | Checked by |
|---|---|---|---|
| 1 | PC-1 to DC-Server works | Passed | Sandbox check |
| 2 | PC-2 to DC-Server still works | Passed | Sandbox check |
| 3 | PC-1 to PC-2 still works | Passed | Sandbox check |
| 4 | PC-2 keeps its documented addressing | Passed | Sandbox check |
| 5 | HQ-Router keeps its documented addressing | Passed | Sandbox check |
| 6 | DC-Server keeps its documented addressing | Passed | Sandbox check |
| 7 | PC-1 FastEthernet0 keeps its documented addressing | Passed | Sandbox check |

## Root cause

PC-1's default gateway is 192.168.20.254, an address no device owns; the router LAN interface is 192.168.20.1.

## The fix

What was changed to resolve the ticket, compared with the network as it was reported:

Compared with the starting topology, the learner changed the configuration of 1 device.

**PC-1**

```diff
- default-gateway 192.168.20.254
+ default-gateway 192.168.20.1
```

## Result

Every check passed on this snapshot:

> PC-1 has a working gateway again and reaches DC-Server; PC-2 is unaffected.

## Topology

![Topology diagram](topology.svg)

## Addressing

| Device | Interface | Address | Mask | Mode | Gateway |
|---|---|---|---|---|---|
| PC-1 | FastEthernet0 | 192.168.20.15/24 | 255.255.255.0 | VLAN 1 | 192.168.20.1 |
| PC-2 | FastEthernet0 | 192.168.20.16/24 | 255.255.255.0 | VLAN 1 | 192.168.20.1 |
| HQ-Router | GigabitEthernet0/0 | 192.168.20.1/24 | 255.255.255.0 | routed |  |
| HQ-Router | GigabitEthernet0/1 | 10.0.0.1/24 | 255.255.255.0 | routed |  |
| DC-Server | FastEthernet0 | 10.0.0.100/24 | 255.255.255.0 | VLAN 1 | 10.0.0.1 |

## Device configurations

Running-configs as `show running-config` prints them in the BuildTheNet sandbox. Passwords, secrets, keys and SNMP communities are replaced with `<removed>`.

- [SW-Desk](configs/SW-Desk.txt)
- [HQ-Router](configs/HQ-Router.txt)

---

Built on [BuildTheNet](https://www.buildthenet.io/sandbox)
