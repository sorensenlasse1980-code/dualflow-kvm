# DualFlow KVM — User Guide

**For Windows 11 x64 · Version 1.13.0**

Control two PCs with one keyboard and mouse.

- **PC 1 — Host:** The PC with your main physical keyboard and mouse.
- **PC 2 — Client:** The second PC you control over your local network.

Both PCs use their own monitors and graphics hardware. DualFlow shares input, clipboard content, and files — not video or audio streams.

[Back to README](README.md)

---

## Before You Start

- Connect both PCs to a trusted, reachable local network. Avoid guest networks with device isolation.
- Have administrator permission available for installation.
- Keep a physical keyboard and mouse available on Client for initial setup and recovery.
- Use **`DualFlow-KVM-v1.13.0-setup.exe`** on both PCs for a new installation.
- Ensure Microsoft Edge WebView2 Runtime is available.
- Save work and finish active transfers before installing or updating.

Download the installer from the [official releases page](https://github.com/sorensenlasse1980-code/dualflow-kvm/releases).

Already using DualFlow? See **Updating DualFlow** below. You normally do not need to uninstall first.

## 1. Set Up PC 1 — Host

1. Run `DualFlow-KVM-v1.13.0-setup.exe`.
2. Complete setup and approve the normal Windows administrator prompt.
3. Review the optional permission for remote Ctrl+Alt+Del.
4. Launch DualFlow and open **Settings**.
5. Under **Computer role**, select **Host**.
6. Leave **Network mode → Mode** on **Auto discovery** unless you need manual addressing.
7. Under **Secure PC pairing**, select **Show pairing code**.
8. Copy the complete code for use on Client.
9. Keep DualFlow running while you prepare PC 2.

The pairing code authorizes the connection. Keep it private and only share it with the intended Client.

If you have purchased a lifetime license, activate it on Host only.

## 2. Set Up PC 2 — Client

1. Install the same version using `DualFlow-KVM-v1.13.0-setup.exe`.
2. Launch DualFlow and open **Settings**.
3. Under **Computer role**, select **Client**.
4. Leave the network mode on **Auto discovery**, unless manual addressing is needed.
5. Under **Secure PC pairing**, paste the complete Host code.
6. Select **Save pairing code**.
7. Wait until both PCs show **Linked**.
8. Open **Diagnostics & Test → Link & input service**.
9. Check that the installed Windows input service is connected. Use **Refresh service status** if needed.

Return to Host to arrange your displays.

> Use one Host and one Client. Display layout and pointer preferences are configured on Host and synchronized to Client.

## 3. Arrange and Test Displays

On **Host**:

1. Open **Displays**.
2. Identify the monitor cards by PC name and resolution.
3. Drag each monitor by its top-left cell to match your physical arrangement.
4. Make the intended crossing edges touch.
5. Move the pointer to the actual shared edge and continue outward.
6. Click a text field on Client and test typing.
7. Return to Host and test normal clicking and typing.

The layout uses a **6 × 4 grid**:

- Standard landscape displays generally occupy one tile.
- Wider displays can span several tiles.
- Only adjoining occupied edges create crossings.
- Empty cells and diagonal corners do not connect screens.
- Client cannot rearrange the layout independently.

Layout changes save and synchronize automatically.

### Taskbar and Edge Behavior

Reach the actual screen boundary and deliberately move outward to cross. Moving within the taskbar area alone should not switch PCs.

### Emergency Return

Press either shortcut on the physical Host keyboard:

- **Pause**
- **Ctrl+Alt+F12**

If control does not recover, use the physical reserve keyboard and mouse.

### Connecting or Disconnecting Monitors

DualFlow refreshes monitor inventory and pointer mapping when displays are disconnected, reconnected, or have their resolution/scaling changed.

Review **Displays** after a hardware change. Saved positions are restored where Windows display identities remain available. If Windows identifies a monitor differently, adjust its position on Host.

---

## 4. Everyday Mouse and Keyboard Use

Move across a configured shared edge to switch computers. Keyboard input follows the active PC.

Normal Windows focus still applies: click the intended application or text field before typing.

Right Shift and NumLock functionality are supported. However, the physical keyboard’s NumLock light is not synchronized with Client’s state.

Both PCs must remain awake and network-reachable.

## 5. Copy and Paste

### Text and Images

1. Copy on the source PC using **Ctrl+C** or the application’s Copy action.
2. Move to the receiving PC.
3. Click the intended destination.
4. Paste using **Ctrl+V** or the application’s Paste action.

### Files and Folders

1. Select local files or folders on Desktop or in File Explorer.
2. Use **Copy** or **Ctrl+C**.
3. Move to the receiving PC.
4. Open the destination folder, or click an empty area of its Desktop.
5. Use **Paste** or **Ctrl+V**.
6. Wait for Windows to finish copying.

Files are fetched **on demand**. Copying a file does not immediately transfer its entire contents.

Nested folders, empty files, and empty directories are supported. Keep both PCs connected and ensure sufficient space for temporary staging and the final copy.

Use **Copy**, not Cut, for cross-PC transfers. Remote source deletion through Cut/Paste is not implemented.

Cloud-only files may need to be downloaded locally first. Symbolic links, junctions, and other reparse points are not ordinary supported transfer items.

## 6. Drag and Drop Files or Folders

Native cross-PC Drag & Drop is available in **both directions**:

- Host → Client.
- Client → Host.

Supported sources and destinations are **Windows Desktop and File Explorer**, including folders inside Explorer.

### How to Transfer

1. Open the intended destination folder or Explorer window on the receiving PC.
2. Return to the source PC.
3. Click and hold a file or folder with the left mouse button.
4. Keep the button held while moving across the configured shared screen edge.
5. Move over the intended Desktop, Explorer window, or folder.
6. Release the button.
7. Wait for the copy to finish and verify the destination.

Empty files, empty folders, and nested folder structures are supported.

**Drag & Drop copies the content. The original remains on the source PC.**

Do not hold Shift expecting a move operation. Cross-PC MOVE and Shift+Drag source deletion are not implemented.

### Cancel a Drag

Press **Esc** while the drag is active.

If an operation fails or the network disconnects, check the destination before retrying. Do not assume a partially attempted transfer completed.

Arbitrary third-party application drop targets are outside the verified scope; use Desktop or File Explorer.

---

## 7. Windows Sign-In, Lock Screens, and Administrator Prompts

The installed Windows input service supports configured, paired machines:

- Before the first user login after startup.
- At Windows lock and password screens.
- During supported UAC prompts and elevated-application interactions.

### Prepare Once

1. Complete installation and pairing while signed in.
2. Confirm normal mouse and keyboard operation.
3. Check the Client input-service status under **Diagnostics & Test**.
4. Keep physical reserve input available.

### Use Client at the Lock Screen

1. Move the Host pointer onto Client.
2. Click or interact with the lock-screen image to display the sign-in field.
3. Enter the password or PIN normally using the Host keyboard.
4. Continue after Windows signs in.

DualFlow does not bypass passwords, automatically approve UAC, or remove Windows authentication requirements.

A sleeping or powered-off PC must be awake and network-reachable. DualFlow does not provide Wake-on-LAN.

## 8. Send Ctrl+Alt+Del to Client

Use either method:

### Keyboard Shortcut

While controlling Client, press:

**Ctrl+Alt+End**

### In-App Action

On Host:

1. Open **How to Use**.
2. Find the remote Ctrl+Alt+Del section.
3. Select **Send Ctrl+Alt+Del to Client**.

Physical **Ctrl+Alt+Del** on the Host keyboard remains local to Host.

Remote secure attention requires explicit setup permission for the supported service-only Windows SAS policy. Managed Windows policy can restrict it.

Windows does not return confirmation that the security screen appeared. A “requested” status means the request was issued, not that the screen’s appearance was verified.

If the request is denied, follow the displayed message. Do not disable UAC, Secure Desktop, or other security protections.

---

## 9. Updating DualFlow

### Update Both PCs from Host

Both installations must support coordinated updates. This workflow was introduced in **v1.11.8**.

1. Save work on both PCs.
2. Finish file transfers and drag operations.
3. Make sure both PCs are awake, paired, and **Linked**.
4. Ensure both PCs have internet access.
5. On Host, open **Settings** and select **Check for updates**.
6. When an update is available, select **Update both PCs**.
7. Select **Confirm update both PCs** once.
8. Wait while Client downloads, verifies, installs, relaunches, and reconnects.
9. Host verifies Client’s authenticated identity, expected version and role, and required readiness.
10. Host then downloads and installs its own update.

Each PC downloads and verifies its own package. Host does not send installer binaries to Client.

Do not start another installer, change roles, or interrupt file replacement during the update.

Shared control may be temporarily unavailable while applications and services restart.

### After the Update

DualFlow opens its existing non-modal confirmation showing the version actually running, for example:

**Updated to v1.13.0**

Check the version on both PCs and confirm **Linked** has returned.

### If Client’s Update Fails

Host remains on its existing version if Client does not pass the required verification.

Follow the displayed error instead of repeatedly starting competing updates. A restored network connection alone does not prove that Client updated successfully.

### Manual Upgrade

For older installations or recovery:

1. Save work and finish transfers.
2. Install the verified package on **Client first**.
3. Confirm Client launches normally.
4. Install the same version on **Host**.
5. Confirm both versions, **Linked**, and normal operation.

A preliminary uninstall is not normally required. Do not delete configuration or manually replace running executable files.

---

## 10. Trial and Lifetime Licensing

Licensing is managed on **Host only**.

- A 10-day trial is available for evaluation.
- Client does not require a separate license-key entry.
- One lifetime license covers one Host and its paired Client, subject to the product terms.

### Activate a License

1. On Host, click the license/trial status indicator.
2. Use **Buy Lifetime License** if you need to purchase a key.
3. Enter the purchased key.
4. Select **Activate**.
5. Confirm **Lifetime License Activated** appears.

Internet access is required for activation. Normal paired input and file-transfer traffic uses the LAN.

If the trial expires, activate on Host. Settings and updates remain available.

## 11. Optional Settings

### Pointer Sensitivity — Host

Open **Settings → Cursor sensitivity**.

- **Auto-sync sensitivity (DPI match)** adjusts movement using the Host/Client resolution ratio.
- To use a manual speed, turn auto-sync off.
- Adjust **Client Speed Multiplier**.
- Select **Save sensitivity**.

The manual multiplier is unavailable while auto-sync is enabled.

### Cursor Size — Host

Open **Settings → Cursor size**.

1. Choose a **Cursor Size** level.
2. Enable **Sync cursor size to Client** if desired.
3. Select **Save pointer settings**.

These preferences are controlled from Host.

### Manual IP Address and Ports

On each PC:

1. Open **Settings → Network mode**.
2. Select **Manual / fixed IP**.
3. Enter the **other PC’s IPv4 address**.
4. Use matching **Discovery UDP** and **Input TCP** ports.
5. Select **Save network settings**.
6. Use **Restart DualFlow** if a restart is requested.

This selects the address DualFlow connects to. It does not assign a static IP address in Windows.

---

## 12. Diagnostics and Benchmarks

Open **Diagnostics & Test**.

### Connection Test

On Host, select **Run connection test**.

Review each result and its suggested solution. A green connection test confirms the checks performed; it does not prove every application, file operation, or Windows security screen works.

### Input-Service Status

On Client, review **Link & input service**.

Select **Refresh service status** if needed. If unavailable, use the installed application and verified setup package rather than a copied standalone executable.

### Benchmark

1. Select a reference rate appropriate to the test.
2. Select **Start Benchmark**.
3. Use DualFlow normally.
4. Select **Stop Benchmark**.

Sessions stop automatically after two minutes.

For mouse-arrival measurements, run the benchmark on **Client** and move the Host mouse over Client.

Reports include application RTT, local Rust-process memory, and mouse-arrival measurements where available.

Important distinctions:

- RTT is round-trip application timing, not synchronized one-way input latency.
- Capture-to-inject latency is unavailable.
- Memory excludes WebView2 and the separate input service.
- Benchmark sampling is inactive outside the test.

Stopping automatically copies the Markdown report and saves a timestamped file under:

```text
Documents\DualFlow KVM\Benchmarks
```

**Copy Results & Save** copies and saves another report.

> Copying a benchmark report replaces the current clipboard contents. Finish any pending clipboard transfer first.

## 13. Tray, Startup, and Quit

Closing the main window leaves DualFlow running in the notification area.

- **Show Settings:** Right-click the tray icon and select **Show Settings**.
- **Quit:** Right-click the tray icon and select **Quit**.
- **Start again:** Launch DualFlow normally and wait for reconnection.
- **Normal Windows startup:** The interactive application starts quietly in the tray.
- **After an update:** The visible installed-version confirmation opens.

Quit stops the interactive application and its owned Shell/helper work. The privileged service is a separate installed component used for pre-login functionality.

## 14. Uninstall and Reinstall

Before uninstalling:

1. Save work.
2. Finish all transfers and drag operations.
3. Confirm physical keyboard and mouse input works locally.

Use:

**Windows Settings → Apps → Installed apps → DualFlow KVM → Uninstall**

If removal reports **incomplete**, keep the error and logs. Retry after the reported obstruction clears, or repair with the verified installer.

Do not manually delete program files, services, tasks, or protected configuration.

Configuration and pairing are intentionally retained for supported reinstall. Uninstall is not a factory reset or trial reset.

After reinstalling, check the role, layout, **Linked**, input, and file operations.

---

## Troubleshooting

| Symptom | What to check |
| --- | --- |
| **Discovering / Waiting for link** | Confirm one Host and one Client, matching pairing code and ports, and a reachable trusted LAN. Run the connection test on Host. |
| **Linked, but pointer will not cross** | Check Host’s Displays layout. Screens must share an occupied edge, not just a corner. Reach the boundary and move outward. |
| **Pointer or input becomes unresponsive** | Press **Pause** or **Ctrl+Alt+F12**. Use physical reserve input if needed. |
| **Typing does not enter the intended application** | Click its text field on the active PC. Windows application focus still determines where typing goes. |
| **NumLock works but its light does not change** | Hardware keyboard LED synchronization is not implemented. |
| **Cannot control UAC or the lock screen** | Check Client’s installed input service under Diagnostics & Test. Do not disable Windows security protections. |
| **Remote Ctrl+Alt+Del does nothing** | Check service status and the displayed policy/request result. A request acknowledgment is not proof that Windows displayed the screen. |
| **Drag & Drop does not copy** | Keep the left button held across the edge, then release over Desktop or Explorer. Check connectivity, permissions, and disk space. |
| **File paste does not start** | Use Copy, then Paste on Desktop or in Explorer. Check that source files are available locally and sufficient space exists. |
| **Display layout looks wrong after a hardware change** | Review Displays on Host. Windows may have assigned a different monitor identity. |
| **No update appears** | Check internet access and whether a newer compatible release is available through the configured update source. |
| **Client update fails** | Follow the error. Host should remain unchanged; do not repeatedly start another update. |
| **Signature, version, or readiness validation fails** | Stop and record the exact message. Do not bypass validation. |
| **Uninstall reports incomplete** | Retain the log and recovery path. Retry after the obstruction clears or repair with the verified installer. |
| **Windows Security flags the installer** | Stop and record the exact detection. Do not disable Defender, add exclusions, or restore a flagged package merely to continue. |

### Diagnostic Logs

Depending on the operation, relevant logs may be present in the installation directory, normally:

```text
C:\Program Files\DualFlow KVM\input-service-install.log
C:\Program Files\DualFlow KVM\helper-panic.log
C:\Program Files\DualFlow KVM\update-lifecycle.log
```

When reporting an issue, include:

- Version shown on both PCs.
- Which PC is Host and which is Client.
- Direction of the failing action.
- Reproduction steps and approximate time.
- Relevant error messages and diagnostics.

Review files before sharing. Never include passwords, PINs, pairing codes, license keys, or private signing material.

---

## Quick Reference

| Action | Shortcut or location |
| --- | --- |
| Return to Host | **Pause** or **Ctrl+Alt+F12** |
| Request Client Ctrl+Alt+Del | **Ctrl+Alt+End** while controlling Client |
| Cancel a drag | **Esc** |
| Arrange displays | Host → **Displays** |
| Pair PCs or change network settings | **Settings** |
| Connection test and benchmark | **Diagnostics & Test** |
| Everyday instructions and secure-attention action | **How to Use** |
| Update both PCs | Host → **Settings → Check for updates → Update both PCs** |
| Open the window | Tray → **Show Settings** |
| Stop the interactive application | Tray → **Quit** |

[Website](https://getdualflow.com/) · [Downloads](https://github.com/sorensenlasse1980-code/dualflow-kvm/releases) · [Back to README](README.md)

*DualFlow KVM — One keyboard. One mouse. Two PCs.*
