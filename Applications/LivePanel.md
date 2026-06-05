# LivePanel

Status: `Concept`

## Purpose
LivePanel is currently envisioned as a future unified ecosystem dashboard for Starsong Tools.

Its purpose is to provide:

- a single-pane-of-glass view across Starsong applications
- optional orchestration support
- ecosystem visibility without making other applications dependent on it

The current vision is not to centralize all creator workflows into one large application. The vision is to give the ecosystem a local-first coordination and visibility layer when that becomes useful.

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

## Future Possibilities
Already-identified ecosystem opportunities include:

- application health visibility
- status aggregation
- profile coordination
- application launching
- broader ecosystem visibility

These are opportunities for future planning, not implementation commitments.

## Open Questions
- How should discovery be presented to users?
- How should profile coordination work without overreaching into application-owned behavior?
- What belongs in LivePanel versus individual applications?
- Where should scope boundaries exist so LivePanel stays useful without becoming bloated?
- How much orchestration is helpful before integration becomes unnecessary coupling?
