# Changelog — Ecomm media buying (client zip = ecomm-media-buying-vX.Y.zip)

## v2.0.18 — 2026-10-17
- The winners offer is the pair: for each platform the angle's tests ran on, "<angle> - winning ads" (run of network, standard opening budget) and "<angle> - Yahoo & AOL" (same ads, Yahoo and AOL placements only, whitelist opening budget), in the angle's group, one question with one total and one yes — never a pick list. If the account cannot carry publisher lists yet, the run-of-network campaign is built and the Yahoo one is recorded as pending permission; the growth rule offers it as soon as a list is accepted. The growth rule's Yahoo & AOL offer is now the catch-up path for campaigns that have winners but no Yahoo twin, and the expansion path for advertisers who run no A/B tests (winners picked from the running campaign's own ads, confirmed by the advertiser); the weekly creative ask then proposes a proper test built around their best ad.

## v2.0.17 — 2026-10-17
- Publisher split replaced: a campaign with winning ads is offered one Yahoo & AOL whitelist campaign per platform it runs on (same ads, Yahoo and AOL placements only, whitelist opening budget, in the angle's own group, named "<angle> - Yahoo & AOL - <platform>"). The "everything except Yahoo" pair is gone. The offer waits, with a one-line explanation, when the account cannot carry publisher lists yet. Mail still follows a winning Yahoo & AOL campaign after 14 days.

## v2.0.16 — 2026-10-17
- One campaign group per creative angle, spelled out everywhere: the group is named after the angle ("LP3 Knee") and holds its title test, thumbnail test and winning-ads campaign; never a group per test. New client request "these campaigns are one creative angle / merge groups X and Y" (§4 regrouping: confirm the new block, carry chains and test records, no re-judging). A config that was set up with a group per test gets a one-time merge offer before the next review. Regrouping changes the plugin's own map only; Realize's automatic per-campaign groups cannot be moved through the connector.

## v2.0.15 — 2026-10-16
- Winning-ads campaign: one campaign group = one creative angle, so the new campaign joins the tests' own group as "<angle> - winning ads" (no separate winners group). The offer names only the winning titles and thumbnails; losing ads keep their pause boxes and undetermined ads are never mentioned in chat (the per-ad table lives in the digest file). Manual and Semi-auto ask; Auto, when available, builds and activates it at the opening budget.

## v2.0.14 — 2026-10-16
- Conversion file, per-day override: an attributed file (orders already matched to Realize — attribution date, campaign id, ad id or click id; reference layout `date, campaign_id, campaign_name, item_id, item_name, order_id, purchases, purchase_value_usd, click_id`) counts the way Realize counts, so "for 15 October go by my file, the rest by Realize" now works — `conversion_overrides:` in config, that day's count replaced in every window (yesterday, last 3 / 7 days, since launch, chain days, and per ad in creative tests), CPA = Realize spend ÷ the adjusted count, every card touching the day says "from your file: n sales (Realize m)". A purchase file (order dates) stays whole-source only, and "I can't mix sources" is reserved for it. Install note rewritten around the two kinds of file.

## v2.0.13 — 2026-10-16
- Winners campaign made precise: the angle is the campaign group (a title test and a thumbnail test pair up when they share a group; a test in no group is asked its angle once); the offer needs both tests of the group (2+ winning titles from the title test, 2+ winning thumbnails from the thumbnail test); landing page, platforms, geo, tracking and conversion rule are taken from the tests; the opening budget is spelled out ($500 ÷ goal, 2.5–8×, nearest 50 — $150 at $20, $500 at $100, $1,000 at $400) and can be answered with another number; the offer is a yes/no question in every mode, never built by the run on its own, and an unanswered offer stays open and is re-asked (hourly that day, then each full run).

## v2.0.12 — 2026-10-15
- Winners campaign opens like any new test campaign (2.5–8× the goal by the goal's size), in its own campaign group per angle ("<angle> winners").

## v2.0.11 — 2026-10-15
- Creative tests (beta): daily verdict per ad inside title and thumbnail tests — losing (no sales after 3×G, over 2×G per sale after 5×G, 5+ sales in zone D/E, or starved 14 days), winning (5+ sales in zone A/B; deep 10+ in zone A), otherwise not determined. Losing ads are paused per a new `creative_mode` (Manual tick / Semi-auto by the run); the last active ad is never paused; under 3 active ads the run asks for variants. Tests are recognised from the build or detected from the ads (1 title × N thumbnails / 1 thumbnail × N titles) and confirmed once; "treat <id> as a title test" declares one. Winning components are recorded per angle; with 2+ winning titles and 2+ winning thumbnails a winners campaign is offered (proven anchor, 2.5–8× the goal). The agent may now pause an ad and nothing else on it.

## v2.0.10 — 2026-10-15
- The daily task fires every hour from 10:00 to 19:00 account time: the first firing with the machine on does the review, later firings re-ask only what is still pending (no re-judging), and a day with no firing in the window is skipped. Replaces the single 10:00 run and the three-hour catch-up rule.

## v2.0.9 — 2026-10-15
- Install note: the access token is stored once with git's credential helper before adding the plugin source (Claude Code never prompts for a password), with a one-line check; the source and the plugin can then be added from inside a Claude session.

## v2.0.8 — 2026-10-15
- Self-check: a plain Claude session that finds the skill without the media-buyer agent does one thing only — writes the agent line into the folder's settings and asks the user to reopen the folder. "check setup" prints a fixed seven-line status (agent, rulebook version, Realize connection, folder, scope, daily task, mode) with OK or the fix on each line; the install note makes it step 5.

## v2.0.7 — 2026-10-15
- The daily task is verified after creation (listed back by id) before setup records it; if it could not be created, the settings say "NOT CREATED" and the next session retries.
- Only the plugin's own Realize connection is used; if its tools are missing the run stops and asks for the sign-in instead of using another connection.

## v2.0.6 — 2026-10-15
- Small goals (under $50): the budget ladder starts at 5× the goal instead of 3× (14 levels, e.g. $100 … $2,000 at a $20 goal); the opening budget stays 8× the goal for these.

## v2.0.5 — 2026-10-15
- Thumbnails from Google Drive: a folder or image shared as "anyone with the link" is accepted for the launch plan and the weekly creative test; sub-folders are angles, the ratio is read from the file name, the list is confirmed before building. The agent gets web access for those Drive links only; the install note pre-allows that domain in the folder settings.
- Mode question reworded: Manual — "You'll get our recommendations; only what you tick is applied in Realize"; Semi-auto — "We handle the cuts (decreasing a campaign's daily budget) and the pauses; you approve the budget raises by ticking them."

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
