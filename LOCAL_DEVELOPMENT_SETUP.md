# Local Development Setup

Generated on 2026-04-04
Status: DRAFT

## Purpose

This document explains how to set up a local Garmin Connect IQ development
environment for this project, including the SDK, simulator, editor support, and
the minimum tools needed to build and test against the Forerunner 55 target.

## Current Repo Status

This repository is still in the planning phase.

That means:

- the Connect IQ app scaffold may not exist yet
- the build, lint, format, and test commands may not be wired up yet
- Phase 0 in [IMPLEMENTATION_PLAN.md](IMPLEMENTATION_PLAN.md) is where we land
  the tooling and CI setup

So this guide covers two things:

1. the Garmin tools you should install now
1. the repo-level tooling we expect to add before the first code PR lands

## Official Garmin Sources

Use Garmin's official developer docs as the source of truth:

- [Connect IQ overview](https://developer.garmin.com/connect-iq/overview/)
- [Get the SDK](https://developer.garmin.com/connect-iq/sdk/)
- [Connect IQ basics](
  https://developer.garmin.com/connect-iq/connect-iq-basics/getting-started/
  )
- [Compatible devices](
  https://developer.garmin.com/connect-iq/compatible-devices/
  )
- [Submit an app](https://developer.garmin.com/connect-iq/submit-an-app/)

As of Garmin's overview page, the latest Connect IQ SDK listed there is
`8.4.1`, updated on February 3, 2026. The safest local setup is to install the
latest SDK available through Garmin's SDK Manager instead of hardcoding a
specific older version.

## Required Tools

Install these before starting local development:

- Java
- Visual Studio Code
- Garmin Connect IQ SDK Manager
- Garmin Monkey C Visual Studio Code extension

Repo expectation:

- formatting tool
- lint tool
- test harness
- coverage reporting

Those repo-local tools should land in Phase 0 before the first code PR.

## Step 1: Install Java

Garmin's tooling depends on Java for the SDK tools and build flow.

Recommended check:

```bash
java -version
```

If Java is not installed, install a current JDK that is supported on your
machine, then verify the command above works.

## Step 2: Install the Connect IQ SDK Manager

Garmin's SDK page instructs developers to:

1. download the SDK Manager
1. launch it
1. complete first-time setup
1. use it to download the latest Connect IQ SDK and devices
1. set the downloaded SDK as the active SDK

Use Garmin's official download page:

- [Get the SDK](https://developer.garmin.com/connect-iq/sdk/)

Recommended setup choices for this repo:

- install the latest available Connect IQ SDK
- download the device packages needed for Forerunner 55 development
- keep older device packages optional unless we deliberately widen support

## Step 3: Install the Monkey C VS Code Extension

Garmin's SDK page also documents the Visual Studio Code flow:

1. open VS Code Extensions
1. search for `Monkey C`
1. install the Garmin extension
1. restart VS Code
1. run `Monkey C: Verify Installation`

This project should use the Garmin Monkey C extension as the default editor
workflow unless we later document a different CLI-first flow.

## Step 4: Download the Right Device Profiles

This project targets the Forerunner 55 first.

Garmin lists the Forerunner 55 as:

- `208 x 208`
- `Memory-In-Pixel`
- `8 colors`
- API level `3.4`

Source:

- [Compatible devices](
  https://developer.garmin.com/connect-iq/compatible-devices/
  )

Why this matters:

- simulator testing must use the Forerunner 55 profile
- UI layout should be validated at `208 x 208`
- color assumptions should be kept extremely conservative
- memory and rendering choices should be tested against this class of device,
  not a richer profile

If the SDK Manager lets you choose device downloads individually, make sure the
Forerunner 55 package is installed.

## Step 5: Set Up the Simulator Workflow

For this repo, simulator testing is mandatory before real-device testing.

Minimum simulator setup:

- confirm the Connect IQ simulator launches from the installed SDK tooling
- confirm the Forerunner 55 device profile is available
- use that profile as the default during development

Primary simulator checklist for this project:

- app launches cleanly
- summary screen fits within the density rules from
  [SIMPLE_TIDE_APP_DESIGN.md](SIMPLE_TIDE_APP_DESIGN.md)
- day table paging works with button navigation
- stale, no-cache, GPS, and refresh-failure states render clearly
- no visually dense or graph-heavy UI sneaks in

## Step 6: Prepare for Real-Device Validation

The simulator is necessary, but not sufficient.

We should also validate on a real Forerunner 55 as early as practical for:

- button feel
- text density
- actual legibility
- GPS behavior
- any device-specific quirks not visible in the simulator

Do not wait until the app feels "done" before testing on real hardware.

## Step 7: Repository Tooling We Still Need

Before the first code PR lands, this repo should add:

- build command documentation
- lint configuration
- formatter configuration
- test runner setup
- coverage reporting
- CI checks for format, lint, tests, and coverage

That work is tracked in Phase 0 of
[IMPLEMENTATION_PLAN.md](IMPLEMENTATION_PLAN.md).

## Recommended Local Workflow for This Repo

Once Phase 0 lands, the local workflow should look like this:

1. install Garmin SDK tooling
1. open the repo in VS Code
1. verify Monkey C extension setup
1. run repo format and lint commands
1. run tests with coverage
1. build and run in the Forerunner 55 simulator
1. validate real-device behavior before larger feature PRs

Today, before Phase 0 lands, the Garmin setup is the important part. The
repo-local commands should be documented and enforced as part of the first
tooling PR.

## Troubleshooting

If local setup is failing, check these first:

- Java is installed and available on `PATH`
- the Garmin SDK Manager completed successfully
- the latest SDK is set as active in the SDK Manager
- the VS Code Monkey C extension is installed
- `Monkey C: Verify Installation` passes
- the Forerunner 55 device profile is installed

If something still looks wrong, compare your environment against Garmin's
current docs before assuming the repo is the problem.

## Project-Specific Expectations

For this project specifically:

- always test with the Forerunner 55 profile first
- optimize for low memory and low visual density
- avoid developing against richer devices and back-porting later
- treat the simulator as required and real-device checks as strongly preferred
- keep the watch app thin, conservative, and easy to reason about

## Future Doc Updates

After Phase 0 lands, update this document with:

- exact local format command
- exact local lint command
- exact local test command
- exact local coverage command
- exact local build command
- exact simulator launch command, if we standardize one
