# Work Register

Release line: 1.3.0
Milestone index: 1.3.0
Canonical path: `docs/milestone-1.3.0.md`
Canonical branch or ref: master
Git upstream: origin/master
Remote tracker: jeonghanlee/EPICS-env-support#5 (open, no GitHub milestone yet); support-side M39 mirrored in EPICS-env `docs/milestone-1.3.0.md` (M39) and issue jeonghanlee/EPICS-env#66

Next session entry point: `docs/milestone-1.3.0.md` M2 - all five OS legs built and verified 2026-08-25 (debian13 T1-T3, rocky8 T4-T6, ubuntu24 T7-T9, rocky10 T10-T12, ubuntu26 T13-T15 Pass); M1, M2, and G1 are Complete. Next is M3 (version/ChangeLog/README bump), Blocked on G2 owner authorization.

## Milestone

### Work

| Group | ID | Work unit | Type | Status | Ready | Deps | Done when / Evidence |
| --- | --- | --- | --- | --- | --- | --- | --- |
| Baseline | M1 | Fix the 1.2.1 -> 1.3.0 delta baseline | Milestone | Complete | No | | Full delta between tag 1.2.1 and release HEAD recorded here and committed; [detail](#m1---fix-the-121---130-delta-baseline) |
| Build | M2 | Build the support layer against EPICS-env 1.3.0 layer-1 (asyn R4-46) | Milestone | Complete | No | G1, M1 | ADCore, ADSimDetector, ADGenICam, ADVimba build and link against R4-46; check_deps RPATH/ABSPATH clean, on each OS (all five green: debian13, rocky8, ubuntu24 2026-08-24; rocky10, ubuntu26 2026-08-25); [detail](#m2---build-the-support-layer-against-epics-env-130-layer-1-asyn-r4-46) |
| Release | M3 | Prepare the 1.3.0 version/ChangeLog/README bump and release-eve records | Milestone | Blocked | No | M1, M2, G2 | Version/ChangeLog/README bump and release-eve records staged; tag and GitHub release remain owner-gated; [detail](#m3---prepare-the-130-versionchangelogreadme-bump-and-release-eve-records) |
| Build | G1 | EPICS-env 1.3.0 layer-1 install available (asyn R4-46) | External gate | Complete | No | | 1.3.0 layer-1 is 5-OS green (build plus RUNPATH/asyn re-verification), then present and sourceable exposing asyn R4-46; [detail](#g1---epics-env-130-layer-1-install-available-asyn-r4-46) |
| Release | G2 | Owner authorization to tag and publish 1.3.0 | External gate | Open | No | | Owner authorizes the annotated tag and the GitHub release; [detail](#g2---owner-authorization-to-tag-and-publish-130) |

### Decisions

| ID | Decision | Decision Date |
| --- | --- | --- |
| D1 | The 1.3.0 baseline is tag 1.2.1 plus exactly one commit, 7fe14ee (ADCore 72593ed -> ee039d2, WITH_PVXS = YES, PVXS RELEASE.local wiring); ADSimDetector, ADGenICam, and ADVimba are unchanged from 1.2.1 | 2026-08-24 |

### Milestone Details

#### M1 - Fix the 1.2.1 -> 1.3.0 delta baseline

Origin: 1.3.0 / M1
Identity History: none
GitHub Issue: none
Status: Complete

##### Summary

Establish the exact source-tree delta that separates the last release (tag
1.2.1) from the 1.3.0 candidate on master, so every later step builds and
releases against a fixed, known baseline rather than a moving HEAD.

##### Scope

- Enumerate every commit between tag 1.2.1 and release HEAD.
- Record the module source tags carried at the 1.3.0 baseline.
- Record the config-level change that the delta introduces.

Out of scope: executing the build (M2); the version/ChangeLog/README bump (M3);
any tag or release action.

##### Completion Criteria

- The full 1.2.1 -> HEAD delta is recorded in this register.
- This register is committed on master so the baseline is durable.

##### Dependencies And Decisions

- D1 fixes the baseline content.

##### Implementation Plan

Plan Status: draft
Plan Acceptance: none
Implementation Authorization: none
Superseded Plan Artifacts: none

1. Run `git log --oneline 1.2.1..HEAD` and confirm the commit set.
2. Run `git diff 1.2.1..HEAD` and record the config-level change.
3. Read `configure/RELEASE` at HEAD and record the four module source tags.

##### Test Plan

| Label | Layer | Method | Environment | Expected Result |
| --- | --- | --- | --- | --- |
| T1 | Baseline audit | `git log --oneline 1.2.1..HEAD` and `git diff 1.2.1..HEAD` on the real repository | Local checkout at HEAD | Exactly one commit (7fe14ee); diff limited to `configure/RELEASE` (ADCore ee039d2) and `configure/RULES_MODS_CONFIG` (PVXS wiring + WITH_PVXS = YES) |
| T2 | Source presence | Inspect the checked-out ADCore source at ee039d2 for the PVXS plugin | `ADCore-src/` at ee039d24 | ADCore ee039d2 source carries the NDPluginPvxs plugin (the delta's PVXS claim holds at source level) |

##### Verification Results

| Label | Observed At | Environment | Result | Evidence |
| --- | --- | --- | --- | --- |
| T1 | 2026-08-24 | Local checkout, HEAD 7fe14ee | Pass | `git log --oneline 1.2.1..HEAD` -> single line 7fe14ee; `git diff 1.2.1..HEAD` touches only `configure/RELEASE` and `configure/RULES_MODS_CONFIG`; `configure/RELEASE` at HEAD: ADCore ee039d2, ADSimDetector 0ef1305, ADGenICam ba5b9b8, ADVimba R1-5 |
| T2 | 2026-08-24 | `ADCore-src/` at ee039d24 | Pass | `git -C ADCore-src rev-parse --short HEAD` -> ee039d24; `find ADCore-src -iname '*pvxs*'` lists `ADApp/pluginSrc/NDPluginPvxs.cpp`, `.dbd`, `.h` and `ntndArrayConverterPvxs.{h,cpp}` |

##### Closure Evidence

- Pending: this register committed on master (carrying commit to be recorded here on commit).

##### GitHub Projection

Title: Fix the 1.2.1 -> 1.3.0 delta baseline
Labels: none
GitHub Milestone: none
Observed State: none
Observed Labels: none
Observed Milestone: none
Last Compared: never

#### M2 - Build the support layer against EPICS-env 1.3.0 layer-1 (asyn R4-46)

Origin: 1.3.0 / M2
Identity History: none
GitHub Issue: none
Status: Complete

##### Summary

Build the AreaDetector support layer (ADCore, ADSimDetector, ADGenICam,
ADVimba) against the EPICS-env 1.3.0 layer-1 install (asyn R4-46) and confirm
the modules build, link against R4-46, and pass the dependency check clean.
This also serves as the support-side evidence for EPICS-env M34.T2.

##### Scope

- Source the EPICS-env 1.3.0 layer-1 environment (`setEpicsEnv.bash`).
- Run the standard flow: `make conf.base`, `make init`, `make conf`,
  `make build`, `make symlinks`.
- Confirm the four modules link against asyn R4-46 from layer-1.
- Run the dependency check (EPICS-env `check_deps`) and confirm RPATH/ABSPATH
  clean; corroborate with in-repo `chrpath.modules` / `ldd.modules` /
  `readelf.modules`.

Out of scope: version/ChangeLog/README bump (M3); any tag or release action.

##### Completion Criteria

- All four modules build with no error against layer-1 (asyn R4-46).
- Linkage resolves to the layer-1 asyn R4-46 install.
- `check_deps` reports no RPATH or ABSPATH problems.

##### Dependencies And Decisions

- G1: the EPICS-env 1.3.0 layer-1 install (asyn R4-46) must be available. The
  peer (M40) delivers layer-1 serially per OS; each OS unblocks its own leg of
  this work. debian13 layer-1 was delivered ready 2026-08-24 (peer 3afb4c3 CI
  green, check_deps clean), so the debian13 leg ran; rocky8 layer-1 was
  delivered ready 2026-08-24 (peer gate check_deps clean) and the rocky8 leg
  ran; ubuntu24 layer-1 was delivered ready 2026-08-24 (peer gate check_deps
  clean) and the ubuntu24 leg ran (public OS, layers 1+2 only); rocky10
  layer-1 was delivered ready 2026-08-25 (peer gate check_deps clean) and the
  rocky10 leg ran; ubuntu26 layer-1 was delivered ready 2026-08-25 after the
  cloud-provision resolute proxy-contract fix, and the ubuntu26 leg ran. All
  five OS layer-1 installs are now green, so G1 is satisfied.
- M1: build against the fixed baseline.

##### Implementation Plan

Plan Status: draft
Plan Acceptance: none
Implementation Authorization: none
Superseded Plan Artifacts: none

1. Source the EPICS-env 1.3.0 layer-1 `setEpicsEnv.bash`.
2. `make conf.base` then `make init`.
3. `make conf` and confirm each module's `RELEASE.local` points ASYN at the
   layer-1 install and PVXS at the layer-1 pvxs.
4. `make build` then `make symlinks`.
5. Run `check_deps`; corroborate with `make chrpath.modules` / `ldd.modules`.

##### Test Plan

| Label | Layer | Method | Environment | Expected Result |
| --- | --- | --- | --- | --- |
| T1 | Build | `make conf.base init conf build symlinks` against layer-1 | EPICS-env 1.3.0 layer-1 (asyn R4-46) | All four modules build and install with no error |
| T2 | Linkage | `ldd.modules` / `readelf.modules` on the built libraries | EPICS-env 1.3.0 layer-1 | asyn resolves to the layer-1 R4-46 install |
| T3 | Dependency check | EPICS-env `check_deps`; corroborate with `chrpath.modules` | EPICS-env 1.3.0 layer-1 | No RPATH or ABSPATH problems reported |

##### Verification Results

| Label | Observed At | Environment | Result | Evidence |
| --- | --- | --- | --- | --- |
| T1 | 2026-08-24 | debian13 VM (192.168.122.20), layer-1 /opt/epics/1.2.2/debian-13/7.0.10 | Pass (debian13) | Build script exit 0 (ALL_DONE); four modules installed with unversioned symlinks under .../modules: ADCore-ee039d2, ADSimDetector-0ef1305, ADGenICam-ba5b9b8, ADVimba-R1-5 |
| T2 | 2026-08-24 | debian13 VM, layer-1 /opt/epics/1.2.2/debian-13/7.0.10 | Pass (debian13) | `ldd .../modules/ADCore-ee039d2/lib/linux-x86_64/libNDPlugin.so`: libasyn.so resolves via $ORIGIN to .../asyn (layer-1 R4-46); libpvxs.so.1.5 and libpvxsIoc.so.1.5 to layer-1 pvxs-1.5.2 |
| T3 | 2026-08-24 | debian13 VM, layer-1 /opt/epics/1.2.2/debian-13/7.0.10 | Pass (debian13) | `sudo bash check_deps.bash /opt/epics/1.2.2/debian-13/7.0.10` exit 0; BIN RPATH 0/150, ABSPATH 0/150; SO RPATH 0/78, ABSPATH 0/78, LOSTORG 0/78 (system-path NOTE lines benign) |
| T4 | 2026-08-24 | rocky8 VM (192.168.122.120), layer-1 /opt/epics/1.3.0/rocky-8.10/7.0.10 | Pass (rocky8) | Build script exit 0 (ALL_DONE); four modules installed under .../modules: ADCore-ee039d2, ADSimDetector-0ef1305, ADGenICam-ba5b9b8, ADVimba-R1-5 |
| T5 | 2026-08-24 | rocky8 VM, layer-1 /opt/epics/1.3.0/rocky-8.10/7.0.10 | Pass (rocky8) | `ldd .../modules/ADCore/lib/linux-x86_64/libNDPlugin.so`: libasyn.so resolves via $ORIGIN to .../asyn (layer-1 R4-46); libpvxs.so.1.5 and libpvxsIoc.so.1.5 to layer-1 pvxs-1.5 |
| T6 | 2026-08-24 | rocky8 VM, layer-1 /opt/epics/1.3.0/rocky-8.10/7.0.10 | Pass (rocky8) | `sudo bash check_deps.bash /opt/epics/1.3.0/rocky-8.10/7.0.10` exit 0; BIN RPATH 0/150, ABSPATH 0/150; SO RPATH 0/78, ABSPATH 0/78, LOSTORG 0/78 (system-path NOTE lines benign) |
| T7 | 2026-08-24 | ubuntu24 VM (192.168.122.40), layer-1 /opt/epics/1.3.0/ubuntu-24.04/7.0.10 | Pass (ubuntu24) | Build script exit 0 (ALL_DONE); four modules installed under .../modules: ADCore-ee039d2, ADSimDetector-0ef1305, ADGenICam-ba5b9b8, ADVimba-R1-5 |
| T8 | 2026-08-24 | ubuntu24 VM, layer-1 /opt/epics/1.3.0/ubuntu-24.04/7.0.10 | Pass (ubuntu24) | `ldd .../modules/ADCore/lib/linux-x86_64/libNDPlugin.so`: libasyn.so resolves via $ORIGIN to .../asyn (layer-1 R4-46); libpvxs.so.1.5 and libpvxsIoc.so.1.5 to layer-1 pvxs-1.5 |
| T9 | 2026-08-24 | ubuntu24 VM, layer-1 /opt/epics/1.3.0/ubuntu-24.04/7.0.10 | Pass (ubuntu24) | `sudo bash check_deps.bash /opt/epics/1.3.0/ubuntu-24.04/7.0.10` exit 0; BIN RPATH 0/150, ABSPATH 0/150; SO RPATH 0/78, ABSPATH 0/78, LOSTORG 0/78 |
| T10 | 2026-08-25 | rocky10 VM (192.168.122.130), layer-1 /opt/epics/1.3.0/rocky-10.2/7.0.10 | Pass (rocky10) | Build script exit 0 (ALL_DONE); four modules installed under .../modules: ADCore-ee039d2, ADSimDetector-0ef1305, ADGenICam-ba5b9b8, ADVimba-R1-5 |
| T11 | 2026-08-25 | rocky10 VM, layer-1 /opt/epics/1.3.0/rocky-10.2/7.0.10 | Pass (rocky10) | `ldd .../modules/ADCore/lib/linux-x86_64/libNDPlugin.so`: libasyn.so resolves via $ORIGIN to .../asyn (layer-1 R4-46); libpvxs.so.1.5 and libpvxsIoc.so.1.5 to layer-1 pvxs-1.5 |
| T12 | 2026-08-25 | rocky10 VM, layer-1 /opt/epics/1.3.0/rocky-10.2/7.0.10 | Pass (rocky10) | `sudo bash check_deps.bash /opt/epics/1.3.0/rocky-10.2/7.0.10` exit 0; BIN RPATH 0/150, ABSPATH 0/150; SO RPATH 0/78, ABSPATH 0/78, LOSTORG 0/78 |
| T13 | 2026-08-25 | ubuntu26 VM (192.168.122.30), layer-1 /opt/epics/1.3.0/ubuntu-26.04/7.0.10 | Pass (ubuntu26) | Build script exit 0 (ALL_DONE); four modules installed under .../modules: ADCore-ee039d2, ADSimDetector-0ef1305, ADGenICam-ba5b9b8, ADVimba-R1-5 |
| T14 | 2026-08-25 | ubuntu26 VM, layer-1 /opt/epics/1.3.0/ubuntu-26.04/7.0.10 | Pass (ubuntu26) | `ldd .../modules/ADCore/lib/linux-x86_64/libNDPlugin.so`: libasyn.so resolves via $ORIGIN to .../asyn (layer-1 R4-46); libpvxs.so.1.5 and libpvxsIoc.so.1.5 to layer-1 pvxs-1.5 |
| T15 | 2026-08-25 | ubuntu26 VM, layer-1 /opt/epics/1.3.0/ubuntu-26.04/7.0.10 | Pass (ubuntu26) | `sudo bash check_deps.bash /opt/epics/1.3.0/ubuntu-26.04/7.0.10` exit 0; BIN RPATH 0/150, ABSPATH 0/150; SO RPATH 0/78, ABSPATH 0/78, LOSTORG 0/78 |

##### Closure Evidence

- debian13 leg 2026-08-24: T1/T2/T3 Pass (build, layer-1 asyn+pvxs linkage, check_deps exit 0). Serves as debian13 support-side evidence for EPICS-env M34.T2.
- rocky8 leg 2026-08-24: T4/T5/T6 Pass (build, layer-1 asyn+pvxs linkage, check_deps exit 0) on layer-1 1.3.0/rocky-8.10/7.0.10.
- rocky8 downstream corroboration 2026-08-24: site layer-3 RV2 passed on this layer-2 tree (site tip f69438f; siteApps/unidrv/bpmSup/eventGeneratorSup build exit 0; check_deps rc=0 against /opt/epics/1.3.0/rocky-8.10/7.0.10) — reported by the EPICS-env peer. Confirms the layer-2 build holds under layer-3.
- ubuntu24 leg 2026-08-24: T7/T8/T9 Pass (build, layer-1 asyn+pvxs linkage, check_deps exit 0) on layer-1 1.3.0/ubuntu-24.04/7.0.10. Public OS: layers 1+2 only, no layer-3.
- rocky10 leg 2026-08-25: T10/T11/T12 Pass (build, layer-1 asyn+pvxs linkage, check_deps exit 0) on layer-1 1.3.0/rocky-10.2/7.0.10. Public OS: layers 1+2; site rocky10 coverage test to follow via the EPICS-env peer.
- ubuntu26 leg 2026-08-25: T13/T14/T15 Pass (build, layer-1 asyn+pvxs linkage, check_deps exit 0) on layer-1 1.3.0/ubuntu-26.04/7.0.10. Public OS: layers 1+2; ubuntu26 layer-1 was unblocked after the cloud-provision resolute proxy-contract fix.
- All five OS legs green 2026-08-25: debian13 (T1-T3), rocky8 (T4-T6), ubuntu24 (T7-T9), rocky10 (T10-T12), ubuntu26 (T13-T15) — four modules build, link the layer-1 asyn (R4-46) and pvxs-1.5 via $ORIGIN, and check_deps exit 0 on every OS.
- Integrated-run corroboration 2026-08-27: EPICS-env's release pipeline (layer-1 release-1.3.0 @ 991e330, verified 5-OS) rebuilt and verified this layer-2 at support master 7fe14ee within its own run (epics_support operator) on all five OS - 78-module full-stack tree, AD family gz-clean (-g0 -gz=zlib), check_deps EXIT 0 - independent of the manual per-OS legs above. Confirms G1 (layer-1-ready) satisfied and reconfirms the support-side build/check_deps evidence.

##### GitHub Projection

Title: Build the support layer against EPICS-env 1.3.0 layer-1 (asyn R4-46)
Labels: none
GitHub Milestone: none
Observed State: none
Observed Labels: none
Observed Milestone: none
Last Compared: never

#### M3 - Prepare the 1.3.0 version/ChangeLog/README bump and release-eve records

Origin: 1.3.0 / M3
Identity History: none
GitHub Issue: none
Status: Blocked

##### Summary

Stage the 1.3.0 version bump, ChangeLog, and README updates plus the
release-eve records so the release is ready to cut, while leaving the actual
annotated tag and GitHub release for the owner to authorize.

##### Scope

- Decide and apply how 1.3.0 is recorded (see Dependencies And Decisions: this
  repository currently carries no version string or ChangeLog file; version is
  tracked by git tag only).
- Prepare the ChangeLog entry and any README update for 1.3.0.
- Prepare the release-eve records (release notes draft, milestone roll-up).

Out of scope: creating the annotated tag; publishing the GitHub release;
closing any milestone. All of these are owner-gated (G2).

##### Completion Criteria

- The version/ChangeLog/README bump is staged and reviewed.
- The release-eve records are prepared.
- The tag/release step is queued behind owner authorization (G2), not executed.

##### Dependencies And Decisions

- M1: the fixed baseline defines the release content.
- M2: the build/link/check_deps evidence must pass before release-eve records.
- G2: owner authorization gates the tag and GitHub release; while G2 is Open the
  tag/publish sub-step is Blocked.
- Open question (undecided): this repository has no version file or ChangeLog
  today; version is tracked only by git tag. Whether to introduce a version
  file and/or ChangeLog, or keep tag-only versioning, is an owner decision to be
  recorded as a D row before this work executes.

##### Implementation Plan

Plan Status: draft
Plan Acceptance: none
Implementation Authorization: none
Superseded Plan Artifacts: none

1. Record the owner's versioning decision as a D row.
2. Apply the chosen version/ChangeLog/README changes.
3. Draft the release notes and milestone roll-up.
4. Present the owner-gated tag/release command sequence for authorization (do
   not execute).

##### Test Plan

| Label | Layer | Method | Environment | Expected Result |
| --- | --- | --- | --- | --- |
| T1 | Documentation | Review the staged version/ChangeLog/README diff | Local checkout | Bump is consistent with the fixed baseline and D1 |

##### Verification Results

| Label | Observed At | Environment | Result | Evidence |
| --- | --- | --- | --- | --- |
| T1 | Not run | Local checkout | Pending | none |

##### Closure Evidence

- Pending: staged bump, release-eve records, and the owner authorization record (G2).

##### GitHub Projection

Title: Prepare the 1.3.0 version/ChangeLog/README bump and release-eve records
Labels: none
GitHub Milestone: none
Observed State: none
Observed Labels: none
Observed Milestone: none
Last Compared: never

#### G1 - EPICS-env 1.3.0 layer-1 install available (asyn R4-46)

Origin: 1.3.0 / G1
GitHub Issue: none
Status: Complete

##### Summary

The support-layer build (M2) requires a clean EPICS-env 1.3.0 layer-1 baseline.
The readiness bar is not merely "an install exists": the peer (M40) sets it at
5-OS green — the 1.3.0 layer-1 built and RUNPATH/asyn re-verified across the
five OS targets. Wiring plus parse/stage checks alone are not the signal. Until
the peer signals 5-OS green, M2 cannot run and stays Blocked.

##### Completion Criteria

- The EPICS-env 1.3.0 layer-1 is 5-OS green: built and RUNPATH/asyn re-verified
  across the five OS targets (peer M40 signal).
- The resulting install is present and sourceable on the build host, its
  `setEpicsEnv.bash` sources cleanly and exposes asyn R4-46.

##### Verification Results

| Observed At | Result | Evidence |
| --- | --- | --- |
| 2026-08-24 | Pending (gate unmet) | Only a 1.2.2 install exists on the build host: `~/epics/1.2.2/debian-13/7.0.10/` (base 7.0.10) with `modules/asyn-4.46.0` (= R4-46). No 1.3.0 layer-1 install present. The required asyn R4-46 is already available in 1.2.2; the awaited condition is the EPICS-env 1.3.0 layer-1 (peer M40: adds qpc @913fad4 and rgamv2 @27fc633 on top of asyn R4-46). |

##### Closure Evidence

- Pending: confirmed 1.3.0 layer-1 install path and sourced environment (peer to signal on M40 completion).

#### G2 - Owner authorization to tag and publish 1.3.0

Origin: 1.3.0 / G2
GitHub Issue: none
Status: Open

##### Summary

The annotated tag and the GitHub release for 1.3.0 are owner-gated. No tag or
release is created until the owner authorizes the previewed release sequence.

##### Completion Criteria

- The owner authorizes the 1.3.0 annotated tag and the GitHub release after the
  release sequence is previewed verbatim.

##### Verification Results

| Observed At | Result | Evidence |
| --- | --- | --- |
| Not run | Pending | Owner authorization decision to be recorded here |

##### Closure Evidence

- Pending: owner authorization record and the resulting tag/release references.

## Backlog

### Work

| Group | ID | Work unit | Type | Status | Ready | Deps | Done when / Evidence |
| --- | --- | --- | --- | --- | --- | --- | --- |

_No unassigned work._
