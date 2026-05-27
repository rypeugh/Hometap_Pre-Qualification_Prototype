# Screen Map — Hometap Prototype

Full screen inventory for `prototype/index.html`. Navigation via `goTo(n)` — HTML IDs do not match step order.

## Pre-Qualification Phase (screens 1–14)

| Step | HTML ID | Screen |
|------|---------|--------|
| 1 | screen-1 | **Interstitial** — "Get up to $600k from your home equity. No monthly payments." + 01/02/03 steps + Trustpilot proof + "See my offer →" CTA |
| 2 | screen-3 | **Amount** — "How much money are you looking for?" — auto-advances |
| 3 | screen-4 | **Urgency** — "How soon do you need the money?" — auto-advances |
| 4 | screen-2 | **Use case** — "How do you plan to use the money?" — auto-advances |
| 5 | screen-6 | **HEI Interstitial** — personalized dark screen; header/sub-text/quote swap by `selectedKey` |
| 6 | screen-7 | **Address** — "Where is your home located?" — mock autocomplete |
| 7 | screen-8 | **Property type** — "How would you describe it?" — "Investment property" → exit; auto-advances |
| 8 | screen-9 | **Debt** — "What is the estimated debt on your home?" |
| 9 | screen-10 | **Name** — First/Last stacked vertically |
| 10 | screen-11 | **Phone** + TCPA disclaimer |
| 11 | screen-15 | **2FA Verification** — 6-digit OTP, auto-advances on completion |
| 12 | screen-12 | **Email** — manual entry + Google/Apple social auth options |
| 13 | screen-13 | **Loading** — 4 items, auto-advances after ~3.8s |
| 14 | screen-14 | **Results** — plain white header, `--purple` amount, dynamic CTA ("Get my $X →"), two pills, manager card, borderless next steps |

**Exit screen (screen-exit):** Investment property ineligible — graceful dead-end with return home.

## Application Phase (screens 16–29)

Progress bar resets to 0% at Dashboard and advances to 100% at Terms & Submit.

| Step | HTML ID | Screen |
|------|---------|--------|
| — | screen-16 | **Dashboard** — estimated amount, "Finish your application" card; submitted state updates in-place |
| 1/13 | screen-17 | **Marital Status** — dropdown select, Continue gated |
| 2/13 | screen-18 | **Mailing Address** — Yes/No; Yes auto-advances, No reveals address input |
| 3/13 | screen-19 | **Bankruptcy** — Yes/No, auto-advances |
| 4/13 | screen-20 | **Felony conviction** — Yes/No, auto-advances |
| 5/13 | screen-21 | **Pre-foreclosure notice** — Yes/No, auto-advances |
| 6/13 | screen-22 | **Pre-foreclosure sale/short sale** — Yes/No, auto-advances |
| 7/13 | screen-23 | **Title transfer in lieu of foreclosure** — Yes/No, auto-advances |
| 8/13 | screen-24 | **Co-borrower on title** — Yes/No, auto-advances; address injected in heading |
| 9/13 | screen-25 | **Title in trust** — Yes/No, auto-advances; address injected in heading |
| 10/13 | screen-26 | **Outstanding mortgages/liens** — Yes/No, auto-advances; address injected in heading |
| 11/13 | screen-27 | **Employment details** — dropdown + income input; first name injected in heading; Continue gated on both |
| 12/13 | screen-28 | **SSN** — masked input, show/hide toggle, security note; Continue gated on 9 digits |
| 13/13 | screen-29 | **Accept Terms and Submit** — checkbox gates Submit; on submit → `appSubmitted = true`, `goTo(16)` |

## pct Map (full)

```js
// Pre-qual phase
{ 1:7, 3:15, 4:23, 2:31, 6:39, 7:46, 8:54, 9:62, 10:69, 11:77, 15:81, 12:85, 13:92, 14:100,
// Application phase (resets at dashboard)
  16:0, 17:7, 18:14, 19:21, 20:29, 21:36, 22:43, 23:50, 24:57, 25:64, 26:71, 27:79, 28:86, 29:100 }
```
