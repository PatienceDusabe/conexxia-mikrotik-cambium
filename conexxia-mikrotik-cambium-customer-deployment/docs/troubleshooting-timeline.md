# Troubleshooting Timeline

| Symptom | Finding | Action / result |
|---|---|---|
| MikroTik not shown in WinBox Neighbors | Windows had no active Ethernet interface; USB 10/100 LAN device showed **Code 45** | Replaced faulty adapter; WinBox discovery succeeded |
| WAN port could not be removed individually from bridge | `ether1` appeared as a dynamic member associated with `all` bridge-port rule | Replaced broad membership with explicit LAN ports; WAN left outside bridge |
| Antenna repeatedly switched from Registering to Scanning | AP scanning showed strong RF metrics; changes had not been saved | Applied/saved configuration; continued to PPPoE stage |
| PPPoE / internet initially unconfirmed | Antenna was not yet connected, so initial router pings failed | After connecting, PPPoE displayed `R`; routing and router ping verified |
| `google.com` ping timed out | DNS successfully resolved the name, and `8.8.8.8` responded | Distinguished ICMP timeout from DNS failure; wired browser opened Google |
| Wi-Fi phone connected but had no useful LAN address | Phone self-assigned `169.254.206.152`; `wlan1` missing from `bridge-lan` | Added `wlan1` to bridge; phone retest not captured in conversation |

## Practices demonstrated

- Troubleshoot layers in order: cable/adapter → Ethernet link → bridge/VLAN topology → DHCP → PPPoE/routing → DNS/NAT → client browsing.
- Avoid changing operational AP security or resetting equipment without a verified reason.
- Check actual interface and registration status rather than assuming configuration equals service availability.
