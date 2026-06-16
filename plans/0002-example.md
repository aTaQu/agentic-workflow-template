# Plan: [Example] Notifications

A worked example of the plan format — phases, acceptance criteria, and an opt-in parallel wave.
Replace or delete this when you start your project. (`- [ ]` todo · `- [x]` done.)

## Phase 1 — Channels

### Wave 1 (parallel)
- [ ] Email channel delivers via the configured mailer
  Scope: in `src/notifications/email/**`, `templates/notifications/email/**`
- [ ] SMS channel delivers via the SMS provider
  Scope: in `src/notifications/sms/**`
- [ ] Webhook channel POSTs to subscriber URLs
  Scope: in `src/notifications/webhook/**`

### Wave 2 (sequential)
- [ ] Preferences UI toggles all three channels
  Scope: in `src/notifications/prefs/**`, `templates/notifications/prefs/**`

## Phase 2 — Delivery log
- [ ] Record every send attempt with status and timestamp
- [ ] Admin view lists recent attempts, filterable by channel

> Phase 1 Wave 1 is three independent channels with disjoint scopes → `/orchestrate` fans them out in
> parallel, then the prefs UI (which depends on all three) runs as a sequential Wave 2. Phase 2 is a
> flat sequential list with no waves → drive it with `/next`.
