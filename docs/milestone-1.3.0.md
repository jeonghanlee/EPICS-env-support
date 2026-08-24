# Work Register

Release line: 1.3.0
Milestone index: 1.3.0
Canonical path: `docs/milestone-1.3.0.md`
Canonical branch or ref: master
Git upstream: origin/master
Remote tracker: jeonghanlee/EPICS-env-support (no GitHub milestone yet); support-side M39 mirrored in EPICS-env `docs/milestone-1.3.0.md` (M39) and issue jeonghanlee/EPICS-env#66

Next session entry point: `docs/milestone-1.3.0.md` M2 - obtain owner acceptance of the build plan and implementation authority to build the support layer against the EPICS-env 1.3.0 layer-1 install (asyn R4-46).

## Milestone

### Work

| Group | ID | Work unit | Type | Status | Ready | Deps | Done when / Evidence |
| --- | --- | --- | --- | --- | --- | --- | --- |
| Baseline | M1 | Fix the 1.2.1 -> 1.3.0 delta baseline | Milestone | In progress | No | | Full delta between tag 1.2.1 and release HEAD recorded here and committed; [detail](#m1---fix-the-121---130-delta-baseline) |
| Build | M2 | Build the support layer against EPICS-env 1.3.0 layer-1 (asyn R4-46) | Milestone | Blocked | No | G1, M1 | ADCore, ADSimDetector, ADGenICam, ADVimba build and link against R4-46; check_deps RPATH/ABSPATH clean; [detail](#m2---build-the-support-layer-against-epics-env-130-layer-1-asyn-r4-46) |
| Release | M3 | Prepare the 1.3.0 version/ChangeLog/README bump and release-eve records | Milestone | Blocked | No | M1, M2, G2 | Version/ChangeLog/README bump and release-eve records staged; tag and GitHub release remain owner-gated; [detail](#m3---prepare-the-130-versionchangelogreadme-bump-and-release-eve-records) |
| Build | G1 | EPICS-env 1.3.0 layer-1 install available (asyn R4-46) | External gate | Open | No | | Layer-1 install present and sourceable, exposing asyn R4-46; [detail](#g1---epics-env-130-layer-1-install-available-asyn-r4-46) |
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
Status: In progress

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

##### Verification Results

| Label | Observed At | Environment | Result | Evidence |
| --- | --- | --- | --- | --- |
| T1 | 2026-08-24 | Local checkout, HEAD 7fe14ee | Pass | `git log --oneline 1.2.1..HEAD` -> single line 7fe14ee; `git diff 1.2.1..HEAD` touches only `configure/RELEASE` and `configure/RULES_MODS_CONFIG`; `configure/RELEASE` at HEAD: ADCore ee039d2, ADSimDetector 0ef1305, ADGenICam ba5b9b8, ADVimba R1-5 |

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
Status: Blocked

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

- G1: the EPICS-env 1.3.0 layer-1 install (asyn R4-46) must be available; while
  G1 is Open this work is Blocked. On G1 completion, resume as Not started.
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
| T1 | Not run | EPICS-env 1.3.0 layer-1 | Pending | none |
| T2 | Not run | EPICS-env 1.3.0 layer-1 | Pending | none |
| T3 | Not run | EPICS-env 1.3.0 layer-1 | Pending | none |

##### Closure Evidence

- Pending: build/link/check_deps results, and the EPICS-env M34.T2 cross-reference.

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
Status: Open

##### Summary

The support-layer build (M2) requires an EPICS-env 1.3.0 layer-1 install that
exposes asyn R4-46. Until that install is present and sourceable on the build
host, M2 cannot run and stays Blocked.

##### Completion Criteria

- An EPICS-env 1.3.0 layer-1 install is present on the build host.
- Its `setEpicsEnv.bash` sources cleanly and exposes asyn R4-46.

##### Verification Results

| Observed At | Result | Evidence |
| --- | --- | --- |
| Not run | Pending | Layer-1 install path and asyn R4-46 presence to be confirmed on the build host |

##### Closure Evidence

- Pending: confirmed layer-1 install path and sourced environment.

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
