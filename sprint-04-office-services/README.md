# Sprint 04 · Office services

Houston now has two registered IP phones with a verified internal call, a reachable shared printer, a Security workstation, and an entrance camera managed through the IoT registration portal. Open [cedarline-office-services.pkt](cedarline-office-services.pkt) in Cisco Packet Tracer.

## Device and address plan

| Device | Role | VLAN | Address or extension | Connection |
| --- | --- | --- | --- | --- |
| SW-HOU-POE-01 | 3560-24PS PoE switch | 70, 80 | Layer 2 switching | Gi0/1 to SW-HOU-02 Gi0/2 |
| R-HOU-VOICE-01 | 2811 call service | 70 | 10.10.70.10 (call service router) | Fa0/0 to PoE switch Fa0/24 |
| IPPHONE-HOU-RECEPTION-01 | 7960 Reception phone | 70 | 10.10.70.22 (Reception phone), extension 7001 | PoE switch Fa0/1 |
| IPPHONE-HOU-OPS-01 | 7960 Operations phone | 70 | 10.10.70.21 (Operations phone), extension 7002 | PoE switch Fa0/2 |
| PRN-HOU-SHARED-01 | Shared printer | 60 | 10.10.60.20 (shared printer) | SW-HOU-01 Fa0/9 |
| PC-HOU-SEC-01 | Security workstation | 40 | 10.10.40.11 (Security workstation) | SW-HOU-01 Fa0/10 |
| CAM-HOU-ENTRANCE-01 | Webcam with primary PT-IOT-NM-1CFE adapter | 80 | 10.10.80.11 (entrance camera) | PoE switch Fa0/3 |
| SRV-HOU-INV-01 | Existing DNS/HTTP server, now also IoT registration | 60 | 10.10.60.10 (inventory/IoT server) | Existing SW-HOU-01 Fa0/5 |

All new endpoint Ethernet links use copper straight-through cables. The switch uplink uses copper crossover. Phone addresses are DHCP leases and can change.

## Configuration and validation

R-HOU-01 remains the gateway and DHCP server. Its VLAN 40, 70 and 80 gateways are 10.10.40.1 (Security gateway), 10.10.70.1 (Voice gateway), and 10.10.80.1 (Cameras gateway). VLAN 80 crosses the PoE switch, SW-HOU-02 and SW-HOU-01 to the router. VLAN 40 is carried on the SW-HOU-01 router trunk.

The voice DHCP pool excludes the first twenty addresses and supplies option 150 pointing to 10.10.70.10 (call service router). The 2811 uses that address as its telephony source on port 2000. Reception MAC 000A.F365.CA3C maps to extension 7001, and Operations MAC 000A.4132.57B7 maps to extension 7002. The original 2911 rejected telephony-service in this simulator, so call service was placed on the 2811. An accidentally created DHCP pool on the voice router was removed as instructed; R-HOU-01 owns voice DHCP.

| Test | Observed result |
| --- | --- |
| Reception calls Operations | Operations shows From 7001 and Connected |
| Operations reaches shared printer | Repeat ping returns four replies, zero loss |
| Security reaches its gateway and printer | Four replies to each, zero loss |
| Security reaches entrance camera after adapter correction | Four replies, zero loss |
| Camera registers with IoT portal | Named device appears with green status |
| Security turns camera on remotely | Control becomes green and simulated image appears |

## Troubleshooting findings

- A duplicate voice VLAN assignment on Gi0/0.7 prevented assignment to Gi0/0.70. The stray subinterface was removed and the intended gateway configured.
- Voice DHCP initially offered reserved low addresses and logged conflicts. Adding the exclusion range, renewing the phones, and clearing supported DHCP conflict records produced valid leases and an empty conflict table. This Packet Tracer router did not support clear ip dhcp binding.
- The Security PC initially had 19.10.40.11 (mistyped workstation address). Correcting it to 10.10.40.11 (Security workstation) restored gateway and printer reachability.
- Camera connectivity initially passed, then failed during registration troubleshooting. Its switch port was up in VLAN 80 and the trunk forwarded VLAN 80, but the camera MAC was absent. Restarting the Ethernet interface did not fix it. I/O Config showed wireless as the primary adapter and Gigabit Ethernet as the secondary adapter. Replacing this with a single primary PT-IOT-NM-1CFE wired adapter, reconnecting and restoring static addressing restored reachability and enabled registration. This is an observed workaround, not proof of the simulator's internal failure mechanism.
- This version changed the camera registration button to Refresh after connection; the portal's device listing confirmed registration.

## Scope and remaining work

The phones demonstrate Ethernet power without attached external adapters. Camera PoE capability was not verified. The Webcam provides a simulated image and on/off control; no real video stream, recording retention or NVR was tested. Printer testing establishes IP reachability, not a completed print job. Camera and guest access restrictions remain for Sprint 5. No Houston–Brno WAN or VPN has been implemented here.

