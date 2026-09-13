**For Windows 11 · Version 1.12.0**

Control two PCs with one keyboard and mouse. **PC 1 is the Host**, where your physical keyboard and mouse are connected. **PC 2 is the Client**, which you control seamlessly over the local network. Both PCs use their own connected monitors: DualFlow forwards low-latency input, clipboard, and file-transfer data — not heavy video or audio streams.

[Back to README](README.md)

---

## Before you start

- Connect both PCs to the same trusted local network (use the **Private network** profile in Windows; avoid guest networks with device isolation).
- Have Windows administrator permissions available on both PCs during installation.
- Keep a temporary mouse/keyboard available on the Client PC during initial setup, recovery, and update testing.
- For a new installation, use **`DualFlow-KVM-v1.12.0-setup.exe`** on both PCs.
- For the coordinated v1.11.8 → v1.12.0 update test, **do not manually install v1.12.0 first**. Keep v1.11.8 installed on both PCs so the Host can perform the coordinated update.

---

## Already running v1.11.8 on both PCs?

If both PCs are already running v1.11.8, keep v1.11.8 installed to test the coordinated update. Do not manually install v1.12.0 first, or that PC will no longer need this update.

1. Save work on both PCs and finish any active file transfers. Keep local Client input available for recovery.
2. Make sure both PCs are signed in, paired and **Linked**, with internet available for update downloads.
3. On the Host, open DualFlow from the tray and choose **Check for updates**.
4. When v1.12.0 is offered, choose **Update both PCs** and confirm once.
5. The **Client updates first**. Wait while it installs, restarts/relaunches and reconnects. Do not start another installer or change roles.
6. The Host verifies the Client's identity, running version and input-helper readiness before starting its own update.
7. The Host then updates and relaunches.
8. Confirm that the Host window opens and shows **Updated to v1.12.0**.
9. Verify that both PCs are running v1.12.0 and that normal KVM operation has resumed.

> **Important**
>
> If the Client update fails, reconnects with the wrong version, or fails the required health check, the Host must remain unchanged. Follow the displayed error instead of repeatedly starting the update.
>
> A manual local installation remains a recovery option, but it does **not** validate the coordinated-update sequence.

The release must be available through DualFlow's configured official GitHub update endpoint before the production update can be detected. A private draft or an unselected prerelease is not the production latest-release target.

---

## 1. Set up PC 1 — Host

1. Run `DualFlow-KVM-v1.12.0-setup.exe` and complete the setup. Approve the normal Windows administrator prompt if you trust the installer.
2. Launch DualFlow KVM and open **Settings**.
3. Under **Computer role**, select **Host**.
4. Leave **Network mode** set to **Auto discovery** unless your network specifically requires fixed IP addresses.
5. Under **Secure PC pairing**, click **Show pairing code**. Copy or note the complete code. Treat this code securely: it authorizes the connection between the machines.
6. Keep DualFlow running while you prepare PC 2.

---

## 2. Set up PC 2 — Client

1. Run `DualFlow-KVM-v1.12.0-setup.exe` on PC 2 and launch DualFlow KVM.
2. Open **Settings → Computer role** and select **Client**.
3. Leave **Network mode** on **Auto discovery** unless your network requires manual addressing.
4. Under **Secure PC pairing**, enter the pairing code generated on the Host, then click **Save pairing code**.
5. Wait for the status indicator to show **Linked**.
6. Check the Client's **Administrator prompts** / input-service status in Settings. The installed background input service should report as connected before testing UAC or elevated windows.
7. Return to the Host to arrange the displays. Display layout is controlled from the Host.

> **Note**
>
> Only one PC can be the Host. The Client receives display arrangements, layouts and pointer settings from the Host.

### Upgrading from v1.11.7 or earlier

When upgrading from **v1.11.7 or earlier**, install the newer coordinator-capable version separately on both PCs.

Do not uninstall DualFlow or delete its configuration first.

v1.11.8 and v1.12.0 share the compatible update/protocol baseline used for coordinated updates. Earlier versions require both PCs to be upgraded before the new **Update both PCs** workflow can be used.

---

## 3. Arrange and Test Displays (Host)

1. On the **Host PC**, open the **Displays** tab and identify each display by its PC name and resolution.
2. Drag each display card so the layout mirrors your physical monitor arrangement.
3. Ensure the relevant screen edges touch on the grid:
   - Standard 16:9 displays occupy 1 tile.
   - Ultrawide displays can occupy multiple grid cells.
   - An empty neighboring grid cell does not lead to another screen.
4. Changes save automatically and sync to the Client.
5. Move the mouse pointer to the actual shared screen edge and continue outward to control the Client.
6. Click a text field on the Client and verify that keyboard input follows the pointer.
7. Move back to the Host and verify normal return behavior.
8. Test reaching the Client taskbar without unintended crossing.

**Emergency return:** Press `Pause` or `Ctrl + Alt + F12` on the physical Host keyboard to return cursor control to the Host immediately.

Keep physical local input available during initial setup, recovery and update testing.

---

## Software Updates

DualFlow checks for signed updates. From the coordinator-capable update baseline, the Host can coordinate compatible updates for both paired PCs.

### Update both PCs

