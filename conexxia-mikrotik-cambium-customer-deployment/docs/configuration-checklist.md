# Sanitized Configuration Checklist

This is an **illustrative** reference based on the deployment conversation, not an exported production configuration. Inspect first; do not duplicate existing settings.

## Interfaces

| Router interface | Function |
|---|---|
| `ether1` | WAN connection to antenna, PPPoE client |
| `ether2` to `ether5` | Customer LAN ports |
| `wlan1` | Wi-Fi AP, joined to the LAN bridge |
| `bridge-lan` | Common Layer 2 network for wired + Wi-Fi clients |

## RouterOS inspection (read-only)

```routeros
/interface bridge print
/interface bridge port print
/ip address print
/ip dhcp-server print
/ip dhcp-server network print
/interface pppoe-client print
/ip route print
/ip firewall nat print
/ip firewall filter print
```

## Illustrative LAN settings

```text
Bridge: bridge-lan
LAN IP / gateway: 192.168.88.1/24
DHCP pool: 192.168.88.10-192.168.88.254
DHCP network: 192.168.88.0/24
Wireless: wlan1 configured as AP, WPA2-PSK/AES or better if supported
```

## Illustrative PPPoE client settings

```text
Name: pppoe-isp
Interface: ether1
Username: <PPPOE_USERNAME>
Password: <PPPOE_PASSWORD>
Add Default Route: Yes
Use Peer DNS: Yes, if approved by ISP
```

> The name `pppoe-isp` is a placeholder. The actual deployment used a named PPPoE interface and live credentials that are not published.

## Illustrative NAT rule

```routeros
/ip firewall nat add chain=srcnat src-address=192.168.88.0/24 out-interface=pppoe-isp action=masquerade
```

Check for an existing equivalent NAT rule before adding one. Production security also requires appropriate WAN **input and forward firewall rules**; their implementation was not verified in the recorded conversation. Ensure remote management is restricted.

## Cambium subscriber antenna

- Confirm authorized AP and suitable RSSI/SNR, mode, bandwidth, firmware and security compatibility.
- Configure required wireless key **distinct from AP management login and PPPoE credentials**.
- Save/apply changes, then verify registration state.
- Confirm bridge/Ethernet handoff aligns with the ISP's PPPoE service model.
