# DualFlow KVM — User Guide

For Windows 11 · Version 1.11.0

Control two PCs with one keyboard and mouse. **PC 1 is the Host**, where your
keyboard and mouse are connected. **PC 2 is the Client**, which you control over
the local network. Both PCs keep using their own connected monitors.

## Before you start

- Connect both PCs to the same trusted local network. Guest-network isolation
  must not prevent them from communicating.
- Use the **Private** Windows network profile on that trusted network.
- Have administrator permission and a temporary mouse/keyboard available on
  Client for the initial setup.
- Use the v1.11.0 setup EXE on both PCs, including when upgrading from v1.10.x. No developer tools
  or manual firewall scripts are needed.

The pairing code must be entered once before DualFlow's shared clipboard can
work. Use a private method to carry the code from Host to Client.

## 1. Set up PC 1 — Host

1. Run `DualFlow-KVM-v1.11.0-setup.exe` and approve the normal Windows administrator
   prompt if you trust the installer. Complete setup, then open DualFlow KVM.
2. Open **Settings**. Under **Computer role**, select **Host**.
3. Leave **Network mode → Mode** set to **Auto discovery**. Keep the default ports
   unless you need a custom network configuration.
4. Under **Secure PC pairing**, select **Show pairing code**. Copy the complete
   code for use on PC 2. Treat it as a secret: it authorizes control of the pair.
5. Leave DualFlow running while you set up PC 2.

You can choose the role in Settings even if the initial **Set this PC as Host**
button is unavailable while discovery is still running.

## 2. Set up PC 2 — Client

1. Install the same setup EXE on PC 2 and open DualFlow KVM.
2. Open **Settings → Computer role** and select **Client**.
3. Leave the network mode on **Auto discovery**, using the same ports as Host.
4. Under **Secure PC pairing**, paste the complete Host code into **Pairing code**.
   Select **Save pairing code**.
5. Wait for the top-right status to change to **Linked**. After pairing, check
   **Administrator prompts** in Settings: the installed input service should be
   connected. Follow any error shown there before relying on UAC control.

Only one PC should be Host. Client receives the display arrangement and pointer
preferences from Host; do not try to configure the screen layout on Client.

## Trial and lifetime activation — Host only

Your 10-day trial starts when you first select **Host**. The status below the
header shows **Trial: X days left**. The trial uses UTC calendar days and cannot
be restarted by reinstalling. Keep the Windows clock correct: moving it backwards
expires the trial.

1. On Host, click the trial status to open the license dialog.
2. To purchase, click **Buy Lifetime License (€29.95)**. Your default browser opens
   the DualFlow store. Complete the purchase there and obtain your license key.
3. Enter the key in DualFlow on Host and click **Activate**. Host needs internet
   access for this step. Its Windows machine identifier and the key are sent to
   Lemon Squeezy; activation is stored encrypted on this Host.
4. Check for **Lifetime License Activated**. Client should show **Connected to
   Licensed Host** when linked. During the trial it shows **Connected (Host on Trial)**.

Do not activate on Client. Client needs neither a license key nor internet for
licensing. A saved lifetime activation works offline. One license allows one
Host activation; contact the seller to release/reset it before moving to another
Host or after replacing Windows/machine identity.

If the trial expires, remote input and shared clipboard/file transfers stop,
and local input is released. Settings, pairing diagnostics and updates remain
available. Activate on Host to restore the bridge. If activation is interrupted
after contacting the server, do not repeatedly activate: ask the seller to check
the existing instance if the next attempt reports that the limit is reached.

## Software updates — Both PCs

DualFlow checks for signed updates in the background when it starts. This does
not install anything automatically or require an active license.

1. Open **Show Settings** from the tray. The update notice is near the bottom.
2. Click **Check for updates**, or **Update now** when a new version is shown.
3. Save your work and allow the brief KVM disconnection. DualFlow verifies the
   download, runs its installer and restarts silently in the tray.
4. Repeat on the other PC and check the visible version on both PCs.

Update each PC locally if the previous update has left them on incompatible
protocol versions. Keep temporary local input available. Internet is required
for downloading; a failed check or rejected signature does not expire a license
or stop the existing KVM connection. For v1.10.x or earlier, first install v1.11.0
manually on both PCs; older builds do not contain this update engine.

## 3. Return to PC 1 — Arrange and test

1. Open **Displays** on Host. Identify each display by its PC name and resolution.
2. Drag each display from its top-left cell to match the physical arrangement.
   Place screens directly beside, above or below each other as appropriate.
3. Make the intended crossing edges touch. An empty cell or a corner-only contact
   does not create a crossing. Cards cannot overlap or extend outside the grid.
4. Standard 16:9 displays use one tile regardless of resolution. A 32:9 display
   spans two columns; a triple-surround display spans three. Move the entire card
   as one piece. For a Client screen above the middle of a surround display, place
   it directly above the middle occupied cell.
5. Changes save automatically and appear on Client. There is no separate layout
   Save button.
6. Open **Settings → Connection diagnostics → Run connection test** on Host.
   Follow the **Suggested solution** for any failed check.
7. Move the mouse through a shared edge, click a text field on Client and type.
   Return to Host and check that the pointer enters the expected screen segment.

Once setup works, test with the temporary Client mouse unplugged. Confirm that
the pointer remains visible on the desktop, Start and Search. Also test an
administrator window and a UAC prompt before relying on remote input there.

## Daily use

### Switch between PCs

Move the pointer to the physical edge of a screen that borders the other PC in
the layout, then continue moving outward. The keyboard follows the pointer.
At the bottom of a Client screen, the edge guard keeps the taskbar reachable;
continue outward from the actual edge to return to Host.

