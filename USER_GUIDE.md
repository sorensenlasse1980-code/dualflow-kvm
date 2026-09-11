# DualFlow KVM — User Guide
**For Windows 11 · Version 1.11.5**

Control two PCs with one keyboard and mouse. **PC 1 is the Host**, where your physical keyboard and mouse are connected. **PC 2 is the Client**, which you control seamlessly over the local network. Both PCs use their own connected monitors: DualFlow forwards low-latency input and clipboard streams, not heavy video or audio.

---

## Before you start
* Connect both PCs to the same trusted local network (use the **Private network** profile in Windows; avoid guest networks with device isolation).
* Have Windows administrator permissions available on both PCs during installation.
* Keep a temporary mouse/keyboard handy on the Client PC during the very first pairing.
* Use the **`DualFlow-KVM-v1.11.5-setup.exe`** installer on both PCs.

---

## 1. Set up PC 1 — Host
1. Run `DualFlow-KVM-v1.11.5-setup.exe` and complete the setup. Launch DualFlow KVM.
2. Open **Settings**. Under **Computer role**, select **Host**.
3. Leave **Network mode** set to **Auto discovery** (unless your network specifically requires fixed IPs).
4. Under **Secure PC pairing**, click **Show pairing code**. Copy or note the complete code. Treat this code securely: it authorizes remote control between the machines.
5. Keep DualFlow running while you prepare PC 2.

---

## 2. Set up PC 2 — Client
1. Run `DualFlow-KVM-v1.11.5-setup.exe` on PC 2 and launch DualFlow KVM.
2. Open **Settings → Computer role** and select **Client**.
3. Leave **Network mode** on **Auto discovery**.
4. Under **Secure PC pairing**, enter the pairing code generated on the Host, then click **Save pairing code**.
5. Wait for the status indicator in the upper-right corner to show **Linked**.
6. Check **Administrator prompts** in Settings: the background input service should report as connected. (This enables cursor control over UAC and elevated windows).

> [!NOTE]
> Only one PC can be the Host. The Client PC receives display arrangements, layouts, and pointer settings automatically from the Host.

---

## 3. Arrange and Test Displays (Host)
1. On the **Host PC**, go to the **Displays** tab. Identify each display by its PC name and resolution.
2. Drag each display card so the layout on screen mirrors your physical monitor setup (side-by-side or stacked).
3. Ensure screen edges touch on the grid:
   * Standard 16:9 displays occupy 1 tile.
   * Ultrawide (32:9) displays occupy 2 columns; triple-surround setups occupy 3 columns.
4. Changes save automatically and sync to the Client immediately.
5. Move your mouse pointer across the shared edge to control the Client PC. Test typing in a text field, then bring the cursor back to the Host.
6. You can now unplug the temporary physical mouse and keyboard from the Client PC.

---

## Software Updates (Crucial Sequence)

DualFlow checks for signed updates in the background. When an update is ready:

> [!IMPORTANT]
> **ALWAYS UPDATE IN THIS ORDER:**
> 1. **Update the Client PC first:** Move your cursor to the Client PC, open DualFlow Settings, and click **Update now**. The application will terminate safely, run the unattended update, and relaunch automatically with an **Updated to v1.11.5** confirmation banner. No local mouse or UAC clicks are required on the Client.
> 2. **Update the Host PC second:** Move your cursor back to the Host PC and trigger **Update now**.
> 3. Once the Host restarts, seamless control and cross-PC clipboard sharing reconnect automatically.

Starting Windows normally will always launch DualFlow quietly to the system tray. The main window opens only after an active update to confirm completion.

---

## Daily Use

### Switching Between PCs
* **Move across the edge:** Glide the cursor through any shared monitor border configured in your display layout. The keyboard follows the cursor automatically.
* **Edge Guard:** The bottom edge of the Client screen features a small guard zone so you can easily click the taskbar without accidentally jumping back to the Host.
* **Emergency Return Shortcut:** Press `Pause` (or `Ctrl + Alt + F12`) on your physical Host keyboard to snap the cursor back to the Host screen immediately.

### Copying Text, Images, Files, and Folders
* **Text & Images:** Copy on one PC with `Ctrl + C`, move to the other PC, and paste with `Ctrl + V`.
* **Files & Folders (On-Demand Streaming):**
  1. Select files or folders in File Explorer and press `Ctrl + C`. (DualFlow advertises the files instantly without heavy network transfer up front).
  2. Switch to the target PC, navigate to any destination folder (or the **Windows Desktop**), and press `Ctrl + V`.
  3. The file payload streams directly on demand, preserving nested folders and directory structures.
* **Desktop Pasting:** Fully supported via native Windows Shell COM integration (`InShellDragLoop`).

---

## Trial & Lifetime Licensing (Host Only)
* A **10-day trial** begins automatically when a PC is first set to Host. Full functionality is available during the trial.
* To purchase, click **Buy Lifetime License (€29.95)** on the Host. Complete checkout to receive your license key.
* Enter the key in DualFlow on the Host and click **Activate**. (Internet access is required only for this one-time activation).
* **The Client PC does not need a license key or internet access.** As long as the Host is licensed, the Client connects without restrictions.
* Saved lifetime licenses remain fully functional offline during everyday LAN use.

---

## Optional Settings

* **Pointer Sensitivity & Size (Host):**
  * Keep **Auto-sync sensitivity (DPI match)** enabled for automatic DPI matching across different screen resolutions.
  * Adjust the manual **Client Speed Multiplier** (0.50× to 2.50×) if you prefer a faster or slower cursor on the secondary PC.
  * Enable **Sync cursor size** to keep the pointer size identical across both systems.
* **Fixed IP / Custom Ports:**
  * If your router or subnet blocks UDP discovery, switch **Network mode** to **Manual / fixed IP** on both PCs and enter the partner machine's IP address. Default ports: UDP `45831` (Discovery) and TCP `45832` (Input).

---

## Troubleshooting

| Symptom | Resolution |
| :--- | :--- |
| **Discovering / Waiting for link** | Verify both machines are set to the Private Network profile in Windows. Ensure one PC is set as Host and the other as Client, and verify the pairing code matches. |
| **Linked, but pointer won't cross** | Check the **Displays** tab on Host. Screens must share an adjacent occupied grid edge (corners touching diagonally do not count). |
| **Pointer lost / unresponsive** | Press `Pause` or `Ctrl + Alt + F12` on the Host keyboard to force-return the cursor. Check connection diagnostics in Settings. |
| **Cannot control UAC or Admin prompts** | Confirm the background input service is running. Check **Administrator prompts** in Settings on the Client PC. Re-run setup if the service was removed. |
| **File transfer paste doesn't trigger** | Ensure you are pasting into standard File Explorer or onto the Desktop. Reparse points, cloud placeholders, and symbolic links are excluded for stability. |

Diagnostic logs are located at:
```text
C:\Program Files\DualFlow KVM\update-lifecycle.log
C:\Program Files\DualFlow KVM\input-service-install.log