The user confirmed saving running configurations on the five modified routers/switches and saving the Packet Tracer checkpoint. A reopen/persistence test has not yet been captured. The lab runs locally with synthetic data at $0; displayed credentials are lab-only values.

## Evidence walkthrough

The figures preserve both unsuccessful intermediate tests and final successful validation. Earlier settings must not be treated as the final configuration.

### 01. Ip phone powered via ethernet

<img src="screenshots/01-ip-phone-powered-via-ethernet.png" alt="Ip phone powered via ethernet" width="100%">

Actual configuration or test capture supplied during this sprint.

### 02. Ip phone external power unattached

<img src="screenshots/02-ip-phone-external-power-unattached.png" alt="Ip phone external power unattached" width="100%">

Actual configuration or test capture supplied during this sprint.

### 03. Reception port voice vlan

<img src="screenshots/03-reception-port-voice-vlan.png" alt="Reception port voice vlan" width="100%">

Actual configuration or test capture supplied during this sprint.

### 04. Houston main switch voice trunks

<img src="screenshots/04-houston-main-switch-voice-trunks.png" alt="Houston main switch voice trunks" width="100%">

Actual configuration or test capture supplied during this sprint.

### 05. Houston second switch voice trunks

<img src="screenshots/05-houston-second-switch-voice-trunks.png" alt="Houston second switch voice trunks" width="100%">

Actual configuration or test capture supplied during this sprint.

### 06. Poe switch voice trunk

<img src="screenshots/06-poe-switch-voice-trunk.png" alt="Poe switch voice trunk" width="100%">

Actual configuration or test capture supplied during this sprint.

### 07. Duplicate voice subinterfaces

<img src="screenshots/07-duplicate-voice-subinterfaces.png" alt="Duplicate voice subinterfaces" width="100%">

Actual configuration or test capture supplied during this sprint.

### 08. Voice dhcp conflicts

<img src="screenshots/08-voice-dhcp-conflicts.png" alt="Voice dhcp conflicts" width="100%">

Intermediate DHCP conflict evidence, subsequently resolved.

### 09. Corrected voice dhcp pool

<img src="screenshots/09-corrected-voice-dhcp-pool.png" alt="Corrected voice dhcp pool" width="100%">

Actual configuration or test capture supplied during this sprint.

### 10. Voice lease and empty conflict table

<img src="screenshots/10-voice-lease-and-empty-conflict-table.png" alt="Voice lease and empty conflict table" width="100%">

A valid voice lease and an empty DHCP conflict table are shown.

### 11. Reception phone voice address

<img src="screenshots/11-reception-phone-voice-address.png" alt="Reception phone voice address" width="100%">

Actual configuration or test capture supplied during this sprint.

### 12. Operations phone voice address

<img src="screenshots/12-operations-phone-voice-address.png" alt="Operations phone voice address" width="100%">

Actual configuration or test capture supplied during this sprint.

### 13. Voice router to gateway ping

<img src="screenshots/13-voice-router-to-gateway-ping.png" alt="Voice router to gateway ping" width="100%">

Actual configuration or test capture supplied during this sprint.

### 14. 2811 call service and extension mapping

<img src="screenshots/14-2811-call-service-and-extension-mapping.png" alt="2811 call service and extension mapping" width="100%">

Actual configuration or test capture supplied during this sprint.

### 15. Reception registered extension 7001

<img src="screenshots/15-reception-registered-extension-7001.png" alt="Reception registered extension 7001" width="100%">

Actual configuration or test capture supplied during this sprint.

### 16. Operations registered extension 7002

<img src="screenshots/16-operations-registered-extension-7002.png" alt="Operations registered extension 7002" width="100%">

Actual configuration or test capture supplied during this sprint.

### 17. Reception to operations call connected

<img src="screenshots/17-reception-to-operations-call-connected.png" alt="Reception to operations call connected" width="100%">

The Operations phone displays an incoming call from 7001 and Connected.

### 18. Operations to shared printer ping

<img src="screenshots/18-operations-to-shared-printer-ping.png" alt="Operations to shared printer ping" width="100%">

The first printer ping has one timeout; the repeat test has zero loss.

### 19. Security workstation initial connectivity failure

<img src="screenshots/19-security-workstation-initial-connectivity-failure.png" alt="Security workstation initial connectivity failure" width="100%">

Actual configuration or test capture supplied during this sprint.

### 20. Security workstation ip address typo

<img src="screenshots/20-security-workstation-ip-address-typo.png" alt="Security workstation ip address typo" width="100%">

The workstation address contains a 19 instead of 10 in the first octet.

### 21. Security workstation gateway and printer ping success

<img src="screenshots/21-security-workstation-gateway-and-printer-ping-success.png" alt="Security workstation gateway and printer ping success" width="100%">

