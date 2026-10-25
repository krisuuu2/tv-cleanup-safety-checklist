# Cleaned your TV with Claude? Close the door behind you.

A short safety checklist for anyone who used **[CleanYourTV](https://github.com/Gathin23/cleanyourtv)** (or any ADB debloat) on an Android TV or Google TV. Every step comes from the project's own README.

## Why this matters

Cleaning the TV needs **USB / network debugging (ADB)** switched on. While it stays on, a computer can control your TV over the network. Network ADB runs **unencrypted on port 5555**, and on some TV firmware (`ro.adb.secure=0`) **any device on your Wi-Fi gets a shell on the TV**. The launcher keeps working without debugging, so you lose nothing by closing it.

## The checklist (after the cleanup is done)

1. **Switch debugging off on the TV.** Settings → Device Preferences → Developer options → turn off **USB debugging**, **Network/ADB debugging** and **Wireless debugging**, whichever your TV has.
2. **Revoke the USB debugging authorisations** in the same menu.
3. **Note the Wireless debugging port first.** It uses its own port and can stay open when USB debugging is off (seen on a Chromecast with Google TV 4K). `adb mdns services` lists it as `_adb-tls-connect._tcp`.
4. **Restart the TV.**
5. **Check from your computer that the port is closed:** `python3 tools/adb-closed.py <tv ip>` (in the CleanYourTV repo) must say *closed*. With `nc -vz <tv ip> 5555`, only "Connection refused" counts.
6. **Never open the ADB port on your router.** To help someone in another home, use a private link such as Tailscale (see the project's `docs/REMOTE.md`).
7. **Keep your debloat list.** After a firmware update a package can come back; re-run the script, and `tools/restore.sh` still puts the TV back to stock.

## Good to know

- Nothing is uninstalled: packages are only disabled, so `tools/restore.sh` undoes every step, and the TV's own Settings → Apps can re-enable them too.
- Works on Android TV and Google TV (TCL, Sony, Xiaomi, Philips and many more). Samsung (Tizen) and LG (webOS) are different systems.
- Streaming apps keep updating through the Play Store, which is left alone.

Source: [Gathin23/cleanyourtv README](https://github.com/Gathin23/cleanyourtv) (FAQ "Isn't ADB access a security hole?", "Is this safe?", "Restoring your TV").

— @aipractically
