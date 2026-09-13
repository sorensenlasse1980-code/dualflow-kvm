# DualFlow KVM — Validation & Release Quality

DualFlow KVM is designed for reliable, low-latency control of two Windows 11 PCs using one physical keyboard and mouse.

Because a software KVM interacts with input handling, Windows security boundaries, networking, clipboard data, file transfers, system services, and software updates, every production release is validated across multiple layers before it is considered ready.

This document describes the general validation and release-quality process used for DualFlow KVM.

For installation and everyday use, see the [User Guide](USER_GUIDE.md).

---

## Validation Principles

DualFlow releases are validated with a combination of:

- Automated Rust tests
- Frontend and TypeScript checks
- Production builds
- Windows installer lifecycle tests
- Update and signature verification
- Protocol and compatibility tests
- Regression protection
- Real dual-PC testing on Windows 11
- Manual validation of workflows that cannot be fully reproduced by automated tests

Automated tests are used wherever behavior can be tested deterministically.

Real-machine testing is required for functionality that depends on Windows desktop sessions, physical input devices, Secure Desktop, UAC, networking, system services, Explorer, or the complete software-update lifecycle.

A successful build alone is not considered sufficient release validation.

---

## Automated Validation

The automated test suite covers core DualFlow behavior and release-critical components.

Validation includes areas such as:

- Host and Client state handling
- Network protocol behavior
- Authenticated peer communication
- Input-related state transitions
- Display and layout logic
- Clipboard handling
- File and folder transfer logic
- Update coordination
- Version and capability handling
- Error handling
- Recovery behavior
- Installer and update lifecycle
- Post-update relaunch behavior

All required automated tests must pass before a release proceeds to final acceptance testing.

Existing ignored or platform-specific tests are tracked separately and must not be silently treated as successful tests.

---

## Build Validation

Every production release is built using the established DualFlow release process.

The release process verifies:

- Rust release compilation
- TypeScript compilation
- Production frontend build
- Windows application packaging
- NSIS installer generation
- Application version consistency
- Installer version consistency
- Update metadata consistency

Release artifacts must be generated from the validated source tree.

Unexpected source changes discovered during the release process must be reviewed before publication.

---

## Regression Protection

DualFlow development follows a minimal-change principle:

> Working KVM functionality should not be changed unless the release specifically requires it.

Before release, existing functionality is checked for unintended changes.

Protected areas include:

- Mouse capture and forwarding
- Keyboard capture and forwarding
- Cursor transitions between PCs
- Emergency return to Host
- Network reconnect behavior
- Windows input-service lifecycle
- Secure Desktop and UAC support
- Display configuration
- Clipboard synchronization
- File and folder transfer
- Windows Desktop paste
- Licensing
- Diagnostics
- Startup and tray behavior
- Update lifecycle

Unrelated refactoring is avoided during narrowly scoped maintenance and feature releases.

---

## Real Dual-PC Validation

Automated tests cannot fully reproduce a real DualFlow environment.

Production releases therefore require validation using two Windows 11 PCs configured as an actual Host and Client pair.

The physical test environment validates the complete path:

**Physical keyboard and mouse → Host → encrypted local connection → Client → Windows input system**

Testing includes normal operation as well as recovery and reconnect scenarios.

---

## Mouse & Keyboard Validation

Real-machine input testing includes:

- Cursor movement on Host
- Cursor transition from Host to Client
- Cursor transition from Client to Host
- Keyboard following the active PC
- Normal typing
- Modifier keys
- Right Shift
- NumLock functionality
- Mouse buttons
- Mouse wheel
- Rapid pointer movement
- Repeated screen crossings
- Multi-monitor boundaries
- Client taskbar interaction
- Emergency return using `Pause`
- Emergency return using `Ctrl + Alt + F12`

Input testing is performed under normal real-world use rather than relying only on synthetic input generation.

---

## Secure Desktop & UAC Validation

DualFlow uses a dedicated Windows input service so that control can continue when the Client displays administrator prompts or elevated applications.

Release validation includes real Windows UAC testing.

Typical validation includes:

1. Control the Client from the Host.
2. Launch an application requiring administrator approval.
3. Confirm that Windows switches to Secure Desktop normally.
4. Verify that mouse and keyboard control remain available.
5. Test both approval and cancellation where appropriate.
6. Verify normal Client control after returning from Secure Desktop.

DualFlow must not disable, bypass, or weaken Windows UAC or Secure Desktop security.

The application must work within the Windows security model.

---

## Network & Reconnect Validation

DualFlow is designed for local network operation and automatic recovery from normal interruptions.

