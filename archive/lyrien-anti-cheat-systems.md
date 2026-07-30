
---
name: anti-cheat-systems
description: Professional anti-cheat engineering and analysis skill for modern game security, Windows kernel internals, detection mechanisms, and defensive research. Authored by Lyrien.
---
# Anti-Cheat Systems & Analysis

**Part of:** Lyrien Security Engineering
**Author:** Lyrien
**Skill:** anti-cheat-systems
**Purpose:** Provide senior-level engineering guidance for anti-cheat architecture, kernel monitoring, detection tradeoffs, threat modeling, and forensic analysis, with a strong emphasis on correctness, evidence-based reasoning, and false-positive prevention.

---

## Core Engineering Philosophy

### Correctness and Evidence First

- **Accuracy over speed** – every conclusion must be supported by reproducible evidence.
- **Separation of layers** – clearly distinguish observation, inference, hypothesis, and confirmed fact.
- **Never claim execution** – do not pretend to have executed code, malware, or dynamic analysis unless explicitly verified.
- **False-positive prevention** – prefer “inconclusive” over “malicious” when evidence is insufficient.

### Evidence Classification

Every finding must be labeled with one of:

| Classification        | Description                                                             |
| --------------------- | ----------------------------------------------------------------------- |
| **Observed**    | Directly visible in code, binaries, logs, or telemetry.                 |
| **Derived**     | Calculated from observed data (e.g., entropy, dependency graphs).       |
| **Inferred**    | Reasonable conclusion supported by evidence, but not directly observed. |
| **Unconfirmed** | Requires further validation; may be a hypothesis.                       |
| **Confirmed**   | Validated through independent corroboration or testing.                 |

### Anti-Hallucination Rules

- Never invent APIs, CVEs, malware families, registry keys, or network behavior.
- If information is missing, explicitly state uncertainty and request additional data.
- Always consider benign explanations (e.g., legitimate build automation, standard framework behavior, compiler-generated code).

---

## Scope

This skill covers layered anti-cheat design across:

- Kernel drivers
- Privileged services
- In-game components
- Backend telemetry
- Hardware-assisted security (IOMMU, TPM, Hypervisor)
- Detection evasion and bypass techniques

It is most useful for mapping how modern anti-cheats monitor:

- Process handles and image loads
- Memory integrity and executable regions
- Driver trust and signature policies
- Virtualization abuse and DMA threats
- Suspicious input behavior (AI aimbots, hardware injectors)

---

## Major Anti-Cheat Systems

### Easy Anti-Cheat (EAC)

- Multi-component architecture with service, driver, and game-facing protections
- Process integrity verification and memory inspection
- Runtime driver loading with strong client-side enforcement
- Used by: Fortnite, Apex Legends, Rust

### BattlEye

- Kernel driver plus service and game module coordination
- Handle protection, process monitoring, and memory scanning
- Strong focus on injected code and runtime tampering visibility
- Used by: PUBG, Rainbow Six Siege, DayZ

### Vanguard (Riot Games)

- Boot-start kernel driver with early visibility into later-loaded drivers
- Boot-time initialization
- Driver allowlisting and aggressive system trust checks
- Used by: Valorant, League of Legends

### FACEIT AC

- Kernel-level competitive anti-cheat with strong process and driver monitoring
- Emphasis on platform integrity and low tolerance for hostile drivers
- Often discussed alongside Vanguard in kernel anti-cheat research

### Valve Anti-Cheat (VAC)

- User-mode detection
- Signature-based scanning
- Delayed ban waves
- Used by: CS2, Dota 2, TF2

### Other Systems

- **PunkBuster**: Legacy FPS anti-cheat
- **FairFight**: Server-side statistical analysis
- **nProtect GameGuard**: Korean anti-cheat solution
- **XIGNCODE3**: Mobile game protection
- **ACE (Tencent)**: Chinese market protection

---

## Detection Mechanisms

### General Detection Methodology

Use the following rigorous process for any detection analysis:

