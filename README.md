# DualFlow KVM

One keyboard and mouse. Two Windows PCs.

DualFlow KVM lets you control two Windows 11 computers using the keyboard and
mouse connected to one of them. Move the pointer across a configured screen edge
to switch computers, then keep working with the same keyboard.

**Current version: 1.11.0** · Windows 11 x64 · Local network

[Read the English User Guide](USER_GUIDE.md)

## How it works

The **Host** is the PC with your physical keyboard and mouse. The **Client** is
the other PC. Each computer uses its own connected monitors: DualFlow forwards
input and clipboard data, **not video or audio**.

Pair the computers once, arrange their displays on the Host, and leave DualFlow
running in the Windows notification area. Saved settings are reused when you
sign in again.

## Features

- Automatic local-network discovery, with optional fixed IPv4 addresses and
  custom ports.
- One-time pairing with authenticated, encrypted communication.
- A Host-controlled 6 × 4 display canvas with multi-cell ultrawide and surround
  layouts. The Client receives the layout automatically.
- Mouse, keyboard, scrolling and shared text/image clipboard.
- File and folder transfer through Explorer copy/paste, preserving nested files
  and empty folders in either direction.
- Centralized Client pointer sensitivity and optional cursor-size synchronization.
- An installed Windows input service for administrator windows and UAC prompts.
- Silent startup at Windows sign-in, a system tray menu and connection diagnostics.
- Emergency return to Host and automatic recovery when remote control fails.
- A 10-day Host trial and a lifetime license for one Host/Client pair.
- Signed GitHub updates on either PC, including after the trial expires.

The background engine uses Rust and native Windows APIs; the settings UI uses
Tauri v2 and React, not Electron. Resource use depends on input activity and file
transfers; no fixed CPU, memory or latency guarantee is made.

## Requirements

- Two Windows 11 x64 PCs, with their monitors connected normally.
- A trusted local network that allows the PCs to communicate. Use the Windows
  **Private** network profile only on a network you trust.
- Administrator permission to install and run DualFlow.
- Microsoft Edge WebView2 Runtime for the settings window.
- Local input available on the Client for first-time installation and recovery.
- Internet on Host for license activation; internet on each PC for updates.
  LAN control and saved lifetime activation work without internet.

End users do **not** need Rust, Node.js or Visual Studio.

## Get started

Use the Windows setup EXE supplied with the release:
`DualFlow-KVM-v1.11.0-setup.exe`. Install this version manually on **both** PCs
when upgrading from v1.10.x: the authenticated protocol changed in v1.11.0.

1. On PC 1, open **Settings → Computer role → Host**.
2. On PC 2, choose **Client**.
3. On Host, select **Secure PC pairing → Show pairing code**. Enter that code on
   Client and select **Save pairing code**.
4. Once the status is **Linked**, arrange the screens in **Displays** on Host.
   Screens must share a grid edge for the pointer to cross between PCs.
5. On Host, select **Settings → Connection diagnostics → Run connection test**.

See the [User Guide](USER_GUIDE.md) for the full PC-by-PC setup and daily use.
The installer configures Windows Firewall, startup and the input service; no
manual PowerShell firewall script is required. Investigate any setup warning:
successful file installation alone does not confirm that the input service works.

## Everyday controls

- **Switch PCs:** move through a shared screen edge. The keyboard follows the
  active pointer.
- **Return immediately:** press **Pause** or **Ctrl+Alt+F12** on the Host keyboard.
- **Open settings:** right-click the DualFlow tray icon and choose **Show Settings**.
- **Keep running:** close the settings window; DualFlow stays in the tray.
- **Stop DualFlow:** choose **Quit** from the tray menu.

## Trial, activation and updates

The trial starts the first time a PC is configured as Host and lasts 10 UTC
calendar days. Client does not need its own license key or an internet connection
for licensing. Click **Trial: X days left** on Host to activate or open the store.
The lifetime license is €29.95 for one Host and its paired Client, subject to the
store's checkout terms. Only one Host activation is allowed.

Trial expiry stops shared input, clipboard and file transfers; local recovery,
settings and updates remain available. Reinstalling does not reset the trial.
Clock rollback expires a trial, but does not invalidate a saved paid license.

When a signed release is available, click **Update now** on each PC. Installation
briefly disconnects KVM and restarts DualFlow in the tray. Keep both PCs on
compatible versions. See publisher documentation for configuration, signing-key
backup and release validation.

## Security and practical limits

Pair only computers you trust. The pairing code authorizes remote control; do not
publish it or include it in screenshots or support reports. Clipboard sharing is
automatic while linked, including copied text, images and supported files. Quit
DualFlow if you do not want new clipboard contents sent to the other PC.

UAC support requires the installed input service. DualFlow does not automatically
approve prompts, disable Windows security or bypass passwords. Protected sign-in
and Ctrl+Alt+Delete scenarios are not universal compatibility guarantees. Keep a
local recovery method available until your own setup has been tested.

This release does not provide remote video, internet relay, Wake-on-LAN or a
global Explorer drag-and-drop operation across screen edges. File transfer uses
copy/paste; drops onto DualFlow itself work only when Windows permits them.
Transfers do not preserve filesystem permissions, timestamps or alternate data
streams, and symbolic links, junctions and other reparse points are rejected.

## Build from source

For developers: use Windows 11, Node.js 22.12 or later, Rust stable with the
`x86_64-pc-windows-msvc` toolchain, and Visual Studio 2022 Build Tools with
**Desktop development with C++**, including the Windows SDK. WebView2 is also
required to run the UI.

Open PowerShell in the project folder containing `package.json`:

```powershell
npm.cmd ci
.\scripts\build-release.ps1 -SigningKey 'C:\private\dualflow-updater.key'
