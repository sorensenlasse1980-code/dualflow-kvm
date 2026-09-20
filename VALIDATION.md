# DualFlow KVM — Validation & Release Quality

**Release: v1.13.0 · Windows 11 x64**  
**Validation summary updated: 20 September 2026**

DualFlow KVM shares a keyboard and mouse between two Windows PCs. Its operation involves Windows input handling, desktop sessions, privileged services, networking, clipboard data, file transfers, and software updates.

Release acceptance therefore uses several kinds of evidence: automated tests, build and package inspection, real two-PC testing, and controlled installation lifecycle checks.

**A successful build alone is not considered sufficient validation.**

For installation and everyday use, see the [User Guide](USER_GUIDE.md).

---

## Scope of This Document

This document records the validation basis for the final v1.13.0 installer.

Acceptance combines:

- Earlier real two-PC qualification of the application functionality.
- Targeted FQ4 validation of the final Drag & Drop correction.
- Automated regression tests for the final production packaging.
- Exact-package Client installation, uninstall, and reinstall testing.
- Subsequent operator-reported Host update to v1.13.0, with Client already on v1.13.0, followed by restored Linked status and immediate normal operation.
- Signature, integrity, and Microsoft Defender checks.

It does **not** claim that every historical test was repeated against every subsequent installer.

### How Results Are Described

| Description | Meaning |
| --- | --- |
| **Automated PASS** | The recorded automated check completed successfully within its stated scope. |
| **Operator-reported PASS** | The physical operator confirmed the requested behavior on real hardware. |
| **Independent observation** | Read-only inspection verified an observable state, file identity, or recorded result. |
| **Historical evidence** | A result belongs to an earlier identified build or update path. |
| **NOT RUN / not repeated** | The scenario was not executed in the stated test round. |
| **FAIL** | The original failure remains recorded, even when a later correction passes. |

A log entry confirming an API call is not automatically proof that the corresponding Windows interaction completed successfully.

## Exact Release Identity

The final installer covered by the production-package checks is:

| Property | Value |
| --- | --- |
| Filename | `DualFlow-KVM-v1.13.0-setup.exe` |
| Version | `1.13.0` |
| Platform | Windows 11 x64 |
| Size | 4,377,664 bytes |

### Installer SHA256

```text
A65DBBF0C102C80694DDBAB139E571E4332F868953FF9557C27DFB8A85471357
```

The installed application, Shell adapter, and uninstaller were checked against the corresponding package payloads after reinstall.

The original source archive is also retained unchanged:

```text
DualFlow-KVM-v1.13.0-source.zip
SHA256: 576F58F707C56F25367387A02A270CFB9A2F707502CDF8409FFA3025A7BEF841
```

Rebuilding from the same source creates a separate artifact requiring its own identity and validation. Bit-for-bit reproducible builds are not claimed.

---

## Automated Validation Results

The final packaging correction completed the following checks:

| Area | Recorded result |
| --- | --- |
| Main Rust test suite | **284 passed, 0 failed, 5 ignored / NOT RUN** |
| Shell adapter Rust suite | **26 passed** |
| Uninstall lifecycle fixtures — x64 | **28 passed** |
| Uninstall lifecycle fixtures — x86 | **28 passed** |
| Existing update lifecycle | **12 passed** |
| Remote SAS policy and consent fixtures | **20 passed** |
| NSIS temporary-copy recovery fixture | **PASS** |
| Qualification collector | **12 passed** |
| TypeScript and production frontend build | **PASS** |
| Browser baseline | **12 groups passed** |
| UI polish and responsiveness | **18 groups passed** |
| Checklist persistence/export validation | **18 gates passed** |
| Locked offline release compilation and NSIS packaging | **PASS** |
| Package contents, versions, and source-boundary checks | **PASS** |
| Updater signature verification | **PASS** |
| Deliberately corrupted installer bytes | **Rejected** |

These counts describe different test suites and groups. They are not combined into an inflated single “tests passed” total.

The checklist tests validate checklist behavior; they do not mean physical checklist scenarios were executed.

The NSIS fixture uses inert test files. It does not substitute for uninstalling the actual product on Windows.

Two existing dead-code warnings in the frozen input-hook source remained unchanged. The final installer correction introduced no new runtime warning.

### Five Standalone Rust Tests Not Run

The following standalone invocations remain explicitly **NOT RUN**:

```text
clipboard::tests::native_desktop_accepts_virtual_offer_without_fetching_file_bytes
cursor_overlay::tests::real_window_first_presentation_is_acknowledged
helper_diagnostics::tests::panic_log_fixture
input_service::startup_native_tests::privilege_path_fixture
input_service::startup_native_tests::subprocess_exit_fixture
```

Some fixture paths are exercised by other parent tests. That does not retroactively mark these standalone invocations as executed.

