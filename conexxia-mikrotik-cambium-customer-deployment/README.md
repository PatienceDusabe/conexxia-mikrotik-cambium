# Real-World ISP Customer Connectivity Deployment

**Technology:** Cambium subscriber antenna (SM/CPE) · MikroTik RB951Ui-2HnD · RouterOS/WinBox · PPPoE · DHCP · DNS · NAT · Wi-Fi

**Type:** Professional field deployment/troubleshooting (not a lab)  
**Period:** October 2026  
**Role:** ISP technical support / customer connectivity configuration

> **Privacy:** All PPPoE credentials, wireless keys, customer names, AP identifiers, device MACs, public addresses and management access details are intentionally excluded or anonymized. This repository is a learning/portfolio case study, not a production configuration backup.

## Objective

Provide a customer with internet over **Ethernet and Wi-Fi**, using an authorized Cambium subscriber antenna as the wireless last-mile connection and a MikroTik router for customer LAN services. No VLAN segmentation was required.

## Architecture

```text
ISP core / PPPoE service
       |
Cambium access point
       ))  wireless last-mile link
Cambium subscriber antenna (bridge mode / Ethernet handoff)
       |
PoE injector LAN output
       |
MikroTik ether1 (PPPoE client, WAN)
       |
MikroTik LAN bridge (192.168.88.1/24)
       +---- ether2-ether5 --> wired devices
       +---- wlan1 --------> Wi-Fi devices
```

The AP name and frequency are deliberately not published. The network design assumes the antenna provides an appropriate bridged PPPoE handoff, as configured at the site.

## Work performed

1. **Regained management access:** diagnosed a laptop USB-to-Ethernet adapter failure. Windows reported device **Code 45** (not connected). After replacing the faulty adapter, the MikroTik became discoverable in WinBox.
2. **Separated WAN and LAN:** inspected existing bridge membership. A dynamic `all` bridge-port entry had incorporated all ports, including the intended WAN. Replaced it with explicit LAN members `ether2`–`ether5` and `wlan1`, keeping `ether1` outside the LAN bridge.
3. **Configured local networking:** set the LAN gateway to `192.168.88.1/24`, DHCP server and address pool, and DNS behavior for client devices.
4. **Configured internet access:** set up a PPPoE client on `ether1`, default route, and a source NAT `masquerade` rule for the LAN over the PPPoE interface.
5. **Worked on last-mile registration:** scanned nearby Cambium APs, identified the authorized target by site selection and RF information, configured the necessary wireless security, and diagnosed repeated scanning. A missed **Save Changes** action prevented the settings from being applied.
6. **Diagnosed client connectivity:** verified that PPPoE was running, a default route was active, and `8.8.8.8` responded from the router. The wired laptop loaded Google's homepage. The phone initially received `169.254.206.152` (self-assigned address), indicating DHCP failure on Wi-Fi; discovered that `wlan1` was missing from the bridge and added it.
7. **Improved access security:** planned/configured an administrator password change through WinBox; verification of the completed password change is not captured in the notes.

## Verification evidence

| Check | Observed result |
|---|---|
| WinBox discovery after USB adapter replacement | **Passed** |
| MikroTik PPPoE interface | **Running (`R`)** |
| Default route via PPPoE | **Active** |
| Router ping to `8.8.8.8` | **Passed** |
| Router DNS lookup for `google.com` | **Resolved** (ICMP reply timeout; not a DNS-resolution failure) |
| Wired laptop internet browsing | **Passed** — Google page opened |
| Phone DHCP before bridge correction | **Failed** — `169.254.206.152` |
| Wi-Fi bridge correction | **Applied** — `wlan1` added to LAN bridge |
| Phone internet browsing after correction | **Not independently confirmed in recorded evidence** |

## Lessons learned

- Start with Layer 1: bad USB Ethernet adapters can look like router or discovery faults.
- A dynamic bridge membership rule can unintentionally include the WAN.
- Wireless association and successful Internet access are separate stages.
- Settings must be **saved/applied** on the subscriber antenna; otherwise registration attempts may fall back to scanning.
- `169.254.x.x` indicates the client did not acquire a normal DHCP lease.
- Test the router first, then wired clients, then wireless clients.

## Repository contents

- [`docs/configuration-checklist.md`](docs/configuration-checklist.md): sanitized technical checklist and example RouterOS commands.
- [`docs/troubleshooting-timeline.md`](docs/troubleshooting-timeline.md): incident-to-resolution workflow and evidence limits.
- [`docs/validation-checklist.md`](docs/validation-checklist.md): repeatable pre-handover tests.

## Responsible disclosure

No live customer configurations, ISP credentials, router passwords, internal AP identifiers, or unredacted screenshots are included. Commands are examples to explain the deployment; **do not paste them into an existing production router without checking its current configuration and firewall**.
