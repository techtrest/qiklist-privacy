# Calluna Privacy Policy
Last updated: September 23, 2026

## In short

Calluna stores your account and shopping list data on a server we run ourselves, in Germany. To deliver push notifications, some of that content currently passes through Google's Firebase service. We never show ads, sell your data, or use analytics or advertising tools. You can export or delete your data yourself, any time, from inside the app.

## Who we are

Calluna (formerly Qiklist) is developed by Oliver Fricke, an independent developer based in Poland. If you have questions about privacy, contact techtrest@pm.me.

## What we collect

To use Calluna's account and sharing features, we collect:
- Your email address, and either a securely hashed password, or — if you sign in with Google — the email and name Google gives us instead of a password
- An optional display name
- The contents of your shopping lists: item names, categories, and quantities
- Invites between accounts: if you invite someone to share a list, or accept an invite, we record the invitation linking your account to theirs. Invites only work between two people who already have a Calluna account.
- A push notification token, used to deliver notifications to your device
- A Firebase installation ID — a device identifier that Google's push notification service assigns automatically (see "Service providers" below)

## Using the app without an account

You can use Calluna without creating an account. In that case, your list data stays entirely on your device and is never sent to our servers.

One caveat: when the app starts, it asks Google's Firebase service for a push-notification token, regardless of whether you're signed in. This means Google's Firebase Installations service registers that your device exists, even if you never create a Calluna account or turn on notifications. No list content or personal information is included in this request.

## Where your data is stored, and how it's protected

Once you create an account, your data is stored on a server we rent from Hetzner in Germany and administer ourselves.

- **In transit:** all traffic between the app and our server is encrypted (TLS).
- **At rest:** the database itself is not encrypted at rest. To be plain about it: your list contents are not end-to-end encrypted, and they are not encrypted on disk.
- **Backups:** we take a nightly backup, encrypted with AES-256 before it leaves the server, and store it in Hetzner's object storage, also in Germany. Backups are deleted after at most 8 days.

## Service providers

We use a small number of outside services to run Calluna. Here's what each one receives, and why:

| Service | What it receives | Why |
|---|---|---|
| **Hetzner** (Germany) | Your account data and list contents; encrypted nightly backups | Hosting our server and storing backups |
| **Google Firebase Cloud Messaging** | The content needed to build your notification — currently this can include item names, list names, and the name (or email address, if you haven't set a display name) of the partner who triggered it — plus your device's push token, your Firebase installation ID, and delivery telemetry Google collects automatically | Delivering push notifications to your device |
| **Google Sign-In** | Your Google sign-in token, only if you choose to sign in with Google | Verifying your identity when you sign in with a Google account |
| **Resend** | Your email address, and the content of account emails (verification and password-reset links) | Sending account-related emails |

Hetzner is based in the EU, so no international transfer safeguard is needed there. Google and Resend are US-based; where personal data leaves the EU, we rely on the EU–US Data Privacy Framework or Standard Contractual Clauses. <!-- VERIFY: Resend DPA -->

To be direct about the Firebase point above: to show you a notification like "your partner added milk," we currently have to send the item name itself through Google's servers as part of the message. As of this policy, that's how notifications work.

## Server logs

Our server logs each request it receives, including the IP address it came from, for security and troubleshooting. These logs are deleted after 14 days. We don't run a separate web-server access log.

## Reporting an issue

If you use "Report an issue" in the app, it opens your own mail app with a message addressed to techtrest@pm.me. By default, a diagnostic log from your device is attached — you can remove it before sending. This email goes directly to the developer, not through our servers, and isn't covered by the retention periods below; it's handled like any email you choose to send us.

## Legal basis for processing (GDPR)

- **Creating an account, syncing your lists, sharing a list, and sending you notifications:** necessary to perform our contract with you — providing the app's features.
- **Server logs and other security measures:** our legitimate interest in keeping the service running and secure.

## How long we keep your data

- **Account data and list contents:** until you delete your account.
- **Backups:** up to 8 days.
- **Server logs:** 14 days.

To be honest about the gaps: we don't yet have an automatic cleanup process for pending invites that are never accepted or declined, or for accounts that never verify their email address. These can persist longer than they ideally should.

## Sharing a list

Calluna lets two people share a single shopping list. Once you invite someone or accept an invite, both people can see the list's contents, and either person can invite, leave, or remove the other at any time. There's no owner — both partners have equal control.

## Deleting your account

You can permanently delete your account and its data at any time, in the app: open the menu → Account → Delete account. You can also ask us to delete your account by emailing techtrest@pm.me.

Deleting your account is immediate. If you were sharing a list with a partner, the list and its contents stay available to them — only your own account and your membership in that list are removed. Your data is excluded from backups going forward right away, and fully purged from existing backups within 8 days.

If you used Calluna without ever creating an account, uninstalling the app removes all your data, since nothing was sent to our servers (see the caveat above about Firebase).

## Exporting your data

You can export a full copy of your shopping list at any time, in JSON or CSV format, from inside the app. This happens entirely on your device — nothing needs to be sent anywhere to generate your export.

## What we do not do

- We do not show ads.
- We do not sell your data, ever.
- We do not use analytics or advertising SDKs.
- We do not use any AI features today. If we introduce one — for example, recipe parsing — we'll update this policy first, mark the feature clearly as AI-powered in the app, and send only the minimum data necessary to our AI provider to process your request.

## Your rights

Under GDPR and similar regional laws, you have the right to access, correct, export, or delete your personal data. The in-app export and delete-account features let you exercise these rights yourself, at any time. If you need anything beyond what those features offer, contact us at techtrest@pm.me.

You also have the right to lodge a complaint with a data protection supervisory authority. In Poland, that's the Prezes Urzędu Ochrony Danych Osobowych (UODO). If you live elsewhere in the EU, you can contact your own country's supervisory authority instead.

## Children

Calluna is not intended for children under 16. We don't knowingly collect data from children under 16.

## Changes to this policy

We may update this policy as the app changes. We'll always update the date at the top of this page when we do.