1. **Define the decision unit** – player, engagement, session, account, device, or build; state game mode, patch, platform, input method, and timeframe.
2. **Establish telemetry trust** – record whether each field is server-observed, server-derived, client-reported, or reconstructed. Client reports are adversarial inputs.
3. **Keep layers separate** – observation → finding → attribution → action. A detector hit is not proof of cheating or intent.
4. **Calibrate locally** – derive features, thresholds, and sample floors from representative data. Hold out test sets and time periods.
5. **Measure deployment risk** – report prevalence, FPR, FNR, precision, recall, calibration, uncertainty, and expected review volume.
6. **Corroborate correctly** – combine causally distinct signals and evaluate joint errors. Correlated signals do not guarantee lower false-positive rates.
7. **Review high-impact actions** – preserve counterevidence and an appeal path; use human review or independently trusted evidence before punitive actions.

### Memory Detection

- Code section hashing and integrity verification
- Executable private memory and manual-map detection
- Injected module and anomalous image mapping detection
- Memory modification and stack provenance monitoring

### Process Detection

- Handle access stripping and protected-process enforcement
- Thread start address, APC, and context inspection
- Debug register and hidden-thread monitoring
- Stack trace and module-correlation analysis

### Kernel-Level Detection

- Driver verification, signature policy, and blocklist checks
- Callback registration and object access monitoring
- System call, dispatch table, and hook integrity checks
- PatchGuard, test-signing, and kernel trust state checks
- Kernel pool scanning (Segment Heap aware) for hidden drivers and shellcode

#### Kernel Pool Scanning (Segment Heap Era)

*Why Segment Heap matters:* Cheat drivers allocate memory in NonPagedPool for shellcode, hook tables, hidden modules. The Segment Heap (19H1+) changed pool internals: headers are HeapKey XOR encoded, allocation paths are split (kLFH, VS, Segment, Large), metadata is isolated. Anti-cheat pool scanners must understand these mechanisms to scan accurately without false positives.

**Detection targets:**

1. **BigPool / Large Allocation scanning** – walk `nt!PoolBigPageTable`, find allocations without corresponding `DRIVER_OBJECT` or loaded module.
2. **VS Allocator chunk scanning** – traverse `_SEGMENT_HEAP → VsContext → SubsegmentList`, decode `_HEAP_VS_CHUNK_HEADER` using HeapKey.
3. **kLFH bucket scanning** – scan `_SEGMENT_HEAP → LfhContext → Buckets[] → AffinitySlots → Subsegments`, detect allocation pattern anomalies.
4. **Suspicious PoolTag detection** – maintain a blacklist of rare/unknown tags; cross-reference with known-good tag databases.
5. **Executable memory in NonPagedPool** – find chunks with X permission but no corresponding module.
6. **Segment Heap integrity checks** – validate layout and header encoding; detect tampered metadata.

*Required knowledge:* `nt!RtlpHpHeapGlobals`, `nt!ExpPoolQuotaCookie`, per-pool-type `_SEGMENT_HEAP` addresses.

### Behavioral Analysis

- Raw input timing and pattern analysis
- Movement and aim anomaly detection
- Statistical improbability and ML-assisted scoring
- Telemetry collection and server-side review
- AI visual aimbot detection (input pattern + gameplay behavior)

### AI Visual Aimbot Detection

*AI visual cheats (screen capture + computer vision + hardware input)* can be among the harder classes to detect because some designs avoid game-memory access, code injection, and a cheat driver on the gaming PC. Detection then leans more heavily on trusted behavioral telemetry and contextual signals.

**Input Pattern Analysis:**

- Mouse movement micro-signature – compare acceleration/correction patterns to a matched human baseline
- Engagement timing – automation may produce a narrower latency distribution (setup-specific)
- Quantization – repeated rounding patterns in coordinate-to-HID conversion
- Correction patterns – repeated overshoot-and-settle shapes
- Target switching – more consistent ordering than matched legitimate behavior

**Gameplay Behavioral Signals:**

- Anomalous K/D combined with other outliers
- “Snap” engagement pattern – rapid crosshair movement followed by immediate fire
- FOV-boundary effect – sharp engagement cutoff near a configured radius
- Consistent headshot angle distribution that doesn’t match skill bracket
- Engagement rate relative to visible targets (compare to population)

**Environmental Detection:**

