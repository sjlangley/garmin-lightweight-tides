# Garmin Tide App (Forerunner 245 Compatible)

## Overview

This project is a lightweight tide application built using Garmin Connect IQ, specifically designed to run on constrained devices like the Forerunner 245.

The goal is to provide:

* Current tide status (high / low)
* Next tide time
* Simple visual indicator (optional)

The app is intentionally minimal to stay within device limits.

---

## Target Devices

* Primary: Forerunner 245
* Secondary: Other low-memory Connect IQ devices

---

## Architecture

### Design Principles

* Keep the watch app extremely lightweight
* Avoid large embedded datasets
* Prefer phone-assisted data retrieval
* Minimize memory and CPU usage

---

## Data Strategy

### Recommended Approach: Phone-Assisted API

The watch app retrieves tide data via the paired phone.

Options:

* Public tide APIs (e.g. NOAA or equivalent)
* Custom lightweight proxy (optional)

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

The Forerunner 245 has strict limits:

* Memory is limited
* No large datasets
* Limited background processing
* Network calls are restricted

Design accordingly.

---

## Roadmap

* [ ] Basic UI with current tide
* [ ] Next high/low tide
* [ ] Phone-assisted API integration
* [ ] Configurable location
* [ ] Simple graph (if feasible)

---

## Non-Goals

* Full tide charts
* Offline global tide database
* Complex animations

---

## Notes

This project prioritizes compatibility over feature richness.
If something feels “too heavy”, it probably is.

---
