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
Local desktop control panel for the Starsong Tools suite. LivePanel can launch supported apps in service mode, show module status, switch active profiles, run StreamSignal workflows, and preview TideReader overlay state. The LupusAria integration adds bot controls and diagnostics to LivePanel.

### LupusAria
Local Twitch chat bot with AI chat responses, AutoSO, announcements, ad alerts, Media Actions, raffle, and Stream Wrap-Up. The SIP v1 integration adds service mode and LivePanel controls. [LupusAria overview](Applications/LupusAria.md)

## Current Suite Snapshot

Updated 2026-11-20 using the application repositories and their release tags.

| Application | Tagged release | Ecosystem integration |
| --- | --- | --- |
| StreamSignal | `v1.0.0` | SIP profiles and publishing workflows, including duplicate confirmation and operation status. |
| TideReader | `v1.0.0` | SIP profiles, browser support, and LivePanel overlay preview. |
| TuberSwitch | `v1.0.1` | SIP profiles, manual redeem controls, and OBS status. |
| LivePanel | `v1.0.1` | Consumes the three released companion integrations. The LupusAria companion is in development on the integration branch. |
| LupusAria | `v0.11.0` | The integration branch adds headless SIP v1 service mode, bot lifecycle, live feature switches, and recent logs. |

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
