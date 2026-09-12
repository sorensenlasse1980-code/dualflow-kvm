# DualFlow KVM

**One keyboard and mouse. Two Windows PCs.**

DualFlow KVM is a high-performance software KVM for Windows 11 that lets you control two computers using the keyboard and mouse connected to one of them.

Move the pointer across a configured screen edge to switch computers, and continue working seamlessly with the same keyboard and mouse.

**Current version:** 1.11.6 · Windows 11 x64 · Local network

[Read the English User Guide](https://github.com/sorensenlasse1980-code/dualflow-kvm/blob/main/USER_GUIDE.md)

---

## How it works

The **Host** is the PC with your physical keyboard and mouse. The **Client** is the secondary PC.

Each computer uses its own connected monitors. DualFlow forwards low-latency keyboard, mouse, clipboard, and file-transfer data over your local network — not video or audio streams.

Pair the computers once, arrange your displays on the Host, and leave DualFlow running quietly in the Windows notification area.

Saved configurations, display layouts, and paired states restore automatically when you sign in.

---

## Features

* **Zero-Touch Updates:** Hands-free background updates on secondary or headless Client machines, with no physical mouse or UAC intervention required.

* **Smart File & Folder Transfers:** Explorer copy/paste with on-demand (lazy) streaming. Large files transfer only when requested, while nested folder trees and empty directories are fully preserved.

* **Native Desktop Paste:** Full Windows Shell COM integration (`InShellDragLoop`) allows direct copy/paste straight to the Windows Desktop.

* **Low-Latency Input:** High-performance cursor and keyboard switching designed for responsive multi-monitor and dual-PC workflows.

* **Display Canvas:** Host-controlled 6 × 4 display grid supporting complex multi-monitor, ultrawide, stacked, and surround-style layouts.

* **Encrypted Local Pairing:** One-time code exchange with authenticated, end-to-end encrypted communication across your local network.

* **UAC & Elevated Windows:** A dedicated Windows input service maintains cursor and keyboard control over administrator prompts and elevated applications.

* **Silent Boot, Clear Updates:** Starts silently in the Windows system tray and relaunches cleanly after updates with visual version confirmation.

* **Native Lightweight Stack:** Built with Rust and native Windows APIs, with a Tauri v2 and React interface — without the overhead of a traditional Electron application.

---

## Requirements

* Two **Windows 11 x64** PCs connected to the same trusted local network (LAN / Private Network Profile).
* Administrator permissions to install and run DualFlow for the low-latency input service.
* Microsoft Edge WebView2 Runtime, which is normally pre-installed on Windows 11.
* Local input available on the Client PC for initial setup and recovery.
* Internet connection on the Host for license activation.
* Internet connection on both PCs for software updates.

Daily LAN control and an already activated lifetime license continue to work offline.

---

## Getting Started

Download the latest installer:

**`DualFlow-KVM-v1.11.6-setup.exe`**

from the [DualFlow KVM Releases page](https://github.com/sorensenlasse1980-code/dualflow-kvm/releases/latest).

1. **Install**
   Run the DualFlow installer on both computers.

2. **Assign Roles**

   * On PC 1, where your keyboard and mouse are physically connected, select **Host**.
   * On PC 2, select **Client**.

3. **Pair the PCs**
   On the Host, click **Secure PC pairing → Show pairing code**.
   Enter the code on the Client and click **Save pairing code**.

4. **Arrange Displays**
   Once the connection status shows **Linked**, open **Displays** on the Host and arrange the screens so their edges match your physical monitor layout.

5. **Test the Connection**
   Move the cursor across a configured shared edge to begin controlling the Client PC.

> **Update Rule**
>
> When updating DualFlow KVM, always update the **Client PC first**, then the **Host PC**. This helps maintain continuous cursor control over the secondary computer during the update process.

---

## Everyday Controls

* **Switch PCs:** Move the cursor through a configured shared screen edge. The keyboard follows automatically.

* **Emergency Return:** Press `Pause` or `Ctrl + Alt + F12` on the Host keyboard to immediately return cursor control to the Host.

* **Open Settings:** Right-click the DualFlow tray icon and choose **Show Settings**.

* **Quit:** Right-click the tray icon and choose **Quit**.

---

## Licensing & Updates

* **10-Day Free Trial:** Starts automatically when a PC is first configured as Host. Full functionality is available during the trial.

* **Lifetime License:** A single one-time purchase of **€29.95** covers one Host and its paired Client setup indefinitely. The Client PC does not require a separate license key.

* **In-App Updates:** Click **Update now** when a new version is available. Updates install automatically and DualFlow relaunches with a clear **Updated to v1.11.6** confirmation banner.

---

## Security & Practical Boundaries

* **Local LAN Only:** DualFlow does not route keyboard, mouse, clipboard, or file-transfer traffic through external cloud relay servers. Daily PC-to-PC communication remains local and encrypted.

* **UAC Prompts:** DualFlow can route input to elevated dialogs through its dedicated input service, but it does not disable, circumvent, or weaken Windows security policies.

* **Encrypted Pairing:** Host and Client establish an authenticated encrypted connection using the pairing process.

* **File Transfers:** Designed for normal local productivity workflows. Symbolic links, reparse points, and unsupported Windows permission structures are safely excluded from transfers.

---

## Built for Dual-PC Workflows

DualFlow KVM is designed for users who run two Windows 11 PCs side by side and want them to behave like one extended workspace.

Typical setups include:

* Gaming PC + streaming PC
* Sim racing PC + telemetry or streaming PC
* Development workstation + secondary PC
* Productivity workstation + dedicated utility PC
* Multi-monitor dual-PC desks
* Secondary or headless Client systems

Because DualFlow transfers input and productivity data instead of streaming entire displays, each PC continues to use its own GPU and connected monitors.

---

## Build from Source

### Prerequisites

* Windows 11
* Node.js 22+
* Rust stable with the `x86_64-pc-windows-msvc` target
* Visual Studio 2022 Build Tools
* Desktop development with C++
* Windows SDK

### Build

```powershell
npm.cmd ci
.\scripts\build-release.ps1 -SigningKey 'C:\path\to\dualflow-updater.key'
```

---

## Links

* [DualFlow KVM Website](https://getdualflow.com/)
* [Latest Release](https://github.com/sorensenlasse1980-code/dualflow-kvm/releases/latest)
* [User Guide](https://github.com/sorensenlasse1980-code/dualflow-kvm/blob/main/USER_GUIDE.md)
* [GitHub Repository](https://github.com/sorensenlasse1980-code/dualflow-kvm)
