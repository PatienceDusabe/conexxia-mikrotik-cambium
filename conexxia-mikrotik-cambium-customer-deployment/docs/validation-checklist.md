# Customer Handover Validation Checklist

Check each item during a future site visit or when additional evidence is available.

- [x] Router visible through WinBox on a functioning Ethernet interface.
- [x] WAN `ether1` outside the customer LAN bridge.
- [x] Local bridge configured with wired Ethernet members.
- [x] PPPoE client shows `Running`.
- [x] Active default route through PPPoE.
- [x] Router ping `8.8.8.8` returns replies.
- [x] Wired laptop successfully opens a public website.
- [x] Missing `wlan1` bridge membership identified and corrected.
- [ ] Phone receives `192.168.88.x` after Wi-Fi bridge fix (pending recorded verification).
- [ ] Phone successfully browses a public website over Wi-Fi (pending recorded verification).
- [ ] Verify firewall input and forwarding policies protect customer LAN and router.
- [ ] Verify the administrator password change with a fresh login.
- [ ] Save a sanitized configuration summary and complete customer handover.

Do not tick items without observing the outcome.
