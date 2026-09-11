# DualFlow KVM
**One keyboard and mouse. Two Windows PCs.**

DualFlow KVM lets you control two Windows 11 computers using the keyboard and mouse connected to one of them. Move the pointer across a configured screen edge to switch computers, then keep working seamlessly with the same keyboard.

**Current version:** 1.11.5 · Windows 11 x64 · Local network

[Read the English User Guide](USER_GUIDE.md)

---

## How it works
The **Host** is the PC with your physical keyboard and mouse. The **Client** is the secondary PC. Each computer uses its own connected monitors: DualFlow forwards low-latency input and clipboard streams, not heavy video or audio.

Pair the computers once, arrange your displays on the Host, and leave DualFlow running quietly in the Windows notification area. Saved configurations and paired states restore automatically when you sign in.

---

## Features
* **Zero-Touch Updates:** Hands-free background updater on secondary/headless Client machines—no physical mouse or UAC intervention required.
* **Smart File & Folder Transfers:** Explorer copy/paste with on-demand (lazy) streaming. Large files transfer only when requested; nested folder trees and empty directories are fully preserved.
* **Native Desktop Paste:** Full Windows Shell COM integration (`InShellDragLoop`) allowing direct copy/paste straight to the Windows Desktop.
* **Low-Latency Input:** High-performance cursor and keyboard switching across multi-monitor setups.
* **Display Canvas:** Host-controlled 6 × 4 grid supporting multi-cell ultrawide and surround monitor layouts.
* **Encrypted Local Pairing:** One-time code exchange with fully authenticated, end-to-end encrypted local network communication.
* **UAC & Elevated Windows:** Dedicated Windows input service ensures smooth cursor control over administrator prompts and elevated windows.
* **Silent Boot, Clear Updates:** Starts silently in the system tray on Windows boot; reopens cleanly with visual version confirmation after an in-app update.
* **Native Lightweight Stack:** Built with Rust and native Windows APIs; UI powered by Tauri v2 and React (no heavy Electron overhead).

---

## Requirements
* Two Windows 11 x64 PCs connected to the same trusted local network (LAN / Private Network Profile).
* Administrator permissions to install and run DualFlow (for the low-latency input service).
* Microsoft Edge WebView2 Runtime (pre-installed on Windows 11).
* Local input available on Client for initial setup and recovery.
* Internet connection on Host for license activation; internet on both PCs for updates. (Daily LAN control and saved lifetime activation work completely offline).

---

## Getting Started

Download the latest installer: **`DualFlow-KVM-v1.11.5-setup.exe`** from [Releases](https://github.com/sorensenlasse1980-code/dualflow-kvm/releases).

1. **Install:** Run setup on both computers.
2. **Assign Roles:**
   * On PC 1 (with keyboard/mouse), select **Host**.
   * On PC 2, select **Client**.
3. **Pair:** On Host, click **Secure PC pairing → Show pairing code**. Enter the code on the Client and click **Save pairing code**.
4. **Arrange Displays:** Once the status shows **Linked**, drag and arrange your screens under **Displays** on the Host so screen edges align.
5. **Test:** Move your cursor across the edge to control the Client PC.

> [!TIP]
> **Update Rule:** When updating DualFlow KVM in the future, **always update the Client PC first, then the Host PC**. This guarantees continuous cursor control over your secondary screen.

---

## Everyday Controls
* **Switch PCs:** Move the cursor through a shared screen edge. The keyboard follows automatically.
* **Emergency Return:** Press `Pause` or `Ctrl + Alt + F12` on the Host keyboard to instantly snap the cursor back to Host.
* **Open Settings:** Right-click the DualFlow tray icon and choose **Show Settings**.
* **Quit:** Choose **Quit** from the tray icon menu.

---

## Licensing & Updates
* **10-Day Free Trial:** Starts automatically when a PC is first configured as Host. Full functionality is available during the trial.
* **Lifetime License:** A single one-time purchase (€29.95) covers one Host and paired Client setup indefinitely. The Client PC does not require its own license key.
* **In-App Updates:** Click **Update now** when notified. Updates install silently and relaunch DualFlow with a clear **Updated to v1.11.5** confirmation banner.

---

## Security & Practical Boundaries
* **Local LAN Only:** DualFlow never routes your inputs or clipboard over external cloud servers. All traffic is peer-to-peer and encrypted.
* **UAC Prompts:** DualFlow routes inputs to elevated dialogs via its input service, but never disables or bypasses Windows security policies.
* **File Transfers:** Optimized for local productivity; symbolic links, reparse points, and active Windows permissions are safely excluded from file transfers.

---

## Build from Source (Developers)
**Prerequisites:** Windows 11, Node.js 22+, Rust stable (`x86_64-pc-windows-msvc`), and Visual Studio 2022 Build Tools (Desktop development with C++ & Windows SDK).

```powershell
npm.cmd ci
.\scripts\build-release.ps1 -SigningKey 'C:\path\to\dualflow-updater.key'
