# Changelog — Ecomm media buying (client zip = ecomm-media-buying-vX.Y.zip)

## v2.0.4 — 2026-10-15
- Auto mode is not available in the alpha: never offered, never switched on (the four-week unlock is gone). Only Manual and Semi-auto exist for alpha accounts.

## v2.0.3 — 2026-10-15
- Daily run at 10:00 in the account's time zone (was 11:00), confirmed at setup. A run the machine missed is skipped, with a one-line note, and the next morning reviews; no same-day catch-up review.
- Install note and onboarding explain Claude's own permission prompt ("Yes, and don't ask again") in plain words and separate it from the media buyer's Manual / Semi-auto mode; nobody needs Claude's "Auto" permission setting.

## v2.0.2 — 2026-10-14
- Conversion file: the install note now has its own section ("If you see more sales than Realize does") with the four things to send — purchases with date and time, the campaign id or UTM campaign name on every row, the time zone, the period through yesterday — and says this is the media buyer's feature, not Realize's.
- The file's times are converted from the file's time zone (asked once, stored as `conversion_file_tz`) to the account's, and each purchase is counted on its day of purchase in the account's time zone; the reconciliation says when the offset moves a day.

## v2.0.1 — 2026-10-14
- Network in scope: give a network id at setup and choose "all accounts in the network" or only some; campaigns are listed through the network and always judged and changed through their own advertiser account; one goal / mode / maximum for the network or per account; the media plan asks which account it is for.
- The cube speaks CPA, not ROAS: zones are shares of your goal (deep winner ≤ 2/3, winner to 10/11, at goal to 20/17, over goal to 4/3, blowout above), stored per account as zone lines; every threshold, card and digest line is a cost per sale against the goal. Same lines as before, same verdicts.
- The goal, the purchase event, the mode and the maximum are only ever the advertiser's answers; a missing one is asked for, never estimated or filled with an example. "No signal" now states the amount needed ("at least 7× your goal, $700, in the last 7 days") and each campaign's spent / needed.
- Account folder: setup names the folder it will write in; the plugin's own folder is refused.
- The agent reads the Realize connector only; it no longer loads other skills.

## v2.0.0 — 2026-10-13
- Packaging: the skill ships as a plugin (`ecomm-media-buying`) with one agent, `media-buyer`, and its own Realize connection; installed from a private marketplace with auto-update, or from a zip. Run it as the session's agent (`claude --agent media-buyer`) or as a worker from any session.
- Scope and stop: the accounts to manage are listed at onboarding in `config/scope.md`; nothing outside that list is ever read or changed. "Stop" asks: pause the suggestions / change the accounts / stop entirely.
- Modes at onboarding: Manual or Semi-auto; Auto is offered 28 days after onboarding.
- Daily schedule created at onboarding (desktop scheduled task; catch-up run after a missed day); checklist for the client's side; Remote Control suggested for Manual.
- Campaign groups: one named group per test family, names `<group key> - <variable> - <platform>`, kept in config and shown as headings; Realize's automatic per-campaign group explained; no group budgets, never the budget allocator.
- Growth (beta): weekly creative-test offer on the first run of the week; publisher split (Yahoo & AOL / Non-Yahoo × desktop / mobile) when a creative test has winners, Mail two weeks later; after the account's good week: display (300×250 and 300×600 first), then a tier-1 predictive audience once 100 purchases in 30 days are reached.
- Client requests (beta): conversion file reconciliation (needs campaign ids or UTM names) with an optional file-as-source mode; new country / market / product as a wave of the launch plan; CPA goal change with a dry run before any write; client-instructed actions logged as such.
- Data pulled: + last-30-day account total (predictive eligibility); + ad-level report for the winning ads.

## v1.7 — 2026-10-13
- Opening budget rule: standard creative-test anchor $700 → $500 (5× at a $100 goal); the multiple is now kept between 2.5× and 8× (was 4–8×), so a $40–60 goal opens at 8× and a $400 goal at 2.5× ($1,000). Other surface anchors unchanged. Day-4 first verdict noted for openings below 3.5×.

## v1.6 — 2026-10-08
- Exceptional start: raise on the morning after 2 full days when ≥5×G spent, ROAS 150%+ since launch, each day 110%+ (the only raise path on day 2; grid raises from day 3).
- Utilization gate back, at 70% of the daily budget spent yesterday.
- Zone E line 67% → 75%; panels D and E one step stricter; raise waits 2/3/5 days; cut above the top level −25%.
- Approval flow: one "Approve all / Pick one by one / Skip today" question, then 4-campaign boxes in PAUSE → CUT → RAISE order with a net line; holds as one FYI line; no filler rows.
- First-two-weeks calendar in Step 2a and a timing paragraph in the install note.

## v1.5 — 2026-09-30
- Daily runs; four-code cube (R/H/W/X) with exit and warning chains; per-campaign tick boxes in plain language; budget levels as CPA multiples; cut table; 4–8× opening rule; Semi-auto mode; tracking emphasis and freeze guardrail; "skip media plan building".
