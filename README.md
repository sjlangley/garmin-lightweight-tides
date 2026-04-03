# Garmin Tide App (Forerunner 55 Compatible)

## Overview

This project is a lightweight tide application built using Garmin Connect IQ,
specifically designed to run on constrained devices like the Forerunner 55.

The goal is to provide:

* Current tide status (high / low)
* Next tide time
* A button-paged text table of cached high/low tides
* A one-shot "use current location" option to resolve the nearest station

The app is intentionally minimal to stay within device limits.

---

## Target Devices

* Primary: Forerunner 55
* Secondary: Other low-memory Connect IQ devices

---

## Architecture

### Design Principles

* Keep the watch app extremely lightweight
* Avoid large embedded datasets
* Prefer phone-assisted data retrieval
* Use GPS only on demand
* Minimize memory and CPU usage

---

## Data Strategy

### Recommended Approach: Phone-Assisted API

The watch app retrieves tide data via the paired phone, then stores a compact
offline cache on-device.

Options:

* Public tide APIs (e.g. NOAA or equivalent)
* Custom lightweight proxy (optional)

Recommended MVP payload:

* Selected station name
* Cache timestamp
* Up to 7 days of high/low tide events for that station
* Events grouped by day for rendering
* Each event should include only the minimum needed for rendering: event type,
  event time, and level

Recommended MVP station flow:

* Default station is selected on the phone
* The watch may offer a one-shot "use current location" action
* The watch gets a GPS fix once, sends coordinates through the thin-client flow,
  and receives the nearest station plus a fresh 7-day cache
* The app should never store a large station catalog on-watch

---

## Project Structure

```
/source
  App.mc              # Entry point
  View.mc             # UI rendering
  TideService.mc      # Data handling
/resources
  layouts.xml         # UI layouts
  strings.xml         # Text resources
/manifest.xml         # App configuration
```

---

## Development Setup

### Requirements

* Garmin Connect IQ SDK
* Visual Studio Code + Connect IQ extension
* Java (for SDK tools)

---

## Build

```
monkeyc -f monkey.jungle -o bin/app.prg -y developer_key
```

---

## Run (Simulator)

```
connectiq
```

---

## Constraints (IMPORTANT)

The Forerunner 55 has strict limits:

* Memory is limited
* The screen is smaller at 208 x 208
* The display is limited to 8 colors
* No large datasets
* Limited background processing
* Network calls are restricted

Design accordingly.

---

## Roadmap

* [ ] Basic UI with current tide
* [ ] Next high/low tide
* [ ] Phone-assisted API integration
* [ ] 7-day cached high/low tide table with button paging by day
* [ ] Phone-selected home station
* [ ] One-shot GPS nearest-station lookup

---

## Non-Goals

* Full tide charts
* Offline global tide database
* Complex animations

The app should prefer text over visuals. If a feature needs a graph to make
sense, it probably does not belong in the MVP.
The Forerunner 55 MVP should assume button navigation first.

---

## Notes

This project prioritizes compatibility over feature richness.
If something feels “too heavy”, it probably is.

---
