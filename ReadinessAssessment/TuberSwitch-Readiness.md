# TuberSwitch Readiness Assessment

## 1. Executive Summary

### Overall Readiness Score
`6.9 / 10`

### Key Findings
Strengths:
- Clear focused responsibility
- Good utility-shell alignment
- Strong candidate for SIP identity, health, status, and mode-related actions

Weaknesses:
- Profile model is partial rather than fully user-facing
- Design-system alignment is solid but less mature and less expressive than StreamSignal and TideReader

Notable opportunities:
- Strong future participant for mode/state visibility
- Good candidate for later SIP action and profile support

## 2. Design System Alignment

### Color System
Classification: `High`

Notes:
- Strong palette alignment
- Uses the same surface and accent family as the rest of the ecosystem

### Typography
Classification: `Medium`

Notes:
- Clear and usable
- Slightly less aligned with the preferred Starsong typography direction because of its current stack and simpler hierarchy

### Spacing
Classification: `Medium`

Notes:
- Generally solid
- More utilitarian and less consistently refined than the other two apps

### Cards
Classification: `Medium`

Notes:
- Uses panel-like containers effectively
- Card language is less developed and less varied than the ecosystem’s strongest examples

### Buttons
Classification: `Medium`

Notes:
- Functional hierarchy exists
- Less explicitly aligned with the shared Starsong button vocabulary

### Status Components
Classification: `Medium`

Notes:
- Connection pills and mode indicators are useful
- Less mature as a general reusable status language than StreamSignal or TideReader

### Forms
Classification: `Medium`

Notes:
- Clear and practical
- Less refined in structure and validation behavior than TideReader

### Feedback Patterns
Classification: `Medium`

Notes:
- Functional
- More limited than StreamSignal in toast/banner patterns and less formalized than TideReader in inline validation

### Shell Alignment
Classification: `High`

Notes:
- Strong example of the compact utility shell family
- Good fit with the Starsong utility-app pattern

## Design System Summary

### Alignment Score
`7.2 / 10`

### Recommended Improvements
- Align typography and button hierarchy more closely with the shared system
- Mature status components into a broader semantic model
- Continue refining forms and feedback behavior

### Migration Difficulty
`Medium`

## 3. SIP-v1 Readiness

### Application Identity
Classification: `Easy`

Why:
- Clear product identity and focused purpose

### Health Reporting
Classification: `Easy`

Why:
- The app already has meaningful connection and operational state

### Status Reporting
Classification: `Easy`

Why:
- Current mode, connection state, and app-detection outcomes provide natural status sources

### Actions
Likely actions:
- `open`
- `refresh`
- `switchMode`
- `openSettings`

Implementation difficulty: `Moderate`

Why:
- Lightweight and mode-switch actions are feasible
- Some actions touch OBS/Twitch behavior and should remain carefully scoped

### Capabilities
Likely capability flags:
- `supportsStatusReporting`
- `supportsQuickActions`
- `supportsModeSwitching`
- `supportsProfiles` in a future-expanded sense

Difficulty: `Easy to Moderate`

### Overall SIP Readiness

#### Readiness Score
`7.5 / 10`

#### Implementation Complexity
`Medium`

#### Recommended SIP Adoption Phase
`Phase 3`

## 4. Profiles v1 Readiness

### Existing Concepts
- mode profiles
- current mode
- startup mode
- reward behavior linked to mode
- scene behavior linked to mode

### Mapping Quality
Classification: `Medium`

Why:
- The app has a real profile-adjacent concept, but it is narrower and more implementation-tied than a broader user-owned profile system

### Activation Readiness
Classification: `Medium`

Why:
- `POST /api/v1/profile` is realistic, but profile semantics may need clarification or expansion beyond the current mode model

## Profile Readiness Summary

### Readiness Score
`6.6 / 10`

### Migration Complexity
`Medium`

## 5. Ecosystem Opportunities

### Most Valuable Integrations
- mode/status sharing
- health visibility
- lightweight mode-switch actions
- future profile coordination

### Future Opportunities
- participation in Starsong Profile activation
- LivePanel visibility for mode and connection state
- clearer mapping between avatar workflow state and ecosystem orchestration

## 6. Risks and Constraints

### Complexity Risks
- Expanding mode behavior into a broad ecosystem profile system too quickly could create confusing abstractions

### Coupling Risks
- Integration should not make OBS/Twitch scene logic dependent on external orchestration

### Architecture Risks
- Exposing internal mode structures directly through SIP may lock in product-specific assumptions too early

### UX Risks
- Users may conflate mode switching with higher-level profile orchestration if naming is not careful
