# Cedarline Switch-Port Map

The interface names below are planned. Confirm them against the switch models selected in Packet Tracer before cabling.

## Houston headquarters

| Switch | Planned port | Connected device | VLAN or purpose |
| --- | --- | --- | --- |
| `SW-HOU-01` | `Fa0/1` | Finance workstation | Access VLAN `10` |
| `SW-HOU-01` | `Fa0/2` | Operations workstation | Access VLAN `20` |
| `SW-HOU-01` | `Fa0/3` | Designated IT admin workstation | Access VLAN `30` |
| `SW-HOU-01` | `Fa0/4` | Shared printer | Access VLAN `60` |
| `SW-HOU-01` | `Fa0/5` | Inventory app server | Access VLAN `60` |
| `SW-HOU-01` | `Fa0/6` | Operations IP phone | Voice VLAN `70` |
| `SW-HOU-01` | `Fa0/7` | Staff AP in the Finance work area | Staff Wi-Fi VLAN mapping to confirm in Sprint 3 |
| `SW-HOU-01` | `Fa0/8` | Staff AP in the meeting room | Staff Wi-Fi VLAN mapping to confirm in Sprint 3 |
| `SW-HOU-01` | `Gi0/1` | Uplink to `SW-HOU-02 Gi0/1` | Trunk carrying the VLANs needed on both switches |
| `SW-HOU-01` | `Gi0/2` | Link to Houston gateway | Interface and VLAN configuration to confirm in Sprint 2 |
| `SW-HOU-02` | `Fa0/1` | Reception IP phone | Voice VLAN `70` |
| `SW-HOU-02` | `Fa0/2` | Reception guest AP | Access VLAN `50` |
| `SW-HOU-02` | `Fa0/3` | Entrance camera | Access VLAN `80`, PoE support to verify |
| `SW-HOU-02` | `Fa0/4` | Security viewing workstation | Access VLAN `40` |
| `SW-HOU-02` | `Fa0/5` | Spare reception wall jack | Administratively shut down during hardening |
| `SW-HOU-02` | `Gi0/1` | Uplink to `SW-HOU-01 Gi0/1` | Other end of the Houston trunk |

## Brno branch

| Switch | Planned port | Connected device | VLAN or purpose |
| --- | --- | --- | --- |
| `SW-BRNO-01` | `Fa0/1` | `PC-BRNO-01` | Access VLAN `20` |
| `SW-BRNO-01` | `Fa0/2` | `PC-BRNO-02` | Access VLAN `20` |
| `SW-BRNO-01` | `Fa0/3` | Brno staff AP | Access VLAN `20` for Operations Wi-Fi |
| `SW-BRNO-01` | `Gi0/1` | Link to Brno gateway | Interface configuration to confirm in Sprint 2 |