If you ever need local control immediately, press **Pause** or **Ctrl+Alt+F12**
on the **Host keyboard**. Pause is the simpler option if available. Keep local
input available as a recovery method if neither shortcut responds.

### Use the system tray

Right-click the DualFlow icon in the Windows notification area, near the clock:

- **Show Settings** opens the app window.
- **Quit** stops the app and ends the connection.

Closing the window does not quit. After installation, DualFlow is configured to
start silently when you sign in to Windows, using your saved settings. A manual
launch can display the normal administrator prompt.

### Copy text, images, files and folders

For text and supported clipboard images, copy on one PC, switch to the other and
paste into an application that accepts the content. Sharing works both ways.

For files and folders:

1. First open the destination folder in File Explorer on the receiving PC. Click
   inside it and leave the pointer over that window before returning to the sender.
2. On the sending PC, select the files or folders in Explorer and press **Ctrl+C**.
   Copying starts the network transfer automatically.
3. Keep both PCs connected. When transfer completes, DualFlow places the received
   selection on the receiving clipboard and requests a paste at its cursor target.
4. Check the destination. If nothing was pasted, select the intended destination
   folder and press **Ctrl+V** after the transfer finishes. Do not paste again if
   the files have already appeared.

Nested files and empty folders are preserved. Use **Copy**, not Cut, and verify
the received files before deleting any originals. Completed transfers use temporary
disk space on the receiving PC as well as space for the pasted copy.

Dragging an Explorer selection across a screen edge is **not supported**. Dropping
onto the DualFlow window is an alternative only when Windows permits the drop.
Symbolic links, junctions and other reparse points, including some cloud-backed
files, are not supported; use ordinary local files. Folder hierarchy and contents
are copied, not permissions, timestamps or other filesystem metadata.

Clipboard sharing is automatic while linked. Quit DualFlow before copying content
you do not want shared with the other PC.

## Optional settings

### Pointer speed and size — Host only

Move the pointer back to Host before changing these settings.

- Under **Cursor sensitivity**, keep **Auto-sync sensitivity (DPI match)** enabled
  for resolution-based scaling. For manual control, turn it off, adjust **Client
  Speed Multiplier** from 0.50× to 2.50× and select **Save sensitivity**.
- Under **Cursor size**, choose **Cursor Size** level 1–5. Leave **Sync cursor size
  to Client** checked to apply it to both PCs, then select **Save pointer settings**.

To keep native cursor visibility available without a Client mouse, DualFlow uses
temporary Windows Mouse Keys settings. Keep **Num Lock on** for normal numeric
keypad entry; with it off, Mouse Keys may use the keypad for pointer navigation.

### Fixed IP or custom ports — Both PCs

Use Auto discovery unless your network requires manual addressing.

1. On each PC, open **Settings → Network mode** and select **Manual / fixed IP**.
2. In **Peer IPv4 address**, enter the **other PC's** address, not its own.
3. Use matching port values on both PCs: **Discovery UDP** defaults to `45831`
   and **Input TCP** defaults to `45832`.
4. Select **Save network settings**, then **Restart DualFlow** when prompted.
5. Repeat on the other PC, then run the connection test from Host.

Manual mode does not configure a static address in Windows or bypass network
isolation. The chosen peer addresses must remain valid and reachable. Pairing is
still required. Do not expose these ports to the internet.

### Change roles

Select **Host** or **Client** under **Settings → Computer role** on each PC.
Keep exactly one Host, with the physical keyboard and mouse connected to it.
Role changes interrupt control; have local input available and recheck pairing
and the display layout afterward.

## Updates and display sleep

The running version appears in the app footer and at the top of Settings.
For updates, have local input available on the PC being updated, use **Quit** in
the tray, then run the newer setup EXE. Do not uninstall or delete settings first.
An existing 1.10.7 Host can remain paired with a 1.10.8 Client; this particular
update does not require a Host reinstall.

Entering Client requests a display/screensaver wake-up. This is not Wake-on-LAN:
a fully sleeping, hibernating or powered-off PC must be woken separately. Password
protection still requires normal Windows sign-in. UAC still requires your explicit
Yes/No choice and a working installed input service.

## Troubleshooting

Start with **Run connection test** in Host Settings, then use the relevant check:

| Symptom | What to check |
| --- | --- |
| Discovering / Waiting for link | Both apps are running, one Host and one Client, same pairing code, matching ports, trusted Private network, no guest isolation. For Manual mode, verify the peer addresses. |
| Linked, but the pointer will not cross | Configure Displays on Host. The screens must share an occupied grid edge. Check for an input-hook error and run the connection test. |
| Input stops or the pointer is missing | Use Pause or Ctrl+Alt+F12 on Host. Check Client's version and any displayed error. Restart DualFlow normally; retain local input for recovery. |
| An administrator window or UAC prompt cannot be controlled | Check Administrator prompts in Client Settings. Use the installed setup version, not just a copied application EXE. Do not disable UAC or Secure Desktop. |
| A file or folder did not arrive | Allow the transfer to finish, check disk space and any transfer error, then paste into Explorer. Use ordinary local files rather than links or cloud placeholders. |
| The settings window disappears | Closing it hides it to the tray. Choose Show Settings from the tray menu. |

If you need to report a problem, include versions on **both** PCs, their roles,
the connection-test result, the exact error and the steps that reproduce it.
For setup or input-service issues, the default installation folder contains:

```text
C:\Program Files\DualFlow KVM\input-service-install.log
C:\Program Files\DualFlow KVM\helper-panic.log
```

If you installed elsewhere, use that folder instead. Do not post pairing codes
or your configuration file publicly. Windows-managed security policies and some
protected desktops may prevent remote input; keep a local recovery method.

[Back to README](../README.md)
