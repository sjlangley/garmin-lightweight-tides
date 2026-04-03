# Implementation Plan

Generated on 2026-04-04
Status: DRAFT

## Delivery Principles

- Ship in small, reviewable PRs.
- Do not land product code before formatting, linting, and CI gates exist.
- Every code PR should target at least 80% test coverage for the code it
  introduces or materially changes.
- Prefer narrow vertical slices over broad scaffolding.
- Each phase should leave the app in a runnable, understandable state.
- If a feature creates ambiguity about memory, rendering cost, or device
  behavior, stop and validate in the Forerunner 55 simulator before expanding.

## Quality Gates

These gates apply before and during implementation:

- Formatting tool is configured before the first product code PR lands.
- Linting is configured before the first product code PR lands.
- CI runs formatting, lint, and tests on every PR.
- Test coverage reporting is enabled before the first product code PR lands.
- Code PRs should not merge if they reduce coverage below the agreed 80%
  threshold for the touched area.
- Simulator validation on the Forerunner 55 should be part of every UI-facing
  phase.

## Phase Plan

## Phase 0: Repo Tooling and CI

Goal: establish the quality bar before product code starts landing.

PR scope:

1. Add Connect IQ project scaffold files needed for buildability only.
1. Add formatting and lint tooling configuration.
1. Add test harness and coverage reporting setup.
1. Add CI workflow for format, lint, tests, and coverage.
1. Document local developer commands in the README.

Exit criteria:

- A contributor can run format, lint, and tests locally.
- CI enforces the same checks on pull requests.
- Coverage output is visible and ready to gate future PRs.

Notes:

- Keep this PR focused on engineering workflow, not tide logic.
- If needed, split into `0A tooling` and `0B build scaffold`.

## Phase 1: Minimal App Skeleton

Goal: land the smallest runnable Connect IQ app with no networking.

PR scope:

1. Add `App.mc`, `View.mc`, `TideService.mc`, and manifest/build files.
1. Render a static summary screen only.
1. Use placeholder text for station, state, next tide, and freshness.
1. Add unit tests for view-model or formatting helpers introduced in this PR.

Exit criteria:

- App launches in the Forerunner 55 simulator.
- Summary screen renders within the planned density constraints.
- Tests cover the new helpers and state mapping at 80%+.

## Phase 2: UI State Model and Button Flow

Goal: make the UI structure real before live data arrives.

PR scope:

1. Define the screen states from the design doc:
   `loading`, `fresh`, `stale`, `no cache`, `GPS locating`, `GPS denied`,
   `GPS timeout`, `no nearby station`, `refresh failed`.
1. Implement summary screen to day-table navigation.
1. Map `START`, `UP`, `DOWN`, and `BACK` behavior.
1. Add tests for state transitions and button-driven day paging logic.

Exit criteria:

- The app can move between summary and daily table views.
- Non-happy-path states render predictable copy.
- Button handling is deterministic and covered by tests.

## Phase 3: Offline Cache Model

Goal: support the real MVP data shape without live API calls yet.

PR scope:

1. Introduce the compact tide cache model.
1. Model one station, station mode, freshness timestamp, and 7 cached days.
1. Render daily event rows with time, type, and level.
1. Add fixtures for realistic 7-day tide data.
1. Add tests for decoding, day grouping, formatting, truncation, and stale-state
   labeling.

Exit criteria:

- App renders cached tide data from local fixtures.
- One-day-per-screen paging works with realistic event counts.
- Cache-driven rendering is covered at 80%+ for the touched code.

## Phase 4: Phone-Assisted Tide Fetch

Goal: replace stub data with a real thin-client fetch path.

PR scope:

1. Implement the first real tide retrieval path for one station.
1. Parse only the minimal fields needed by the app.
1. Persist the compact 7-day cache on device.
1. Handle refresh failures without losing the previous cache.
1. Add tests around fetch success, parse failure, and stale-cache fallback.

Exit criteria:

- The watch can fetch and persist a 7-day cache for one station.
- Existing cached data remains available when refresh fails.
- Network logic is covered by tests and simulator-validated.

## Phase 5: Home Station Selection

Goal: make the default station real without turning the watch into a browser.

PR scope:

1. Implement phone-selected home station support.
1. Display `Home: <station>` clearly on the summary screen.
1. Refresh and cache tides for the selected home station.
1. Add tests for station-label rendering and station-change behavior.

Exit criteria:

- The app clearly identifies the current home station.
- Changing the home station updates fetched and cached data correctly.
- No watch-side station catalog is introduced.

## Phase 6: One-Shot GPS Nearest-Station Override

Goal: support travel use cases without adding continuous location complexity.

PR scope:

1. Add the explicit `Use Current Location` action.
1. Acquire GPS once on demand.
1. Resolve the nearest station through the thin-client path.
1. Display `Nearest: <station>` clearly and refresh the cache.
1. Add tests for GPS state transitions and override behavior.

Exit criteria:

- GPS is only used when explicitly requested.
- Home vs nearest station labeling is always obvious.
- Failure modes are handled gracefully:
  permission denied, timeout, no nearby station, refresh failure.

## Phase 7: Hardening and Release Readiness

Goal: make the MVP safe to review, test, and potentially publish.

PR scope:

1. Polish copy and layout within the documented density rules.
1. Validate memory-sensitive behavior on the Forerunner 55 simulator.
1. Add any missing tests around regressions or edge cases.
1. Tighten CI thresholds if needed.
1. Update README and release notes for installation and known limits.

Exit criteria:

- App matches the documented interaction model.
- Tests and CI are stable.
- The MVP is reviewable for store-readiness or real-device validation.

## Proposed PR Sequence

If we want maximum reviewability, land in this order:

1. `PR 0A` tooling, lint, format, CI, coverage
1. `PR 0B` minimal Connect IQ project scaffold
1. `PR 1` static summary app
1. `PR 2` UI state model and button flow
1. `PR 3` offline cache model and daily table
1. `PR 4` real tide fetch and persistence
1. `PR 5` phone-selected home station
1. `PR 6` one-shot GPS nearest-station override
1. `PR 7` hardening and release prep

## Coverage Strategy

The 80% target should be enforced pragmatically:

- Prefer coverage for pure logic, state mapping, parsing, date grouping, and
  formatting helpers.
- Avoid fake confidence from screenshot-only tests with little logic coverage.
- Treat parsing and state transitions as the highest-value units to test.
- If a PR is hard to test to 80%, that is usually a sign the PR is too broad or
  the logic needs extraction into smaller units.

## Review Strategy

Each PR description should answer:

1. What user-visible capability landed?
1. What is deliberately not in scope yet?
1. How was this validated?
1. What test coverage was added?
1. What is the next PR expected to do?

## Not in Scope for MVP

- Tide charts
- Multi-station browsing on-watch
- Background GPS tracking
- Offline station catalogs
- Rich visualizations that depend on color or dense layouts