## Real Two-PC Qualification

Windows input, Secure Desktop, native Explorer Drag & Drop, and complete update workflows require real-machine testing.

The physical path under test is:

**Physical Host input → authenticated local connection → Client input service / Windows session → target application**

### Qualification History

| Stage | Recorded result and scope |
| --- | --- |
| FQ3 FINAL | Original 225-point result: **223 PASS / 2 FAIL**. The original failures remain preserved. |
| Supplementary retest | A successful Notepad-closed retest was recorded separately, without overwriting the original failure. |
| FQ4 targeted correction | **18 PASS / 0 FAIL / 0 NOT RUN**, with matching installed candidate identities. |
| Final production installer | Separate exact-package Client install, running-application uninstall, reinstall, and requested functional checks completed. |
| Subsequent Host update | Operator reported both PCs on v1.13.0, restored Linked status, and immediate normal operation. Client was already on the target version; this was not a two-installer update test. |

The complete 225-point checklist was not repeated on FQ4. Its targeted results are not represented as 225 new passes.

FQ4 was an identified qualification candidate with historical version metadata. It is not confused with the final v1.13.0 production installer.

### Input and Display Coverage

The retained qualification covers the established workflows, including:

- Pointer crossing in both directions.
- Mouse clicks, holds, and wheel input.
- Keyboard routing and ordinary typing.
- Host application/UI focus while forwarding Client input.
- Right Shift and NumLock functionality.
- Emergency return to Host.
- Shared multi-monitor boundaries and taskbar interaction.
- Live monitor removal and reinsertion.
- Resolution and scaling changes.
- Lock-screen, sign-in, elevated-window, and Secure Desktop operation.

These are retained qualification results, not a claim that every combination of hardware, Windows settings, or application was tested.

Historical intermittent pointer-jump observations and their investigation limits remain recorded. Failure to reproduce an intermittent issue is not proof that it can never recur.

## Native Drag & Drop Validation

v1.13.0 adds native file and folder **COPY** Drag & Drop between Windows Desktop and Explorer.

Targeted physical acceptance includes:

- Host Desktop and Explorer → Client destinations.
- Client Desktop and Explorer → Host destinations.
- Desktop, Explorer, and folder destination targeting.
- Ordinary applications open on Host, including Notepad and terminal windows.
- Cancellation followed by another transfer.
- Repeated transfers.
- Interruption and recovery.
- Source cleanup without manual Escape recovery.
- Normal mouse and keyboard operation after the drag.

### Completion Requires More Than a Copied File

A successful destination copy alone is not sufficient.

Validation also considers whether:

- The original source drag terminates.
- Mouse/button ownership unwinds.
- Subsequent clicks and drags work normally.
- Cancelled or stale operations do not produce unwanted later drops.

FQ4 traces corroborate source capture release and destination COPY completion. The physical assertions that Explorer’s drag loop ended and files landed at the intended destination come from the operator’s checklist.

The late source-admission callback order is covered by automated tests. The collected FQ4 physical trace showed early callback admissions; it is not presented as physical execution of the late-admission branch.

### Transfer Boundaries

The verified scope is COPY into Windows Desktop and Explorer.

Cross-PC MOVE, Shift+Drag source deletion, Cut/Paste moves, and arbitrary third-party application drop targets are not included in that acceptance.

## Clipboard and File Copy/Paste

The existing clipboard and lazy file-transfer architecture is preserved.

Qualification includes the established workflows for:

- Text and images.
- Files and folders.
- Both transfer directions.
- Desktop and Explorer paste destinations.
- Nested folder structures and empty directories.
- On-demand content fetching.
- Continued normal KVM operation around transfers.

Drag & Drop does not replace the working Copy/Paste implementation.

A received offer is not equivalent to a completed transfer. Final destination content and completion behavior matter.

---

## Windows Sign-In, UAC, and Remote Ctrl+Alt+Del

DualFlow uses its installed Windows service for supported privileged input and configured pre-login operation.

Retained physical evidence covers sign-in/lock-screen input, password entry, elevated Windows interaction, and remote secure attention.

These features do not:

- Bypass a password or PIN.
- Disable UAC or Secure Desktop.
- Automatically approve administrator prompts.
- Remove managed Windows policy restrictions.

Remote Ctrl+Alt+Del requires explicit setup permission for the supported service-only Windows SAS policy.

A successful secure-attention request is not proof that Windows displayed the security screen. Actual screen behavior requires physical observation.

## Installer, Uninstall, and Reinstall Validation

The final package includes narrowly scoped uninstall corrections:

- Correct registry command and icon quoting.
- Correct Shell executable removal.
- Installation-specific process, service, and task identity checks.
- Bounded shutdown and file-lock handling.
- Verification of required cleanup.
- An incomplete-removal result and recovery path when mandatory cleanup fails.