Actual configuration or test capture supplied during this sprint.

### 22. Office services topology before camera

<img src="screenshots/22-office-services-topology-before-camera.png" alt="Office services topology before camera" width="100%">

Actual configuration or test capture supplied during this sprint.

### 23. Camera available network interfaces

<img src="screenshots/23-camera-available-network-interfaces.png" alt="Camera available network interfaces" width="100%">

Actual configuration or test capture supplied during this sprint.

### 24. Security workstation to camera ping success

<img src="screenshots/24-security-workstation-to-camera-ping-success.png" alt="Security workstation to camera ping success" width="100%">

Initial camera reachability passed before the later registration connectivity failure.

### 25. Webcam specifications and registration support

<img src="screenshots/25-webcam-specifications-and-registration-support.png" alt="Webcam specifications and registration support" width="100%">

Actual configuration or test capture supplied during this sprint.

### 26. Iot registration service enabled

<img src="screenshots/26-iot-registration-service-enabled.png" alt="Iot registration service enabled" width="100%">

Actual configuration or test capture supplied during this sprint.

### 27. Iot portal login success empty device list

<img src="screenshots/27-iot-portal-login-success-empty-device-list.png" alt="Iot portal login success empty device list" width="100%">

Actual configuration or test capture supplied during this sprint.

### 28. Camera registration settings before connection

<img src="screenshots/28-camera-registration-settings-before-connection.png" alt="Camera registration settings before connection" width="100%">

Actual configuration or test capture supplied during this sprint.

### 29. Camera registration failure interface settings

<img src="screenshots/29-camera-registration-failure-interface-settings.png" alt="Camera registration failure interface settings" width="100%">

Wireless0 is selected in this intermediate Settings view.

### 30. Camera wired gateway set registration still fails

<img src="screenshots/30-camera-wired-gateway-set-registration-still-fails.png" alt="Camera wired gateway set registration still fails" width="100%">

Wired gateway settings were entered, but registration was still unsuccessful.

### 31. Registration server to camera ping failure

<img src="screenshots/31-registration-server-to-camera-ping-failure.png" alt="Registration server to camera ping failure" width="100%">

Actual configuration or test capture supplied during this sprint.

### 32. Server gateways and security workstation ping success

<img src="screenshots/32-server-gateways-and-security-workstation-ping-success.png" alt="Server gateways and security workstation ping success" width="100%">

Actual configuration or test capture supplied during this sprint.

### 33. Camera port up vlan80 no camera mac learned

<img src="screenshots/33-camera-port-up-vlan80-no-camera-mac-learned.png" alt="Camera port up vlan80 no camera mac learned" width="100%">

Fa0/3 is up in VLAN 80 and the uplink forwards VLAN 80; the camera MAC is not learned.

### 34. Camera ping fails after interface restart

<img src="screenshots/34-camera-ping-fails-after-interface-restart.png" alt="Camera ping fails after interface restart" width="100%">

Actual configuration or test capture supplied during this sprint.

### 35. Camera mac still absent after interface restart

<img src="screenshots/35-camera-mac-still-absent-after-interface-restart.png" alt="Camera mac still absent after interface restart" width="100%">

Actual configuration or test capture supplied during this sprint.

### 36. Camera wireless primary ethernet secondary adapters

<img src="screenshots/36-camera-wireless-primary-ethernet-secondary-adapters.png" alt="Camera wireless primary ethernet secondary adapters" width="100%">

Wireless is the primary adapter; wired Gigabit Ethernet is secondary.

### 37. Camera ping restored after primary wired adapter change

<img src="screenshots/37-camera-ping-restored-after-primary-wired-adapter-change.png" alt="Camera ping restored after primary wired adapter change" width="100%">

After configuring a single primary wired adapter, camera ping succeeds.

### 38. Camera registration button changed to refresh

<img src="screenshots/38-camera-registration-button-changed-to-refresh.png" alt="Camera registration button changed to refresh" width="100%">

The registration button changes to Refresh; the portal is checked next.

### 39. Camera registered green status in security portal

<img src="screenshots/39-camera-registered-green-status-in-security-portal.png" alt="Camera registered green status in security portal" width="100%">

The named camera appears in the portal with a green status indicator.

### 40. Camera remote controls before on test

<img src="screenshots/40-camera-remote-controls-before-on-test.png" alt="Camera remote controls before on test" width="100%">

The camera control is red and the simulated image panel is black before the on test.

### 41. Security workstation camera on and image visible

<img src="screenshots/41-security-workstation-camera-on-and-image-visible.png" alt="Security workstation camera on and image visible" width="100%">

Remote on control is green and the simulated image is visible.

### 42. Office services final topology

<img src="screenshots/42-office-services-final-topology.png" alt="Office services final topology" width="100%">

