# StarsongDesignSystem

StarsongDesignSystem is the central documentation hub for the Starsong Tools ecosystem.

It exists to keep ecosystem standards, architecture decisions, application overviews, and roadmap planning in one practical place without introducing unnecessary structure.

## What Is Starsong Tools
Starsong Tools is a collection of focused creator and streaming utilities that can operate independently or together as a connected ecosystem.

## Core Principles
- Standalone First
- Local First
- Creator Focused
- Simple Over Complex
- Consistency Over Novelty
- Integration Without Dependency

## Current Applications

### StreamSignal
Multi-destination stream announcements, reusable announcement profiles, SIP-driven Go Live and End Stream workflows, and service-mode participation for LivePanel.

### TideReader
Music and now-playing overlays with saved overlay profiles, local output generation, overlay preview data, and SIP status/profile participation.

### TuberSwitch
Avatar/profile switching for OBS and Twitch reward alignment, with saved stream profiles, app detection, service mode, and SIP profile activation.

### LivePanel
Local desktop control panel for the Starsong Tools suite. LivePanel can launch supported apps in service mode, show module status, switch active profiles, run StreamSignal workflows, and preview TideReader overlay state.

## Current Suite Snapshot
Last scanned: 2026-06-11.

| Application | Current local state | Ecosystem integration state |
| --- | --- | --- |
| StreamSignal | `v0.4.0` release line is tagged and documented; local `wails.json` still reports product version `0.3.1`, so release metadata should be reconciled before the next publication pass. | SIP, service mode, active-profile status, Go Live, duplicate confirmation, End Stream, and operation status endpoints are documented in the app repo. |
| TideReader | `v0.5.0` is tagged and project metadata reports `0.5.0`. | SIP exposes app, health, capabilities, status, profile list, current profile, and profile activation over localhost. |
| TuberSwitch | `v0.6.0` is tagged and Wails metadata reports `0.6.0`. | SIP exposes app, health, capabilities, status, profiles, current profile, and profile activation over localhost. |
| LivePanel | `v0.1.0` is tagged and Wails/frontend metadata report `0.1.0`. | Consumes StreamSignal, TideReader, and TuberSwitch SIP/service-mode surfaces; current app requirements are StreamSignal `v0.4.0`, TideReader `v0.5.0`, and TuberSwitch `v0.6.0`. |

## Repository Purpose
This repository contains:

- design standards
- architecture standards
- integration protocols
- ecosystem planning
- application overviews

## Current Structure
- [DesignSystem](DesignSystem)
- [Architecture](Architecture)
- [Applications](Applications)
- [Roadmap](Roadmap)

## Core Standards
- [Starsong Design System v1.1](DesignSystem/Starsong-Design-System-v1.1.md)
- [Starsong Ecosystem Review v1.1](DesignSystem/Starsong-Ecosystem-Review-v1.1.md)
- [SIP-v1](Architecture/SIP-v1.md)
- [Starsong Profiles v1](Architecture/Starsong-Profiles-v1.md)
- [Starsong Installer Standard v1](Architecture/Starsong-Installer-Standard-v1.md)

## Repository Standards

### Documentation Before Implementation
Major ecosystem decisions should be documented before implementation begins.

### Single Source of Truth
Ecosystem-level standards should live here. Application-specific implementation details should remain in application repositories.

### Simplicity First
The repository should stay lightweight, navigable, and grounded in current needs.