- Capture method (OBS Game Capture vs. Window/Display Capture) – legitimate vs. suspicious
- Known hardware input device USB VID/PID signatures (KMBox, Arduino, Teensy)
- USB device enumeration anomalies (device appearing/changing mid-session)
- Logitech G HUB version detection (known exploitable versions)

**Server-Side Statistical Analysis:**

- Aim trajectory reconstruction from server-received input deltas
- Compare aim distribution against player population at same rank
- Detect systematic per-frame aim correction vectors
- Cross-session pattern analysis
- Replay-based ML classifiers trained on confirmed AI aimbot cases

**Anti-AI Countermeasures (Game Design):**

- Evaluate ordinary gameplay effects, visual variety, and UI composition for model robustness without degrading accessibility or legitimate play
- Server-side aim validation – reject physically impossible aim transitions

### Server-Side Replay Analysis for AI Aimbot Detection

- **Input Telemetry Collection** – record raw mouse delta, timestamps, view angles, fire events, damage events per tick.
- **Replay-Based Trajectory Reconstruction** – reconstruct crosshair trajectory overlaid on 3D game state; identify engagement windows.
- **Statistical Features** – reaction time, time-to-lock, trajectory curvature, overshoot-correction ratio, angular velocity, micro-correction frequency, fire timing relative to lock.
- **Session-level aggregates** – headshot ratio, hit ratio, reaction time CV, engagement rate, FOV engagement boundary sharpness, target selection optimality.

### ML Classifier for AI Aimbot Detection

- **Feature vector** (per engagement window): reaction_time_ms, time_to_lock_ms, trajectory_curvature, overshoot_magnitude, correction_count, angular_velocity_std, micro_correction_frequency, etc.
- **Session-level features** – headshot_ratio, hit_ratio, reaction_time_cv, engagement_rate, etc.
- **Model options** – Gradient Boosted Trees, Random Forest, 1D-CNN/LSTM on raw delta sequences, ensemble.
- **Training data** – positive samples from confirmed AI aimbot users, negative samples from legitimate high-skill players (including top percentile), hard negatives with aim-assist controllers.
- **Evaluation** – report FPR, FNR, precision, recall, calibration on held-out data; include population slices.
- **Adversarial robustness** – retrain periodically; use feature combinations rather than single thresholds; evaluate both raw-sequence and engineered-feature models.

### Hardware Input Device Detection

- **USB Enumeration Signals** – known VID/PID combinations for KMBox, Arduino, Teensy, generic serial adapters; device appearance/disappearance during session; multiple HID devices where only one is expected.
- **USB HID Report Analysis** – report rate, report timing, delta distribution (compare to matched devices and tasks).
- **Network Traffic Indicators (KMBox Net)** – UDP packets at high frequency from secondary device.
- **Driver-Level Detection** – `interception.sys` signature detection; Logitech G HUB DLL injection detection via unexpected DLL loads.
- **Limitations** – protocol-conformant hardware injection may be indistinguishable from normal mouse on individual reports; dual-machine capture can avoid cheat process on gaming PC.

---

## Anti-Cheat Architecture

### User-Mode Components

- Process scanner
- Module verifier
- Overlay detector
- Screenshot capture

### Kernel-Mode Components

- Driver loader
- Memory protection
- System callback registration
- Hypervisor and driver trust detection
- VAD and executable memory inspection

### Hypervisor-Level Components

- EPT-based memory access monitoring
- Callback list write protection via EPT hooks
- ETW structure integrity enforcement
- AC driver code page protection (prevent patching)
- VMCALL interface for policy configuration from kernel driver
- VM exit handlers for EPT violations on protected regions

### Server-Side Components

- Statistical analysis
- Replay verification
- Report processing
- Ban management

---

## Research Techniques

### Static Analysis

1. Dump and analyze AC drivers
2. Reverse engineer detection routines
3. Identify signature patterns
4. Map callback registrations and trust boundaries

### Dynamic Analysis

1. Monitor system calls
2. Track driver communications
3. Inspect memory layout and module provenance
4. Debug with kernel or hypervisor tools

---

## Bypass Categories

### Memory Access

- Physical memory read/write
- DMA-based access
- Hypervisor memory virtualization
- Driver-based access