Real-machine validation includes:

- Initial Host/Client connection
- Automatic discovery
- Authenticated pairing
- Normal reconnect
- Client application restart
- Client PC restart
- Host application restart
- Temporary network interruption
- Physical network disconnect and reconnect
- Recovery after the Client becomes temporarily unavailable

A restored network connection alone must not be treated as proof of successful completion of operations that require additional validation.

---

## Clipboard Validation

Clipboard validation includes common productivity workflows between Host and Client.

Testing includes:

- Plain text
- Formatted text where supported
- Images
- Host → Client
- Client → Host
- Repeated copy/paste operations
- Switching PCs between copy and paste

Clipboard behavior is tested together with normal KVM switching to ensure that clipboard synchronization does not interfere with input responsiveness.

---

## File & Folder Transfer Validation

DualFlow supports Explorer-based file and folder transfer using on-demand streaming.

Real-machine validation includes:

- Individual files
- Multiple files
- Large files
- Folders
- Nested folder structures
- Empty directories
- Host → Client
- Client → Host
- Paste into File Explorer
- Paste directly onto the Windows Desktop
- Transfer completion and integrity

Unsupported Windows filesystem structures such as symbolic links and reparse points are handled according to DualFlow's documented safety boundaries.

Large transfers are tested while normal KVM input remains active.

---

## Display & Cursor Boundary Validation

Display arrangements are validated using the Host-controlled display canvas.

Testing includes:

- Side-by-side displays
- Stacked displays
- Different resolutions
- Multi-monitor Host configurations
- Multi-monitor Client configurations
- Ultrawide layouts
- Shared screen edges
- Non-connected screen edges
- Repeated cursor crossings

The on-screen arrangement must correspond to the physical display layout so that transitions occur only across valid shared boundaries.

---

## Performance Validation

DualFlow is designed to keep input forwarding responsive while avoiding unnecessary CPU, memory, and network overhead.

Performance validation can include:

- Input responsiveness
- Application round-trip measurements
- Polling behavior
- Processing-time measurements
- Network throughput
- Large file-transfer behavior
- CPU usage
- Memory usage
- Stability during sustained operation

Performance measurements are interpreted according to what they actually measure.

For example, application round-trip time is not presented as synchronized one-way physical input latency.

Benchmark results should identify the environment and software version used for the measurement.

---

## Software Update Validation

DualFlow uses signed update packages.

Update validation includes:

- Update discovery
- Version comparison
- Release metadata validation
- Installer download
- Updater signature verification
- Rejection of invalid or corrupted update packages
- Installer lifecycle
- Application shutdown
- Installation
- Relaunch
- Version verification
- Existing post-update confirmation behavior

After a successful in-app update, DualFlow must report the version actually running rather than relying only on the version requested before installation.

---

## Coordinated Update Validation

Coordinator-capable DualFlow releases can update both paired PCs from the Host.

The intended update sequence is:

1. Host detects a compatible update.
2. User chooses **Update both PCs**.
3. Client independently downloads and verifies its signed update package.
4. Client installs the update.
5. Client relaunches and reconnects.
6. Host verifies the authenticated Client identity.
7. Host verifies the Client's actual running version.
8. Host verifies required Client input-helper readiness.
9. Only after successful Client verification does the Host begin its own update.
10. Host installs and relaunches.
11. Existing post-update UI confirms the version actually running.

The Client is intentionally updated first.

A re-established TCP connection alone is not sufficient proof of a successful Client update.

If required Client validation fails, the Host must remain on its existing version rather than blindly continuing the coordinated update.

Each PC downloads and validates its own update package. The Host does not act as an installer-binary relay.

---

## Update Security & Integrity

DualFlow update packages use the configured updater-signing mechanism.

Release validation verifies that:

- The expected installer is referenced
- The update metadata matches the intended release
- The detached updater signature matches the installer
- The signature validates against the configured DualFlow updater public key
- Modified or corrupted packages are rejected
- Version and compatibility information is validated before coordinated installation

SHA-256 checksums may also be published as an additional integrity reference.

> **Important**
>
> SHA-256 checksums are integrity references, not digital signatures.
>
> DualFlow's updater signing mechanism is also separate from Windows Authenticode code signing.

Private signing keys are never included in public release artifacts.

---

## Release Artifacts

A standard DualFlow GitHub release contains the established production update assets:

```text
DualFlow-KVM-vX.Y.Z-setup.exe
DualFlow-KVM-vX.Y.Z-setup.exe.sig
latest.json
SHA256SUMS.txt
