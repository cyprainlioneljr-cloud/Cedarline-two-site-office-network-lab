# Cedarline IP and VLAN Plan

Houston uses the planning block `10.10.0.0/16`, and Brno uses `10.20.0.0/16`. Each office VLAN has its own `/24` subnet. The home network uses `192.168.50.0/24`. These are planned assignments for the Packet Tracer build.

| Site | VLAN | Purpose | Subnet | Planned gateway |
| --- | ---: | --- | --- | --- |
| Houston | 10 | Finance | `10.10.10.0/24` | `10.10.10.1` |
| Houston | 20 | Operations | `10.10.20.0/24` | `10.10.20.1` |
| Houston | 30 | IT | `10.10.30.0/24` | `10.10.30.1` |
| Houston | 40 | Security | `10.10.40.0/24` | `10.10.40.1` |
| Houston | 50 | Guest Wi-Fi | `10.10.50.0/24` | `10.10.50.1` |
| Houston | 60 | Shared services and printer | `10.10.60.0/24` | `10.10.60.1` |
| Houston | 70 | Voice | `10.10.70.0/24` | `10.10.70.1` |
| Houston | 80 | Cameras | `10.10.80.0/24` | `10.10.80.1` |
| Houston | 90 | Switch management | `10.10.90.0/24` | `10.10.90.1` |
| Brno | 20 | Operations | `10.20.20.0/24` | `10.20.20.1` |
| Home | — | Remote employee | `192.168.50.0/24` | `192.168.50.1` |

Both offices use VLAN 20 for Operations. The site-to-site VPN will route between their distinct IP subnets.