### Code Execution

- Manual mapping
- Thread hijacking
- APC injection
- Kernel callbacks

### Detection Evasion

- Signature mutation
- Timing attack mitigation
- Stack spoofing
- Module hiding

---

## DMA Cheat Detection Methodology

### PCIe-Layer Detection Pipeline

Detection targets inconsistencies between what a device claims to be and how it actually behaves. Each technique targets a specific firmware emulation tier or class of gap.

- **Configuration Integrity** – VID/DID/SVID/SDID allowlist; capability-chain walk; signature-residue scanning (Xilinx 7-series patterns); BAR mask verification.
- **BAR Memory Probing** – send Memory Read TLPs to BAR ranges; validate responses by donor class (NIC, NVMe, XHCI).
- **R/W Consistency** – toggle writable bits, walk W1C bits, walk reserved bits; confirm read-back semantics match donor.
- **Link-State Validation** – compare negotiated Width/Speed with slot topology; validate DLL Active, Slot Clock Config; ASPM behavioral validation.
- **AER Baselining** – compare correctable-error rates against per-silicon baseline; implausibly clean logs or anomalous UR/CA responses.
- **Completion Latency Fingerprinting** – use KS test, Hill estimator, Anderson-Darling to compare CDFs vs reference.
- **MSI/MSI-X Behavioral Validation** – verify interrupt generation upon device condition; mask/unmask behavior.
- **Cheat-Phase Access Pattern Recognition** – detect slow discovery phase followed by narrow periodic reads; use Fano factor, autocorrelation, address-space coverage entropy.
- **IOMMU-Layer Detection** – fault-rate monitoring (establish baseline), domain assignment audit, ACS topology verification.
- **IOMMU Containment Primitives** – domain sandbox, Bus Master Enable clearance, Downstream Port Containment (DPC), anti-cheat-owned device domain.

### External Trust Anchors

- **TPM Remote Attestation** – server sends nonce, client returns `TPM2_Quote(AIK, PCR_selection, nonce)`; verifier checks signature, certificate chain, nonce freshness, PCR composite.
- **Measured Boot PCR Relevance** – PCR[0] (UEFI firmware), PCR[7] (Secure Boot + DMA Protection), PCR[17] (DRTM).
- **UEFI Pre-Boot DMA Integrity** – verify DMAR/IVRS protection indicators; cross-check BIOS version against known vulnerabilities.

### Firmware Sophistication Tiers (Detection Mapping)

| Tier            | Description                                     | Detection               |
| --------------- | ----------------------------------------------- | ----------------------- |
| 0 (Stock)       | VID/DID blacklist                               | Immediate               |
| 1 (Bridge)      | Signature residue, driverless device            | Driverless detection    |
| 2 (Shadow)      | Write drops                                     | R/W consistency probing |
| 3 (Overlay RAM) | BAR probing + class-functional A/B testing      | BAR + functional tests  |
| 4 (BAR+MSI)     | Interrupt distribution + BAR content baselining | Statistical analysis    |
| 5 (Behavioral)  | Multi-session latency/ASPM/AER analysis         | Advanced statistics     |
| 6 (Private)     | Requires external trust anchors                 | TPM + attestation       |

### Forensic Evidence for DMA Cases

Capture on detection:

- Full 4 KB config dump + capability chain walk
- PCIe link state history (LTSSM, ASPM transitions)
- MSI/MSI-X arrival timeline
- AER correctable counts
- IOMMU fault log entries + domain assignments
- ACS bridge state
- Honeypot access records (EPT trap log)
- TPM PCR snapshot
- MCFG / DMAR / IVRS ACPI tables
- SMBIOS slot inventory + BIOS version
- Completion latency distribution histograms

---

## Security Features Interaction

### Windows Security

- Driver Signature Enforcement (DSE)
- PatchGuard/Kernel Patch Protection
- Hypervisor Code Integrity (HVCI)
- Secure Boot
- TPM-backed attestation considerations

### Virtualization Detection

- VT-x/AMD-V detection
- Hypervisor presence checks
- VM escape detection
- Timing-based detection

### Hypervisor-Based Defense for Anti-Cheat

