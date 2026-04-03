# AGENTS.md

## Purpose

Guide AI agents (gstack, Codex, Claude, etc.) to build a Garmin Connect IQ tide app that works on constrained devices like the Forerunner 245.

---

## Core Constraints (DO NOT VIOLATE)

1. **Memory is extremely limited**

   * Do NOT embed large datasets
   * Avoid arrays with large static data
   * Keep code minimal

2. **No heavy computation**

   * Avoid complex math models
   * Avoid unnecessary loops or processing

3. **Networking is limited**

   * Prefer phone-assisted data
   * Do NOT assume direct internet access from watch

4. **UI must be simple**

   * No complex layouts
   * No heavy redraw loops
   * Keep rendering lightweight

---

## Architecture Rules

* The watch app is a **thin client**
* Business logic should be minimal
* Data should come from:

  * phone companion OR
  * lightweight API calls

---

## Code Guidelines

* Language: Monkey C only
* Follow Connect IQ SDK patterns
* Keep files small and focused
* Avoid abstraction-heavy patterns
* Prefer clarity over cleverness

---

## Allowed Features

* Display text (current tide, next tide)
* Simple shapes or indicators
* Basic time calculations

---

## Forbidden Patterns

* Large embedded JSON or datasets
* Continuous background processing
* Complex charting libraries
* Overly dynamic UI rendering

---

## File Responsibilities

* `App.mc` → App lifecycle
* `View.mc` → UI rendering
* `TideService.mc` → Tide data retrieval (stub or API)

---

## Development Strategy

1. Start with a static mock tide display
2. Add dynamic time updates
3. Integrate data source (API or phone)
4. Optimize for memory and performance

---

## Testing

* Always test in simulator with low-memory device profile
* Prefer real-device validation early

---

## Success Criteria

* App runs without crashing on Forerunner 245
* Memory usage stays within limits
* UI remains responsive

---

## Tone for Agents

* Be conservative
* Prefer simple implementations
* Avoid over-engineering

---
