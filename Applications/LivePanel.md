# LivePanel

Status: `Released Foundation`

## Purpose
LivePanel is the local desktop control panel for Starsong Tools.

Its purpose is to provide:

- a single-pane-of-glass view across Starsong applications
- optional orchestration support
- ecosystem visibility without making other applications dependent on it

The current vision is not to centralize all creator workflows into one large application. LivePanel gives the ecosystem a local-first coordination and visibility layer while leaving application-owned workflows in their owning apps.

## Current Implementation State
Last scanned: 2026-06-11.

- Current release line: `v0.1.0`.
- Supported apps: StreamSignal, TideReader, and TuberSwitch.
- Minimum dependent versions listed by LivePanel: StreamSignal `v0.4.0`, TideReader `v0.5.0`, and TuberSwitch `v0.6.0`.
- Current capabilities include service-mode launch, `--show` restoration, module status, SIP diagnostics, active profile switching, StreamSignal Go Live and End Stream workflows, and TideReader overlay preview.
- LivePanel stores module location/configuration data, but not app-owned credentials, overlay/profile data, destination secrets, or profile storage.

## Ecosystem Role
LivePanel is intended to fill the visibility and coordination gap between independent Starsong applications.

It exists conceptually because:

- the ecosystem is growing beyond a single product
- future contributors may need one place to observe ecosystem health and state
- profile coordination may eventually benefit from a dedicated orchestration surface

LivePanel complements the existing applications by sitting above them as an optional layer. It does not replace their owned responsibilities.

## Relationship to SIP
LivePanel consumes SIP.

LivePanel is also a SIP participant.

LivePanel does not own SIP.

LivePanel receives no special protocol privileges.

Its role should be to use the same ecosystem rules as every other SIP-capable application:

- discover participants
- inspect identity
- read health and status
- detect capabilities
- trigger allowed actions

## Relationship to Profiles
Profiles remain application-owned.

LivePanel may coordinate profile activation.

LivePanel does not replace application profile systems.

Its likely future role is to help a user activate a Starsong Profile across participating applications while preserving each application’s ownership of its own profile definitions.

## Non-Goals
- Not a replacement for StreamSignal
- Not a replacement for TideReader
- Not a replacement for TuberSwitch
- Not a plugin marketplace
- Not a workflow automation engine
- Not a cloud platform
- Not required for ecosystem operation

## Current And Future Possibilities
Current implemented ecosystem capabilities include:

- application health visibility
- status aggregation
- profile coordination
- application launching
- StreamSignal workflow controls
- TideReader overlay preview

Future opportunities still include:

- broader ecosystem visibility
- deeper cross-application profile coordination
- additional Starsong applications
- richer diagnostics and release compatibility guidance

## Open Questions
- How should discovery be presented to users?
- How should profile coordination work without overreaching into application-owned behavior?
- What belongs in LivePanel versus individual applications?
- Where should scope boundaries exist so LivePanel stays useful without becoming bloated?
- How much orchestration is helpful before integration becomes unnecessary coupling?