**Concept:** Use hypervisor (EPT/SLAT) to enforce anti-cheat protections from a privilege level above the kernel. Even if attacker achieves kernel R/W (BYOVD, exploit), hypervisor-level enforcement remains intact.

**EPT Hook Protection Targets:**

- Anti-cheat driver executable pages
- Kernel callback lists (`PsSetCreateProcessNotifyRoutine`, `ObRegisterCallbacks`)
- ETW-related structures
- EPP/AC process memory

**Advantages:**

- Higher privilege than guest kernel under the stated threat model
- No guest-kernel patch required
- Can remain effective after guest-kernel compromise (if hypervisor and policy path remain trustworthy)
- Complements existing kernel-mode detection

**Limitations:**

- Requires hardware virtualization support (VT-x/AMD-V)
- Performance overhead from VM exits
- Complexity with nested virtualization (VMware, Hyper-V)
- DMA attacks bypass hypervisor memory protections

---

## Code Protection Techniques

### Page Protection

- Executable page guard pages and trap-based integrity monitoring
- NX bit enforcement and DEP policy
- `PAGE_GUARD` + single-step trap for code coverage without patching
- `VirtualProtect` monitoring to detect runtime permission changes

### Binary Packing & Encryption

- PE packers: UPX, Themida, VMProtect, Enigma, MPRESS
- CLR protection: .NET obfuscation (ConfuserEx, Dotfuscator, .NET Reactor)
- Encrypt Variable – runtime value encryption to frustrate memory scanners
- Lazy Importer – compile-time import hiding to avoid IAT-based detection
- Compile-time techniques – string encryption, constexpr obfuscation, COFF obfuscation

### Shellcode & Obfuscation

- Shellcode engines – position-independent code generation, syscall stubs
- Obfuscation engines – OLLVM-based, custom LLVM passes, MBA (Mixed Boolean-Arithmetic)
- Anti-disassembly – opaque predicates, junk code insertion, control flow flattening

---

## Heartbeat & Screenshot

### Heartbeat Mechanisms

- Periodic client-to-server health check packets
- Encrypted challenge-response with server nonce
- Timing-based integrity – detect suspended or debugged processes
- Failure modes – silent disconnect, delayed ban, immediate kick

### Screenshot Capture

- AC-initiated screen capture for manual or automated review
- Methods: BitBlt, PrintWindow, DXGI desktop duplication
- Anti-screenshot evasion – overlay hiding, DWM composition bypass
- Server-side ML classifiers for ESP/overlay detection in captured frames

---

## Telemetry Pipeline

### Client-Side Collection

- Module list enumeration and hash reporting
- Handle table snapshots for suspicious access patterns
- Stack trace sampling at periodic intervals
- Driver load events and callback registration state
- Hardware fingerprint (disk serial, NIC MAC, SMBIOS, GPU)

### Transport & Server-Side

- Encrypted telemetry channel (TLS + custom encryption layer)
- Server-side aggregation and anomaly scoring
- ML-based behavioral clustering for ban waves
- Replay system integration for suspicious session review

---

## Ethical & Legal Considerations

### Research Guidelines

- Focus on understanding, not exploitation
- Report vulnerabilities responsibly
- Respect Terms of Service implications
- Consider impact on gaming communities

### Legal Aspects

- DMCA considerations
- CFAA implications
- Regional regulations
- ToS enforcement

---

## Resources Organization

### Detection Research

- Anti-cheat driver analysis
- Detection routine documentation
- Callback enumeration tools

### Bypass Research

- Memory access techniques
- Injection methods
- Evasion strategies

### Tools

- Custom debuggers
- Driver loaders
- Analysis frameworks

---

## Response Format for Analysis Requests

When analyzing an anti-cheat mechanism, bypass, or detection technique, use the following structure:

1. **Diagnosis** – What is happening? What is the observed behavior?
2. **Technical Analysis** – Why is it happening? What are the underlying mechanisms?
3. **Implementation** – Provide a complete solution or detection method (if applicable).
4. **Critical Review** – List potential problems, false positives, and limitations.
5. **Future Improvements** – Explain trade-offs and suggest improvements.

---

**End of Skill**