1. On the Host, choose **Check for updates**.
2. When a compatible newer version is available, choose **Update both PCs** and confirm once.
3. The Client independently downloads and verifies its own signed update package.
4. The Client updates first and reconnects automatically.
5. The Host verifies the Client's authenticated identity, actual running version and required input-helper readiness.
6. Only after Client verification succeeds does the Host start its own update.
7. The Host updates and relaunches.
8. DualFlow opens using the existing post-update confirmation flow and shows the version actually running.

> **Update safety**
>
> A re-opened network connection alone is not treated as a successful Client update. If Client validation fails, the Host remains on its existing version.
>
> Each PC downloads and signature-checks its own update package. The Host does not stream installer binaries to the Client.

Normal Windows sign-in launches DualFlow quietly to the system tray. A completed in-app update uses the existing visible post-update confirmation.

---

## Daily Use

### Switching Between PCs

- **Move across the edge:** Glide the cursor through a configured shared monitor border. The keyboard follows automatically.
- **Edge Guard:** The Client taskbar area is protected against unintended crossing where applicable.
- **Emergency Return:** Press `Pause` or `Ctrl + Alt + F12` on the physical Host keyboard to immediately return cursor control to the Host.

### Copying Text, Images, Files, and Folders

- **Text & Images:** Copy on one PC with `Ctrl + C`, move to the other PC, and paste with `Ctrl + V`.
- **Files & Folders (On-Demand Streaming):**
  1. Select ordinary local files or folders in File Explorer and press `Ctrl + C`.
  2. Move to the receiving PC and select the destination folder or **Windows Desktop**.
  3. Press `Ctrl + V` or use the destination's Paste action.
  4. The file payload streams on demand. Wait for completion and verify the result before deleting the originals.
- **Desktop Pasting:** Supported through native Windows Shell integration.

Nested folders and empty directories are retained. Temporary staging and the final pasted copy require sufficient free disk space.

Reparse points, junctions and symbolic links are not ordinary supported transfer items. Cloud placeholders may need to be downloaded locally first.

> **Not included**
>
> Cross-screen Explorer Drag & Drop is not added by v1.12.0. Keyboard LED synchronization is not added either; the physical NumLock LED on the Host keyboard is not a remote-state indicator.

---

## Trial & Lifetime Licensing (Host Only)

- A **10-day trial** begins automatically when a PC is first configured as Host. Full functionality is available during the trial.
- To purchase, click **Buy Lifetime License (€29.95)** on the Host and complete checkout to receive your license key.
- Enter the key in DualFlow on the Host and click **Activate**.
- **The Client PC does not require a separate license key.**
- Activate and manage licensing through the existing Host license status/dialog.
- Saved lifetime licenses remain functional for normal offline LAN use after activation.

---

## Optional Settings

- **Pointer Sensitivity & Size (Host):**
  - Keep **Auto-sync sensitivity (DPI match)** enabled for automatic DPI matching across different screen resolutions.
  - Adjust the manual **Client Speed Multiplier** if you prefer a faster or slower cursor on the secondary PC.
  - Enable **Sync cursor size** to keep pointer sizing consistent across both systems.
- **Fixed IP / Custom Ports:**
  - If automatic discovery is unavailable, switch **Network mode** to **Manual / fixed IP** and enter the **other PC's IPv4 address**.
  - Both PCs must use matching DualFlow ports.
  - This setting tells DualFlow which address to use; it does not configure a static IP address in Windows itself.
- **Diagnostics & Test:**
  - Use **Diagnostics & Test** for an optional benchmark.
  - Start and stop the test explicitly.
  - Reports are copied and automatically saved under `Documents\DualFlow KVM\Benchmarks\`.
  - Capture-to-inject transit is unavailable; application RTT is not a synchronized one-way latency measurement.

---

## Tray and Startup

Closing the main DualFlow window leaves the application running in the Windows notification area.

- **Show Settings:** Right-click the DualFlow tray icon and choose **Show Settings**.
- **Quit:** Right-click the tray icon and choose **Quit**. This closes DualFlow and its active connection.
- **Normal Windows sign-in:** DualFlow starts quietly in the tray.
- **After an in-app update:** The existing visible post-update confirmation opens and reports the version actually running.

---

## Troubleshooting

| Symptom | Resolution |
| --- | --- |
| **Discovering / Waiting for link** | Verify both applications are running, one PC is Host and the other Client, the pairing code matches, ports match, and both machines can reach each other on the trusted local network. |
| **Linked, but pointer won't cross** | Check the **Displays** tab on Host. Screens must share an adjacent occupied grid edge; corners touching diagonally do not count. |
| **Pointer lost / unresponsive** | Press `Pause` or `Ctrl + Alt + F12` on the Host keyboard. If recovery fails, use local input and the tray menu. |
| **Cannot control UAC or administrator prompts** | Check that the Client input service is connected. Use the installer rather than a copied standalone application EXE. Do not disable UAC or Secure Desktop. |
| **File transfer paste doesn't trigger** | Paste into standard File Explorer or onto the Desktop. Check that the source consists of supported local files/folders and that sufficient disk space is available. |
| **No update appears** | Check that both PCs have internet access, the published release is newer than the installed version, and the configured update manifest is reachable. |
| **Client update fails** | Host should remain unchanged. Follow the displayed error and do not repeatedly restart the coordinated update. |
| **Signature / version / health error** | Stop the update attempt and record the exact message. Do not bypass update validation. |

Diagnostic logs are normally located at:

```text
C:\\Program Files\\DualFlow KVM\\input-service-install.log
C:\\Program Files\\DualFlow KVM\\helper-panic.log