Final logical topology, including the new office services. Houston and Brno remain separate sites without a WAN link.

## Screenshot index

| Figure | Evidence |
| --- | --- |
| 01 | [Ip phone powered via ethernet](screenshots/01-ip-phone-powered-via-ethernet.png) |
| 02 | [Ip phone external power unattached](screenshots/02-ip-phone-external-power-unattached.png) |
| 03 | [Reception port voice vlan](screenshots/03-reception-port-voice-vlan.png) |
| 04 | [Houston main switch voice trunks](screenshots/04-houston-main-switch-voice-trunks.png) |
| 05 | [Houston second switch voice trunks](screenshots/05-houston-second-switch-voice-trunks.png) |
| 06 | [Poe switch voice trunk](screenshots/06-poe-switch-voice-trunk.png) |
| 07 | [Duplicate voice subinterfaces](screenshots/07-duplicate-voice-subinterfaces.png) |
| 08 | [Voice dhcp conflicts](screenshots/08-voice-dhcp-conflicts.png) |
| 09 | [Corrected voice dhcp pool](screenshots/09-corrected-voice-dhcp-pool.png) |
| 10 | [Voice lease and empty conflict table](screenshots/10-voice-lease-and-empty-conflict-table.png) |
| 11 | [Reception phone voice address](screenshots/11-reception-phone-voice-address.png) |
| 12 | [Operations phone voice address](screenshots/12-operations-phone-voice-address.png) |
| 13 | [Voice router to gateway ping](screenshots/13-voice-router-to-gateway-ping.png) |
| 14 | [2811 call service and extension mapping](screenshots/14-2811-call-service-and-extension-mapping.png) |
| 15 | [Reception registered extension 7001](screenshots/15-reception-registered-extension-7001.png) |
| 16 | [Operations registered extension 7002](screenshots/16-operations-registered-extension-7002.png) |
| 17 | [Reception to operations call connected](screenshots/17-reception-to-operations-call-connected.png) |
| 18 | [Operations to shared printer ping](screenshots/18-operations-to-shared-printer-ping.png) |
| 19 | [Security workstation initial connectivity failure](screenshots/19-security-workstation-initial-connectivity-failure.png) |
| 20 | [Security workstation ip address typo](screenshots/20-security-workstation-ip-address-typo.png) |
| 21 | [Security workstation gateway and printer ping success](screenshots/21-security-workstation-gateway-and-printer-ping-success.png) |
| 22 | [Office services topology before camera](screenshots/22-office-services-topology-before-camera.png) |
| 23 | [Camera available network interfaces](screenshots/23-camera-available-network-interfaces.png) |
| 24 | [Security workstation to camera ping success](screenshots/24-security-workstation-to-camera-ping-success.png) |
| 25 | [Webcam specifications and registration support](screenshots/25-webcam-specifications-and-registration-support.png) |
| 26 | [Iot registration service enabled](screenshots/26-iot-registration-service-enabled.png) |
| 27 | [Iot portal login success empty device list](screenshots/27-iot-portal-login-success-empty-device-list.png) |
| 28 | [Camera registration settings before connection](screenshots/28-camera-registration-settings-before-connection.png) |
| 29 | [Camera registration failure interface settings](screenshots/29-camera-registration-failure-interface-settings.png) |
| 30 | [Camera wired gateway set registration still fails](screenshots/30-camera-wired-gateway-set-registration-still-fails.png) |
| 31 | [Registration server to camera ping failure](screenshots/31-registration-server-to-camera-ping-failure.png) |
| 32 | [Server gateways and security workstation ping success](screenshots/32-server-gateways-and-security-workstation-ping-success.png) |
| 33 | [Camera port up vlan80 no camera mac learned](screenshots/33-camera-port-up-vlan80-no-camera-mac-learned.png) |
| 34 | [Camera ping fails after interface restart](screenshots/34-camera-ping-fails-after-interface-restart.png) |
| 35 | [Camera mac still absent after interface restart](screenshots/35-camera-mac-still-absent-after-interface-restart.png) |
| 36 | [Camera wireless primary ethernet secondary adapters](screenshots/36-camera-wireless-primary-ethernet-secondary-adapters.png) |
| 37 | [Camera ping restored after primary wired adapter change](screenshots/37-camera-ping-restored-after-primary-wired-adapter-change.png) |
| 38 | [Camera registration button changed to refresh](screenshots/38-camera-registration-button-changed-to-refresh.png) |
| 39 | [Camera registered green status in security portal](screenshots/39-camera-registered-green-status-in-security-portal.png) |
| 40 | [Camera remote controls before on test](screenshots/40-camera-remote-controls-before-on-test.png) |
| 41 | [Security workstation camera on and image visible](screenshots/41-security-workstation-camera-on-and-image-visible.png) |
| 42 | [Office services final topology](screenshots/42-office-services-final-topology.png) |
