# Phishing Simulation & Security Awareness Report

**Task:** Conduct a Phishing Simulation
**Tool used:** [GoPhish](https://getgophish.com/) (open-source phishing simulation framework)
**Scope:** Self-administered test against two personally-owned test email accounts
**Date:** October 11, 2026

> **Note on scope:** This simulation was run exclusively against test accounts owned
> and controlled by the person conducting the exercise. No third-party accounts,
> coworkers, or external recipients were targeted at any point. Email addresses
> and local usernames visible in the original tool output have been redacted from
> all screenshots in this repository before publishing.

---

## 1. Executive Summary

This project simulates a realistic phishing attack to evaluate how email security
controls (spam filtering) and an end user respond to a credential-theft attempt.
Using GoPhish, a single phishing email impersonating a "Google Account Security"
alert was sent to two self-owned test accounts. The exercise tracked the full
engagement funnel — delivery, opens, clicks, data submission, and user reporting —
and the results were used to evaluate both automated and human defenses.

## 2. Methodology

| Step | Detail |
|---|---|
| Platform | GoPhish v0.12.1, run locally (`127.0.0.1:3333` admin panel, port 80 phishing server) |
| Sending profile | Gmail SMTP relay |
| Target group | 2 self-owned test Gmail accounts |
| Email pretext | Fake "Google Account Security Team" alert claiming a login attempt from Russia |
| Social engineering techniques used | Brand impersonation, urgency (2-hour deadline), fear/loss framing (threat of account deletion) |
| Landing page | Neutral training-notice page (no credential harvesting form used) |
| Tracking | GoPhish's built-in open/click/submit/report event tracking |

See `screenshots/01-environment-setup.png` through `05-campaign-launched.png` for
the full setup and launch sequence.

## 3. Results

| Metric | Count |
|---|---|
| Emails Sent | 2 |
| Emails Opened (tracked) | 0 |
| Links Clicked | 0 |
| Data Submitted | 0 |
| Reported as Phishing | 0 |

Full funnel data: `screenshots/06-results-dashboard.png` and
`screenshots/07-results-details.png`.

### Key finding: automated spam filtering intercepted the email

Gmail's spam filter flagged the simulated email automatically, with the explicit
system message:

> "This message is similar to messages that were identified as spam in the past."

This is a notable result in itself — the email never reached the primary inbox of
either test account. See `screenshots/08-spam-filter-detection.png`.

This also explains why **Email Opened = 0** in the GoPhish dashboard despite the
message having been manually opened from the Spam folder during review: Gmail
does not load remote tracking pixels for messages inside Spam by default, which
suppressed the open-tracking signal.

### The email content as received

`screenshots/09-phishing-email-content.png` shows the message as it rendered in
Gmail, including the spoofed sender identity and the lookalike destination link.

## 4. Phishing Indicators Present in the Test Email

| Indicator | Present? | Detail |
|---|---|---|
| Brand impersonation | ✅ | Claimed to be from "The Google Account Team" |
| Lookalike / mismatched domain | ✅ | Link pointed to a non-Google domain, not `accounts.google.com` |
| Urgency / artificial deadline | ✅ | "within the next 2 hours" |
| Fear / loss-based pressure | ✅ | Threatened permanent deletion of the account |
| Generic greeting | ✅ | "Dear User" rather than the recipient's actual name |
| Call-to-action link | ✅ | Single prominent link styled as the only resolution path |

## 5. Analysis

- **Automated defenses performed well.** Gmail's reputation/heuristic-based spam
  filtering caught the message before a human ever had to make a judgment call,
  demonstrating the value of provider-side filtering as a first line of defense.
- **Human-layer awareness is still the necessary backstop.** Spam filters are not
  perfect — a sufficiently well-crafted lookalike email can still land in an
  inbox. The indicators in Section 4 are exactly what an end user needs to
  recognize independently of whether a filter catches the message.
- **Low sample size.** This test used 2 self-owned accounts rather than a
  representative organizational sample, so results describe filter behavior and
  message construction more than broad human susceptibility trends.

## 6. Recommendations

1. **Verify sender domains**, not just display names — "The Google Account Team"
   is not a real, verifiable identity.
2. **Hover over links before clicking** to confirm the destination domain matches
   the claimed organization.
3. **Treat urgency and deadline-based pressure as a red flag**, not a reason to
   act quickly — legitimate security teams don't threaten account deletion over
   email.
4. **Navigate to accounts directly** (typing the URL or using a saved bookmark)
   rather than clicking email links for anything password- or security-related.
5. **Report suspicious emails** using the mail provider's "Report phishing"
   feature even when the filter has already moved it to Spam, to reinforce
   provider-side detection models.

## 7. Repository Contents

```
├── README.md                          # This report
└── screenshots/
    ├── 01-environment-setup.png       # GoPhish server startup
    ├── 02-sending-profile.png         # SMTP sending profile configuration
    ├── 03-target-group.png            # Self-test target group setup
    ├── 04-campaign-configuration.png  # Campaign setup (template, landing page, URL)
    ├── 05-campaign-launched.png       # Campaign launch confirmation
    ├── 06-results-dashboard.png       # Engagement funnel results
    ├── 07-results-details.png        # Per-recipient delivery status
    ├── 08-spam-filter-detection.png   # Gmail spam filter catching the email
    └── 09-phishing-email-content.png  # Rendered phishing email content
```

**Privacy note:** All personally identifying details (local usernames, full test
account email addresses) visible in the original tool screenshots have been
redacted prior to publishing this repository.
