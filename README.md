# DualFlow KVM

**One keyboard and mouse. Two Windows PCs.**

DualFlow KVM is a software KVM for Windows 11 that lets you control two computers using the keyboard and mouse connected to one of them.

Move the pointer across a configured screen edge to switch computers, and continue working with the same keyboard and mouse.

**Current version:** 1.13.0 · **Platform:** Windows 11 x64 · **Connection:** Local network

[Website](https://getdualflow.com/) · [Downloads](https://github.com/sorensenlasse1980-code/dualflow-kvm/releases/latest) · [English User Guide](https://github.com/sorensenlasse1980-code/dualflow-kvm/blob/main/USER_GUIDE.md) · [License Store](https://dualflow-kvm.lemonsqueezy.com)

---

## How It Works

The **Host** is the PC with your main physical keyboard and mouse. The **Client** is the secondary PC.

Each computer uses its own connected monitors, applications, and graphics hardware. DualFlow forwards keyboard, mouse, clipboard, and file-transfer data over your local network — **not video or audio streams**.

Pair the computers once, arrange your displays on Host, and leave DualFlow running quietly in the Windows notification area.

The installed Windows input service supports configured pre-login and lock-screen operation. Initial installation and pairing must be completed while signed in.

## What’s New in v1.13.0

- **Native Cross-PC Drag & Drop:** Copy files and folders between Desktop and File Explorer on either PC.
- **Windows Sign-In Control:** Control configured, paired machines before the first user login and at lock/password screens.
- **Remote Ctrl+Alt+Del:** Request the Client’s Windows security screen using a shortcut or the in-app action, where Windows policy permits.
- **Live Display Updates:** Refresh monitor inventory and pointer mapping after monitor disconnection, reconnection, resolution, and scaling changes.
- **Improved Drag Lifecycle:** More reliable source admission, handoff, cancellation, and cleanup.
- **Built-In How to Use:** Everyday instructions and remote secure-attention controls inside the application.
- **Installer and Uninstaller Improvements:** Installation-identity checks, bounded file-lock handling, verified cleanup, and a recoverable failure path.

The established pointer, keyboard, clipboard, Copy/Paste, licensing, diagnostics, and coordinated-update workflows are preserved.

## Features

### Responsive Mouse and Keyboard Sharing

Cross a configured shared display edge to control the other PC. Keyboard input follows the active PC.

Normal clicking, holding, scrolling, keyboard forwarding, Right Shift, and NumLock functionality are supported.

Press **Pause** or **Ctrl+Alt+F12** on the Host keyboard for emergency return to Host.

### Host-Controlled Display Canvas

Arrange monitors on a **6 × 4 grid**, supporting standard, ultrawide, stacked, and surround-style configurations.

Wide monitors can span multiple cells. Only shared occupied edges connect; empty cells and diagonal corners are not crossing points.

Client receives the layout from Host. Live display changes refresh topology, and saved positions are restored where display identities remain available.

### Native Drag & Drop

Drag files or folders from Desktop or Explorer, keep the left mouse button held while crossing to the other PC, and release over the intended Desktop, Explorer window, or folder.

Supported in both directions:

- Host → Client.
- Client → Host.
- Desktop and Explorer sources and destinations.
- Individual files, empty files, empty folders, and nested folder trees.

Content is streamed on demand through the authenticated connection.

**Drag & Drop copies. The original remains on the source PC.**

Cross-PC MOVE, Shift+Drag source deletion, and Cut/Paste moves are not implemented. Press **Esc** to cancel a drag.

### Clipboard and File Copy/Paste

Synchronize text and images, or copy files and folders between the PCs using standard Windows Copy/Paste.

File contents are fetched on demand when the destination requests them. Copying a file does not immediately transfer its entire payload.

Desktop paste, Explorer destinations, nested folders, and empty directories are supported.

### Windows Sign-In, UAC, and Elevated Windows

The installed input service supports configured pre-login operation, lock screens, password entry, administrator prompts, and elevated applications.

DualFlow does not bypass passwords, disable UAC, or remove Windows authentication requirements.

### Remote Ctrl+Alt+Del

While controlling Client, press **Ctrl+Alt+End**, or use:

**How to Use → Send Ctrl+Alt+Del to Client**

Physical **Ctrl+Alt+Del** on the Host keyboard remains local to Host.

Remote secure attention requires explicit setup permission for the supported service-only Windows SAS policy. Managed Windows policy may restrict this feature.

### Update Both PCs

Host can coordinate an update from a single action:

1. Client downloads and verifies its own signed update.
2. Client installs, relaunches, and reconnects.
3. Host verifies the authenticated Client’s expected version, role, and readiness.
4. Only then does Host update itself.

Host does not stream installer binaries to Client. If Client fails verification, Host remains on its existing version.

### Quiet Startup, Clear Update Confirmation

Normal Windows startup launches DualFlow quietly in the system tray.

After a successful update, the application opens its existing non-modal version confirmation, such as:

**Updated to v1.13.0**

### On-Demand Diagnostics

Use **Diagnostics & Test** for connection checks and optional benchmarks.

Benchmark sampling is inactive outside an active test. Reports cover application RTT, local process memory, and incoming mouse intervals where available.

Capture-to-inject latency is not reported. Memory measurements cover the local Rust process, not WebView2 and the Windows service combined.

### Native Application Stack

Built with **Rust**, native Windows APIs, **Tauri v2**, **React**, and **TypeScript** — not Electron.

Performance depends on hardware, network conditions, and workload. No zero-latency or zero-resource-use guarantee is made.

---

## Requirements

- Two **Windows 11 x64** PCs on a trusted, reachable local network.
- Administrator permission for installation and privileged service setup.
- Microsoft Edge WebView2 Runtime.
- Physical keyboard and mouse available on Client during initial setup and recovery.
- Internet access on Host for license activation.
- Internet access on both PCs for update downloads.

Normal paired keyboard, mouse, clipboard, and file-transfer traffic uses the local network.

Both PCs must be awake and reachable. Pre-login control is not Wake-on-LAN.

## Getting Started

Download **`DualFlow-KVM-v1.13.0-setup.exe`** from the [Releases page](https://github.com/sorensenlasse1980-code/dualflow-kvm/releases).

### 1. Install on Both PCs

Run the installer on each computer and approve the normal Windows administrator prompt.

Setup installs the application, input service, contained Shell adapter, startup task, and application-specific firewall rules. No manual firewall PowerShell script is required.

Review the optional remote Ctrl+Alt+Del permission during installation.

### 2. Assign Roles

In **Settings**:

- Choose **Host** on the PC with your main keyboard and mouse.
- Choose **Client** on the second PC.

Use the same supported version on both PCs for normal operation.

### 3. Configure Networking

Use automatic discovery for a typical local network.

If needed, configure the peer IPv4 address and matching network ports manually in Settings. Follow any restart instruction shown after changing settings.

Guest Wi-Fi isolation, firewall restrictions, or managed network policies can prevent discovery or connection.

### 4. Pair the PCs

On Host:

1. Open **Settings → Secure PC pairing**.
2. Select **Show pairing code**.
3. Copy the complete code.

On Client:

1. Open **Settings → Secure PC pairing**.
2. Paste the complete Host code.
3. Select **Save pairing code**.

Wait until both PCs show **Linked**. Keep the pairing code private.

### 5. Arrange Displays

Open **Displays** on Host and drag monitor cards by their top-left cell to match your physical arrangement.

Move the pointer across a configured shared edge to control Client. Click the intended application or text field before typing.

Review the layout after hardware changes if Windows assigns a monitor a different identity.

---

## Everyday Controls

| Action | Control |
| --- | --- |
| Switch PCs | Move across a configured shared screen edge |
| Return immediately to Host | **Pause** or **Ctrl+Alt+F12** |
| Copy files between PCs | Copy on source, then paste on destination |
| Drag files between PCs | Hold the left button, cross the edge, release over the destination |
| Cancel an active drag | **Esc** |
| Request Client Ctrl+Alt+Del | **Ctrl+Alt+End** while controlling Client, or the action in **How to Use** |
| Open the application window | Tray icon → **Show Settings** |
| Stop the interactive application | Tray icon → **Quit** |

Closing the application window leaves background operation running.

**Quit** stops the interactive application and its owned work. The installed privileged service is a separate component used for pre-login functionality.

## Licensing

- **10-Day Trial:** Evaluate Host functionality before purchasing.
- **Lifetime License:** One-time purchase for one Host and its paired Client setup, subject to the license terms.
- **Host-Only Activation:** Enter the purchased license key on Host. Client does not require a separate key.

See the [official store](https://dualflow-kvm.lemonsqueezy.com) for current pricing, applicable taxes, and checkout terms.

The source archive does not grant an open-source license or replace the existing product license terms.

## Updating

### Update Both PCs from Host

For compatible paired installations:

1. Save work and finish active transfers or drag operations.
2. Make sure both PCs are awake, connected, and show **Linked**.
3. Ensure both PCs have internet access.
4. On Host, select **Check for updates**.
5. Select **Update both PCs**, then **Confirm update both PCs**.
6. Allow Client to update and pass verification before Host updates.
7. Confirm the installed version on both PCs.

Shared control can be temporarily unavailable while applications and services restart. Keep reserve local input available.

> **Upgrading an older installation**
>
> The coordinated workflow was introduced in v1.11.8. Installations on v1.11.7 or earlier require a separate initial upgrade on each PC.
>
> For manual upgrades, install **Client first, then Host**. A preliminary uninstall is not normally required.

Each PC independently downloads and validates its update. Do not manually replace running executables or start competing update operations.

## Uninstall and Reinstall

Save work, finish transfers, and confirm physical local input works before removing DualFlow from a PC.

Use:

**Windows Settings → Apps → Installed apps → DualFlow KVM → Uninstall**

The uninstaller checks required cleanup and does not report successful removal when mandatory steps fail.

If removal reports **incomplete**, retain the diagnostic log and recovery uninstaller. Retry after the reported obstruction clears, or repair with the verified installer. Do not manually delete product files or unrelated services/tasks.

Configuration and pairing are intentionally retained for supported reinstall. Uninstall is not a factory reset or a trial reset.

## Security and Practical Boundaries

- **Trusted Local Network:** Do not expose DualFlow’s network ports directly to the internet.
- **Authenticated Encryption:** Pair only PCs you control and trust. Protect pairing codes and license keys.
- **Windows Authentication Remains Active:** DualFlow does not bypass passwords or silently approve UAC prompts.
- **Signed Updates:** Update packages are checked against the configured updater public key.
- **Signing Distinction:** Tauri updater signatures are not Windows Authenticode publisher signatures.
- **Windows Security:** Do not disable Defender, add exclusions, or bypass security warnings to install DualFlow.
- **Diagnostic Privacy:** Review reports before sharing. Never include passwords, PINs, pairing codes, license keys, or private signing material.

## Known Limitations

- Supports Windows 11 x64 and two PCs.
- Does not stream video or audio, switch monitor inputs, or provide Wake-on-LAN.
- Native cross-PC Drag & Drop is COPY into Windows Desktop and Explorer. Arbitrary third-party application drop targets are outside the verified scope.
- Cross-PC MOVE, Shift+Drag source deletion, and Cut/Paste moves are not implemented.
- Physical keyboard LEDs are not synchronized while forwarding input. NumLock functionality and its physical indicator are separate states.
- Remote Ctrl+Alt+Del depends on supported Windows permissions and policies.
- Pre-login operation requires prior installation, configuration, and persisted pairing.
- Windows scheduling, network conditions, and display identity changes can affect behavior.

## Built for Dual-PC Workflows

Typical setups include:

- Gaming PC + streaming PC.
- Sim racing PC + telemetry or streaming PC.
- Development workstation + secondary PC.
- Productivity workstation + dedicated utility PC.
- Multi-monitor dual-PC desks.

Because DualFlow shares input and productivity data instead of streaming displays, each computer continues using its own GPU, monitors, and applications.

## Source and Build Information

The v1.13.0 source archive is available with the release assets for traceability under the existing license terms.

The Windows build environment uses:

- Rust with the `x86_64-pc-windows-msvc` target.
- Node.js and npm.
- Visual Studio 2022 Build Tools with Desktop development with C++.
- Windows SDK.
- NSIS.
- The versions pinned in the supplied dependency lockfiles and build records.

**Production packaging uses `scripts/build-production.ps1` and the release-specific NSIS generation procedure.** A generic Tauri bundle or the older `build-release.ps1` command is not a substitute for the verified v1.13.0 packaging workflow.

Private updater-signing keys are not included in the repository or source archive. A newly compiled package is a separate build and requires its own testing, security checks, and authorized signing before distribution.

Historical build-time documents are retained for provenance; they are not evidence that their tests were rerun for a later build.

---

Developed by **Lasse Sørensen**.

**DualFlow KVM — One keyboard. One mouse. Two PCs.**
