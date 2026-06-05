# StarsongDesignSystem

StarsongDesignSystem is the authoritative documentation repository for the Starsong Tools ecosystem.

This repository is the home for:

- design standards
- ecosystem architecture
- integration protocols
- profile standards
- branding standards
- future application specifications
- shared ecosystem decisions

It is documentation-first. No application code is moved here, and no shared component library is created at this stage.

## What Is Starsong Tools
Starsong Tools is a collection of focused creator and streaming utilities.

Applications should:

- operate independently
- integrate when available
- remain local-first
- minimize complexity
- share a consistent experience

## Current Applications

### StreamSignal
Multi-destination stream announcements.

### TideReader
Music and now-playing overlays.

### TuberSwitch
Avatar and profile switching.

### LivePanel
Future ecosystem dashboard and orchestration application.

## Ecosystem Principles
- Standalone First
- Local First
- Creator Focused
- Simple Over Complex
- Consistency Over Novelty
- Integration Without Dependency

## Repository Structure
- `docs/design-system`
  Shared design language, design system standards, and review findings.
- `docs/architecture`
  Ecosystem protocols, profiles, and architectural guidance.
- `docs/branding`
  Vision, naming, attribution, and identity rules.
- `docs/applications`
  High-level summaries for current and future Starsong applications.
- `docs/roadmap`
  Ecosystem planning, future priorities, and idea holding areas.
- `assets`
  Reserved for logos, icons, palette references, and mockups.
- `templates`
  Reusable templates for future application, feature, and protocol specifications.

## Current Core Standards
- [Starsong Design System v1.1](docs/design-system/Starsong-Design-System-v1.1.md)
- [Starsong Ecosystem Review v1.1](docs/design-system/Starsong-Ecosystem-Review-v1.1.md)
- [SIP-v1](docs/architecture/SIP-v1.md)
- [Starsong Profiles v1](docs/architecture/Starsong-Profiles-v1.md)

## Repository Standards

### Documentation First
New ecosystem decisions should be documented before implementation.

### Versioned Specifications
Major standards should be versioned.

Examples:

- `Design System v1.1`
- `SIP-v1`
- `Profiles v1`

### Single Source of Truth
Avoid duplicating ecosystem standards across application repositories.

Application repositories should reference StarsongDesignSystem whenever possible.
