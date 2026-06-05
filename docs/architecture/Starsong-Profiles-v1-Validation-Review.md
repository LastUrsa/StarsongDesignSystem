# Starsong Profiles v1 Validation Review

## Purpose
This review evaluates the current Starsong Profiles v1 specification before implementation begins.

The goal is to identify:

- missing requirements
- edge cases
- simplifications
- future risks

This is a documentation-only validation pass.

## Overall Assessment
The Profiles v1 specification is directionally strong.

It is intentionally simple, aligns well with SIP-v1, and preserves application independence. That simplicity is a strength.

The main risk is not conceptual weakness. The main risk is underspecification around identity, result semantics, and activation behavior, especially once multiple applications with uneven profile maturity participate.

## Missing Requirements

### Canonical Application IDs
The spec assumes stable application IDs such as:

- `streamsignal`
- `tidereader`
- `tuberswitch`

This should be explicitly tied to SIP application identity so that Starsong Profile mappings are guaranteed to match `appId` from `GET /api/v1/app`.

Recommendation:
- State that `applications` keys must match SIP `appId` values exactly.

### Profile Name Uniqueness
The spec assumes profile activation by name, but does not explicitly define whether application profile names must be unique.

Risk:
- Two application profiles with the same display name could make activation ambiguous.

Recommendation:
- Require application profile names returned through SIP v1 to be unique within that application.

### Activation Initiator
The spec describes activation flow but does not define who performs it.

Possible initiators:
- a future LivePanel
- an individual application
- a standalone profile manager

Recommendation:
- Explicitly state that any SIP-capable orchestrator may execute the activation workflow and that no application receives special authority.

### Result Status Detail
The current result model uses:

- `success: true|false`
- `reason`

This is usable, but incomplete for a good ecosystem experience.

Recommendation:
- Define recommended reason values for:
- `ProfileNotFound`
- `ApplicationUnavailable`
- `CapabilityUnavailable`
- `ApplicationBusy`
- `ProtocolMismatch`
- `InvalidResponse`

### Availability vs Failure
The spec currently treats missing apps and missing profiles as allowed, but does not strongly separate:

- unavailable application
- reachable application with unsupported profile capability
- reachable application with missing profile

Recommendation:
- Distinguish these clearly in activation summaries.

## Edge Cases

### Duplicate Profile Intent Across Apps
Example:
- Starsong Profile: `Music Stream`
- StreamSignal profile: `Music Announcement`
- TideReader profile: `Album Art Overlay`

This is expected and fine.

However, the spec should clarify that application profile names do not need to match the Starsong Profile name.

Recommendation:
- State that Starsong Profile names are ecosystem-level labels and application profile names remain application-specific.

### Empty Application Mapping
What happens if a Starsong Profile has no `applications` entries?

Recommendation:
- Treat this as invalid profile data.

### Unknown Applications in a Starsong Profile
If a Starsong Profile references an app ID that no current participant recognizes:

Recommendation:
- Treat it as ignorable and report it as unavailable rather than invalidating the whole activation.

### Application Supports Profiles but Returns No Profiles
This case is not currently spelled out.

Recommendation:
- Allow empty lists from `GET /api/v1/profiles`.
- Treat activation attempts against missing names as `ProfileNotFound`.

### Concurrent Activations
The spec does not define what happens when two orchestrators attempt activation at nearly the same time.

Recommendation:
- Leave concurrency handling to applications in v1, but document `ApplicationBusy` as a valid failure reason.

## Simplifications That Are Good

### No Rollback
This is the right call for v1.

Rollback would add complexity well beyond the current ecosystem’s maturity.

### No Shared Storage
Also the right call for v1.

This keeps the ecosystem simple and avoids turning Profiles v1 into a synchronization problem.

### Name-Based Activation
This is good for human readability and debugging.

However, it should be paired with uniqueness expectations.

## Future Risks

### Uneven Product Maturity
Right now, the three reviewed apps are not equally ready:

- StreamSignal is ready
- TideReader is not yet profile-native
- TuberSwitch is partially profile-native

Risk:
- The ecosystem spec may appear complete before the application layer is equally ready.

Recommendation:
- Launch Profiles v1 as an ecosystem contract first, with phased application adoption.

### Name Stability
If profile activation relies only on profile names, renaming profiles could silently break Starsong Profiles stored elsewhere.

Recommendation:
- Accept this tradeoff in v1 for simplicity, but call it out explicitly as a limitation.
- Consider profile IDs in a future version.

### Runtime Semantics
The spec does not define whether profile activation:

- updates active runtime state immediately
- updates only saved selection
- updates both

This can cause cross-app inconsistency.

Recommendation:
- Require each app to document its activation semantics for users and developers.

## Recommended Additions to the Spec

### Add a Limitations Section
Recommended limitations to state explicitly:

- application profile names are the v1 activation key
- renaming profiles may break existing Starsong Profile mappings
- no rollback
- no shared storage
- partial success is normal

### Add a Recommended Reason Vocabulary
Recommended standard reasons:

- `ProfileNotFound`
- `ApplicationUnavailable`
- `CapabilityUnavailable`
- `ApplicationBusy`
- `ProtocolMismatch`
- `InvalidRequest`
- `UnknownError`

### Add a Clarification on `success`
The current top-level `success` field may be misread.

Recommendation:
- clarify that top-level `success` means the activation process completed and at least one target may have succeeded, not that every application succeeded
- or define an additional summary field later in a future revision

## Implementation Guidance Risks to Avoid

### Avoid Over-Centralization
LivePanel should not become the implied owner of Profiles v1.

### Avoid Raw Settings Exchange
Profiles v1 should not tempt applications to expose raw settings payloads through SIP profile activation.

### Avoid App-Name Logic
Profile orchestration should still respect SIP capability-driven design rather than hardcoding product assumptions where possible.

## Conclusion
Profiles v1 is a strong ecosystem concept and a good first orchestration layer for Starsong.

Before implementation begins, the spec should ideally tighten a few areas:

- canonical app ID matching
- profile-name uniqueness expectations
- activation reason vocabulary
- top-level success semantics
- limitations of name-based activation

None of these issues invalidate the design. They are mostly clarifications that will reduce confusion and make first implementations much smoother.