The actual production-consumed NSIS input was checked. Validation did not rely on changing an editable template that the final installer ignored.

### Exact-Package Client Test

On 20 September 2026, the operator performed:

1. Installation over the existing Client.
2. Requested post-install functional checks.
3. Uninstall while the Client application was running.
4. Reinstall using the same verified package.
5. Requested post-reinstall functional checks.

The targeted sequence completed successfully.

The operator reported:

- Restored **Linked** state and prior layout/pairing.
- Working mouse and keyboard.
- Copy/Paste and Drag & Drop in both directions.
- Remote Ctrl+Alt+Del.
- Quit → relaunch → Linked.

Independent read-only checks verified:

- Installed executable versions and hashes.
- Service, task, process, shortcut, and firewall identities.
- Required uninstall cleanup.
- Restored installation components after reinstall.
- Retention of the recorded configuration and pairing state.
- No application-path pending file replacement after reinstall.

Host remained unchanged during this controlled Client lifecycle test.

### Important Limits

This was a clean application reinstall **with intentionally retained configuration**, not an empty-profile or factory-reset test.

An earlier installer correction failed its physical uninstall test. That failure and successful repair remain in the historical record.

The final correction’s physical lock gates succeeded on their first attempt. Automated fixtures exercised lock retries, but the successful physical run does not establish which process caused the earlier transient lock.

## Update Validation

The updater verifies the selected package before installation and preserves the existing post-update version confirmation.

Automated and package checks cover the existing update lifecycle, metadata, signature handling, and the preserved relaunch path.

### Host-Orchestrated Updates

When Client also needs the target version, the intended sequence remains:

1. Client independently downloads and verifies its update.
2. Client installs and relaunches.
3. Client reconnects.
4. Host verifies authenticated identity, expected version and role, and required readiness.
5. Host begins its own update only after successful verification.
6. Host relaunches and displays the installed version.

A restored TCP connection alone is not proof of a successful Client update.

### Historical Physical Update Result

The real GitHub-based **v1.11.8 → v1.12.0** coordinated update was reported successful on 14 September 2026:

**Client first → reconnect → Host → installed-version confirmation**

That is historical two-PC evidence for the workflow.

That historical Client-first result is distinct from the subsequent v1.13.0 result below.

### Subsequent Physical Update Confirmation — 20 September 2026

The operator reported restarting Host, opening DualFlow, and seeing **Update available: v1.13.0**. Client was already running v1.13.0. The operator then planned to use **Review update → Update both PCs**.

Following that update discussion, the operator confirmed (translated from Norwegian):

> Both PCs have v1.13.0, linked up, and worked immediately.

This is recorded as **operator-reported PASS for the observed Host update outcome, matching running versions, reconnection, and immediate usability**.

| Observation | Result and evidence |
| --- | --- |
| Host shows v1.13.0 | **Operator-reported PASS** |
| Client shows v1.13.0 | **Operator-reported PASS**; Client was already on this version before the operation |
| Both PCs return to Linked | **Operator-reported PASS** |
| Normal operation resumes immediately | **Operator-reported PASS**; qualitative observation, not a timed latency or reconnect benchmark |
| Client downloads and reinstalls v1.13.0 | **Not required for this scenario; not claimed as tested** |
| Fresh installed Host executable hashes | **Not independently checked in this follow-up** |
| Explicit “Updated to v1.13.0” banner observation | **Not separately confirmed in the follow-up message** |

The reviewed coordinator code explicitly handles a Client already on the target version: it skips Client installation and checks the authenticated peer, matching version/operation, role, and input-service readiness before proceeding with Host.

That describes the implementation. The operator’s observed result corroborates successful use of the already-current-Client scenario; no new packet trace, installer command line, per-step timestamp, or internal-state observation is fabricated.

A fresh v1.13.0 Client download/install/restart cycle was therefore **not** exercised by this follow-up. Its result must not be described as a complete two-installer Client-first update.

The prior package hashes remain the authoritative identities from the retained evidence. This follow-up does not substitute a version string for a fresh binary-hash check.

### Remaining Scope Limits

The final packaging correction’s original Client manual round did not execute updater-specific scenarios. The later operator report adds a successful Host update outcome with an already-current Client; the original records are not rewritten.

The following distinctions still apply:

