# Cedarline Access Matrix

| Test | Source | Destination and action | Expected | Observed |
| --- | --- | --- | --- | --- |
| A01 | Houston Finance PC | Reach the shared printer | Allow | Pending |
| A02 | Houston Finance PC | Reach an Operations workstation | Deny | Pending |
| A03 | Houston guest laptop | Open the simulated external web app | Allow | Pending |
| A04 | Houston guest laptop | Open the Houston inventory app | Deny | Pending |
| A05 | Houston guest laptop | Reach the shared printer | Deny | Pending |
| A06 | Houston guest laptop | View the entrance camera | Deny | Pending |
| A07 | Houston Security viewing workstation | View the entrance camera | Allow | Pending |
| A08 | Houston Finance PC | View the entrance camera | Deny | Pending |
| A09 | Houston Operations PC | View the entrance camera | Deny | Pending |
| A10 | Designated Houston IT admin workstation | Connect to a Houston switch over SSH | Allow | Pending |
| A11 | Houston Security viewing workstation | Connect to the same switch over SSH | Deny | Pending |
| A12 | Houston guest laptop | Connect to the same switch over SSH | Deny | Pending |
| A13 | Brno Operations PC | Open the Houston inventory app over the site-to-site VPN | Allow | Pending |
| A14 | Brno Operations PC | Reach a Houston Finance workstation | Deny | Pending |
| A15 | Brno Operations PC | Connect to a Houston switch over SSH | Deny | Pending |
| A16 | Home laptop signed in through remote-access VPN | Open the Houston inventory app | Allow if supported | Simulator support to verify |
| A17 | Home laptop signed in through remote-access VPN | Connect to a Houston switch over SSH | Deny if supported | Simulator support to verify |

Record the actual result and link to the sprint evidence after running each test. Test the intended service where the simulator supports it. Record a ping as a reachability check when the intended service cannot be tested.