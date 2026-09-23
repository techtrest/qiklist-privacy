# Policy rewrite notes — 2026-09-23

Companion to the `index.md` rewrite on branch `rewrite/policy-2026-09`. This file is not published; it's for review before merge.

## What changed, and why

| Change in the new policy | Audit finding behind it |
|---|---|
| FCM section now says item names, list names, and the acting user's display name/email pass through Google's servers, instead of the old "no access to shopping list contents" claim | Backend audit §1(b) and §2a: `main.py:2461-2466` (`items_added` push includes `item_names`), plus `invite_received`/`invite_accepted`/`invite_declined` payloads (`main.py:1961-1976, 2130-2144, 2205-2224`) include `list_name` and an inviter/accepter/decliner name that falls back to email. Android audit §2b/§4 confirms the client renders notifications from exactly these fields. Old claim was FALSE per the audit's own verdict. |
| Resend and Google Sign-In added as named service providers (old policy said "no third party except Firebase") | Backend audit §1(c): Resend receives the email address + verification/reset link on every registration/reset email (`mailer.py:132-212`); Google receives the ID token for verification on Google Sign-In (`main.py:871-878`). Old claim was FALSE per the audit. |
| "Firebase installation ID" and "delivery telemetry" added to what Firebase receives | Android audit §2b: `firebase-installations` issues a per-device FID independent of any account; `firebase-datatransport`/Firelog-CCT is live in the release manifest with no opt-out configured anywhere in the project. |
| New caveat under "Using the app without an account": Firebase token fetch happens on every launch regardless of sign-in | Android audit §1a, §2b, Must-fix #5: `MainActivity.kt:107` calls `FirebaseMessaging.getInstance().token` unconditionally. The old "nothing is sent" claim was true only if you exclude Google. |
| Delete-account location corrected to "menu → Account → Delete account" (was "Settings → Delete account") | Android audit §1c, Must-fix #1: the feature lives in `AccountScreen` ("Your data" section), not `SettingsScreen`. |
| New "Reporting an issue" section disclosing the diagnostic-log-attached-by-default email flow to `techtrest@pm.me` | Android audit §6, Must-fix #4: `ReportIssueScreen.kt` attaches a zipped `calluna.log` by default (opt-out toggle exists but defaults on); this flow bypasses the backend entirely. |
| "At rest" encryption stated plainly as none | Backend audit §6: SQLite file is unencrypted; audit explicitly says the old policy should not imply otherwise. |
| Backup facts (nightly, AES-256, Hetzner Object Storage, ≤8 days) added | Not derivable from the backend repo (`deploy.sh` has no backup logic; audit §7 says this must come from you). Taken from your verified server facts, not the audits. |
| Server logs section (IP + request, 14 days, no separate web-server access log) added | Not previously in the policy at all. Taken from your verified server facts. The backend audit itself only found email-address logging on Resend failures and explicitly could not verify IP logging or retention from the repo (see open questions below). |
| Retention section now names two known gaps: stale/unaccepted invites and never-verified accounts | Backend audit §4: no scheduler exists in the app at all; `pending_invites` and unverified `users` rows are retained indefinitely with no automatic cleanup. |
| Legal-basis (GDPR Art. 6) section added | New content — wasn't in the old policy. Contract for account/sync/sharing/notifications; legitimate interest for logs/security, per your instructions. |
| UODO / supervisory-authority complaint right added to "Your rights" | New content — GDPR requirement not previously stated. |
| Children's policy (under 16) added | New content — not previously stated. |
| International-transfer sentence (DPF/SCC) added for Google and Resend, with an inline `VERIFY` comment on Resend specifically | New content, per your instruction to flag Resend's DPA/transfer mechanism as unverified. |
| **Not included:** "invites to someone who has no account yet" | See open questions — this contradicts the backend audit's own findings, so I left it out rather than assert something the audit says is false. |

## Sentences to update when planned fixes ship

These are the specific places to revisit once each fix lands — don't merge these changes speculatively now, but keep this list so the policy doesn't go stale later.

- **FCM "tickle" push (Google no longer sees content):** once notifications move to a data-free tickle + on-device fetch, rewrite the "Service providers" paragraph on Firebase (the table row and the "To be direct about the Firebase point above..." paragraph) to drop the claim that item/list names and names pass through Google. At that point Firebase goes back to "device token + installation ID only."
- **FCM auto-init only after sign-in:** once the token fetch in `MainActivity.kt` is gated behind sign-in, remove or soften the caveat paragraph in "Using the app without an account" — Firebase Installations would then no longer see signed-out devices at all.
- **Invite / unverified-account cleanup job:** once a sweep exists for stale `pending_invites` and never-verified accounts, update "How long we keep your data" to state a concrete retention period for those instead of the current "we don't yet have an automatic cleanup process" admission.
- **Paid unlock via Google Play Billing:** if/when this ships, add a new "Payments" section (Google Play Billing receives purchase tokens; no card details reach us) and add a line to "What we collect" and "Service providers."

## Open questions for you

1. **Invite-to-non-account-holder claim.** Your brief asked me to cover "invites, including the email of an invited person who has no account yet." The backend audit (§2e) says the opposite: `POST /lists/{list_id}/invite` looks up the address in `users` and returns `no_account_for_email` (404) if no account exists — it never stores or emails an address without an account. I followed the audit and left this out of the policy. Let me know if I'm missing a code path, or if this is a feature that doesn't exist yet and was misremembered.
2. **Resend's DPA / transfer mechanism** — flagged inline with `<!-- VERIFY: Resend DPA -->`. I don't have visibility into your actual contract with Resend, so this needs a manual check.
3. **Email addresses logged on Resend failures.** The backend audit found `mailer.py:92-95, 119, 124-126` log the recipient's email address (via `print()`, landing in journald) when a verification/reset email fails to send or send. This isn't covered by the "IP address, 14 days" description you gave me — do you want it disclosed too, folded into the same 14-day retention, or is it out of scope?
4. **Web-server access logs.** You confirmed there are none. The backend audit couldn't verify this from the repo (Caddy config is server-side, out of scope) — worth a one-time check that Caddy itself isn't writing a separate access log somewhere.
5. **Locally-stored partner data on Android.** The Android audit (§3, §7) notes that `shared_with`/`owner_email` (your partner's email) and per-item `assignedTo` names are stored unencrypted in the app's local DataStore/Room DB on each member's device — this is local-only, not a data flow to us or a third party, so I didn't add a line for it, but flag if you want one anyway.