- **Silent `/UPDATE` invocation:** The new report supports the user-visible Host update outcome, but contains no new installer log or command-line evidence independently confirming this mode.
- **Post-update banner:** Running v1.13.0 is confirmed; the specific installed-version banner was not separately reported in this follow-up.
- **Complete dual-PC installation sequence to v1.13.0:** Not exercised in the follow-up because Client already had v1.13.0. The historical v1.11.8 → v1.12.0 full sequence remains separate evidence.
- **Upgrade from the genuine original v1.12.0 installer:** The exact starting Host binary was not identified in this follow-up.
- **Reboot after both PCs reached production v1.13.0:** Not confirmed by this follow-up. The reported Host reboot preceded the update.
- **Disposable-VM fault injection:** Remains **NOT RUN**.
- **Five standalone Rust tests listed above:** Remain **NOT RUN**; physical usability confirmation does not execute them.

Normal setup’s earlier Prepare/Commit/Resume checks and the later successful user-visible update are complementary evidence, not substitutes for unobserved scenarios.

---

## Security and Integrity Checks

### Updater Signature

The final installer’s existing updater signature was verified against the configured public key.

An in-memory one-byte corruption was rejected.

These checks establish signature behavior for the tested bytes. They do not establish that every possible update-failure condition was physically reproduced.

**SHA256 checksums are integrity references, not digital signatures.**

**Tauri updater signing is separate from Windows Authenticode publisher signing.** The final installer is not claimed to have Authenticode publisher signing.

### Microsoft Defender

The exact production installer was scanned on:

**20 September 2026, 09:42:09–09:42:21 UTC**

Recorded environment:

- Definitions: **1.459.301.0**
- Engine: **1.1.26080.3**
- Real-time protection enabled.
- No applicable exclusion recorded.
- Scan exit code: **0**
- No matching detection recorded.
- Installer SHA256 unchanged.

This is a point-in-time result, not a permanent antivirus guarantee or independent security certification.

A separate historical qualification installer received a Defender detection. That issue remains documented separately. It was not cleared by restoring the quarantined file, adding an exclusion, or bypassing Windows Security, and it is not described as a Microsoft-confirmed false positive.

If a later security engine flags the production package, the finding must be investigated rather than dismissed because an earlier scan passed.

### Public Package Review

Release contents were checked for expected files and accidental inclusion of private material.

The bounded source/provenance review found no private-key or live-state pattern findings in the inspected public source archive.

This is not proof against every possible encoded secret or an independent security audit.

Private signing material, live pairing/license state, and private diagnostic archives are not public release assets.

## Build and Source Preservation

The release archive preserves:

- Original application source and resources.
- Dependency lockfiles.
- Actual production packaging inputs.
- Approved installer transformations.
- Build and test records.
- Signature and checksum evidence.
- Source-to-binary provenance information.

All 200 entries in the authoritative original source inventory were verified. The 175-file frozen FQ4 snapshot was also retained and checked.

Archive consolidation performed integrity and dependency-resolution checks without rebuilding the application. Those checks are not described as a fresh successful compile.

Earlier build-time documents remain unchanged, including records written before physical acceptance. Later qualification summaries provide the subsequent results instead of rewriting history.

## Performance Measurement Boundaries

Performance observations must identify the software version and environment that produced them.

DualFlow’s on-demand benchmark reports must be interpreted within their scope:

- Application RTT includes software queues and processing.
- RTT is not synchronized one-way physical input latency.
- Capture-to-inject latency is unavailable.
- Mouse jitter describes incoming packet timing, not complete physical cursor behavior.
- Memory covers the local Rust process, not WebView2 and the input service combined.
- Benchmark sampling is inactive outside an active session.

Historical benchmark numbers remain attributed to their original versions.

No universal zero-latency, zero-CPU, zero-memory, or fixed reconnect-time guarantee is made.

## Release Assets

The v1.13.0 release package includes:

```text
DualFlow-KVM-v1.13.0-setup.exe
DualFlow-KVM-v1.13.0-setup.exe.sig
DualFlow-KVM-v1.13.0-source.zip
latest.json
SHA256SUMS.txt
RELEASE-MANIFEST.json
SOURCE-BINARY-TRACEABILITY.json
```

Validation applies to the identified bytes and documented scope. Renaming, rebuilding, modifying, or re-signing a package must not silently inherit the original artifact’s validation status.

## Release Acceptance Principle

The owner accepted the combined qualification history, final automated results, exact-package Client lifecycle checks, and integrity/security evidence as the basis for v1.13.0 release preparation. The subsequent report of both PCs running v1.13.0, reconnecting, and working immediately adds operator-reported acceptance evidence for the Host update with an already-current Client.

That acceptance does not convert omitted tests into passes or prove the absence of every possible defect.

Future releases must retain the same distinction between:

- What was tested.
- Which exact build was tested.
- What was observed independently.
- What the physical operator confirmed.
- What remains historical, unverified, or not run.

For usage instructions, see the [User Guide](USER_GUIDE.md). For downloads, see [GitHub Releases](https://github.com/sorensenlasse1980-code/dualflow-kvm/releases).
