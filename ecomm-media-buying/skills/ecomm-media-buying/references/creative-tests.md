# Creative tests — daily ad verdicts (beta)

Beta: the verdicts are rules; what is executed follows the account's
`creative_mode` (Manual / Semi-auto — Auto is not available in the beta).
Step 6b of every full run, after the campaign verdicts and before the digest
closes. Everything is relative to the account's goal G and its zone lines.

## 1. Which campaigns are tests

- **Built by this skill** (launch plan, weekly creative flow): the campaign
  name says it — "<angle> - title test", "<angle> - thumbnail test" — the
  type is known from the build, and its group is the angle.
- **Any other managed running campaign** is checked once from its active
  ads (`list_items`): two or more active ads that all share **one title and
  differ in thumbnail** → a **thumbnail test**; all share **one thumbnail and
  differ in title** → a **title test**; anything else is not a test and its
  ads are never judged. Confirm once: "12345678 looks like a thumbnail test —
  1 title × 6 thumbnails. Treat it as one?" The user can also declare it:
  "treat 12345678 as a title test". Record every test in config:
  `creative_tests: 12345678: thumbnail test (<angle>) · 23456789: title test (<angle>)`.
- **The angle is the campaign group.** A title test and a thumbnail test
  belong to the same angle — and their winners pair up in §5 — when they sit
  in the same campaign group: for tests the skill built, the group is the
  angle itself ("LP3 Knee") and holds both tests; for a detected or declared
  test, the group it has in the config `groups:` block. A config that holds
  one group per test ("LP3 Knee - title test" and "LP3 Knee - thumbnail
  test" as two groups) is merged once, with the user's yes (SKILL.md Step 1,
  `client-requests.md` §4). A test in no group is asked once, with
  the confirmation: "Which landing page or angle is this test for? Its
  winners pair with the other test of the same angle." Record the answer as
  the test's angle and add the campaign to that group in `groups:`.
- **One format per angle group.** Tests pair only within one creative
  format: a native video title test pairs with the native video thumbnail
  test of the same angle (both in "<angle> - Video"), never with the image
  tests in "<angle>" (`media-plan.md`, One creative format per group).
- A campaign with a single active ad, a winning-ads campaign (recorded in
  `winners:`, named "<angle> - winning ads", in a "Winning ads - <angles> -
  <date>" group), or a proven-creatives / split / Mail / Display /
  Predictive campaign (by its group), is never a test.
- **Each test belongs to one batch**: the wave it was built in (`batches:`
  in config, `media-plan.md` Groups and names). Detected or declared tests
  form a batch of their own, dated the day they are confirmed as tests and
  shared by every test confirmed that day; record it in `batches:` too.
  Tests that were already running before v2.0.26 get their batch once, on
  the first run that finds them without one (SKILL.md Step 1).

## 2. Data per ad

For each test, `list_items` (the ads and their creation dates) and the
ad-level performance report (the dynamic report broken down by ad, or the
connector's ad breakdown report — whichever `get_dynamic_report_settings`
shows), **full days only, through yesterday**, on the main conversion rule
(campaign conversions when the report cannot split by rule, said in the
digest): spend, conversions, CPA, **since the ad went live** and L7D. A CPA
of 0 or blank is zero sales, never a winner. CTR and CPC never decide a
verdict. When config holds `conversion_overrides:` with ad ids for a day,
each ad's windows are adjusted exactly as the campaign's are
(`client-requests.md` §1, option 3): the file's count for that ad on that
day replaces Realize's, an ad with no row that day is 0 from the file, and
the ad's CPA for the window is its spend ÷ the adjusted count.

## 3. Verdicts — check Losing first, then Winning, else Not determined

Each ad is judged on its own numbers since it went live. Titles are compared
only inside the title test, thumbnails only inside the thumbnail test.

| Status | Rule (any line = that status) | At G = $100 | What follows |
|---|---|---|---|
| **Losing** | 1. zero sales after spend ≥ 3×G · 2. CPA above 2×G after spend ≥ 5×G · 3. 5 or more sales at a CPA above 20/17 of G · 4. *starved*: 14 or more full days live and still under 3×G spent | 1. no sales after $300 · 2. over $200 per sale after $500 · 3. 5+ sales at over $117.65 · 4. under $300 spent after two weeks | pause the ad (§4) |
| **Winning** | 5 or more sales at a CPA at or under 10/11 of G (zone A or B). *Deep winner:* 10 or more sales at or under 2/3 of G | 5+ sales at ≤ $90.91 · deep: 10+ at ≤ $66.67 | keep; its component is a winner (§5) |
| **Not determined yet** | fewer than 5 sales and no Losing line hit — or 5 or more sales at goal (CPA between 10/11 and 20/17 of G): a keeper, not a winner | under 5 sales · or 5+ sales at $90.91–117.65 | leave it running, re-read tomorrow; never paused, never copied |

Why 5 sales: below that one extra sale moves an ad's CPA by 20% or more.
These floors are the ads' own; a campaign is judged by the quick kill and
quick-pause in the main rulebook, whose floors are lower.

## 4. What happens to a losing ad

The only change ever made to an existing ad is **pausing it** — `is_active:
false` through the ad update tool, nothing else on the ad, ever. Never pause
the last active ad of a campaign; when a test is down to **fewer than 3
active ads**, ask once instead: "add new variants" (→ `growth.md` §1 flow,
the winner as the fixed element) or "leave the winners running as they are".

- **`creative_mode: manual`** — losing ads appear in the daily review as
  tick boxes, in an **ADS** section after the campaign actions, label
  `<campaign id> · <group> — PAUSE AD "<title or thumbnail name>"`,
  description in plain words (spent, sales, cost per sale against the goal,
  which Losing line, and "Unticked: keeps running"). Executed only after the
  tick and the final preview, like a campaign action.
- **`creative_mode: semi-auto`** — losing ads are paused by the run and
  listed first under "done today — ads", with the money they were spending.
- Auto — not available in the beta.
Default `creative_mode` = the account's mode, asked once at onboarding
("Same for pausing losing ads in your creative tests?"). A single question
about an ad is advisory and pauses nothing.

## 5. Winners

- A winning ad's **component** (its title in a title test, its thumbnail in
  a thumbnail test) is recorded in config under the test's angle:
  `winners: <angle>: titles [..] · thumbnails [..]`. It is the fixed element
  of the next test offered in the weekly creative ask (`growth.md` §1).
- **Winners campaign.** When the tests of one angle (one campaign group)
  have produced **at least 2 winning titles and at least 2 winning
  thumbnails** — the titles from its title test, the thumbnails from its
  thumbnail test; a single test can only ever deliver one side — offer once,
  with the money, in the review right after the ADS section. **The offer
  names only the winners** — the winning titles and thumbnails, each with
  its sales and cost per sale — and says nothing about the other ads of the
  tests (losing ads have their own pause boxes; undetermined ads are not
  mentioned in chat):
  > "<angle> has 2 winning titles and 3 winning thumbnails (5+ sales each,
  > at or under goal): [list]. This is what the test was for — shall I build
  > the two campaigns for it, on <platform(s) of the tests>: **<angle> -
  > winning ads** (run of network, those 2 titles × those 3 thumbnails, 6
  > ads) at $<opening> a day, and **<angle> - Yahoo & AOL** (the same 6 ads,
  > Yahoo and AOL placements only) at $<whitelist opening> a day? Total
  > $<sum> a day. They go into a new group, **Winning ads - <angle> -
  > <batch launch date>**, where the winners of this test round will sit
  > together."
  The last sentence only when the batch has no winners group yet; once it
  has one, "They join the winners group of this test round (renamed
  Winning ads - <its angles>, <angle> - <batch launch date>), next to <the
  winners campaigns already there>."
  **The offer is the pair, one question, one yes.** Never a pick list and
  never one campaign at a time: the client should see that the test was
  run to expand into these two campaigns together, not choose between
  them. With tests on both platforms that is four campaigns (the pair on
  desktop and the pair on Mobile+Tablet), shown as one list with one total.
  Nothing new is asked for: the landing page is the one the winning ads
  point to (their URLs from `list_items`; if the two tests point to
  different pages, ask once which one); platforms, geo, branding, tracking
  code and conversion rule are the tests' own. **Opening budgets: three quarters
  run of network, one quarter Yahoo & AOL.** The run-of-network campaign
  opens on the winners anchor in `opening-budgets.md`: $750 ÷ G, kept
  between 2.5× and 8× the goal, rounded to the nearest 50 ($150 at a $20
  goal, $400 at $50, $750 at $100 and at $200, $1,000 at $400). The Yahoo &
  AOL campaign opens on the whitelist anchor through the same rule: $250 ÷
  G ($150 at $20, $250 at $50 and at $100, $500 at $200, $1,000 at $400).
  The run-of-network campaign is never the smaller one: it buys Yahoo and
  AOL on its own, and the whitelist campaign adds a measured extra on top,
  not a second engine. One pair per platform the tests ran on. The budgets are
  proposed, never asked open-ended; an answer with other numbers ("yes, at
  $400 and $600") is used as given. **Winners sit in their batch's group,
  not in the angle's test group.** One group per batch of tests, **"Winning
  ads - <angles> - <batch launch date>"** ("Winning ads - LP1 Comfort - 06
  Nov 2026", the date from `batches:`), shared by every angle of that batch:
  the first angle of the batch to qualify opens it — created on the user's
  yes to the offer, exactly as in `media-plan.md` Realize groups (CUSTOM
  distribution, no group budget or cap, the main conversion rule), its id
  written to `batches:` and `groups:` — and every later winners campaign of
  the same batch joins it, found by that id (never by name). When a new
  angle joins, the group is renamed to add it ("Winning ads - LP1 Comfort,
  LP2 Heel - 06 Nov 2026"; more than three angles → the first three and
  "+N more"): `update_campaign_group` with `name` alone, after the user's
  yes to the offer and before the campaigns are built; a failed rename is
  noted in the digest and the build goes on. Example: a batch tests angles
  A, B and C (6 campaigns); A and B reach 2 winning titles and 2 winning
  thumbnails, C does not → A's pair opens "Winning ads - A - <date>", B's
  pair joins it and the group becomes "Winning ads - A, B - <date>" (4
  campaigns, 8 with a platform split); C gets no winners campaign; the next
  batch opens its own group. Winners of video tests open "Winning video ads
  - <angles> - <date>" instead (one format per group). Expect one or two
  winners pairs per batch; an angle that never reaches 2
  titles and 2 thumbnails gets none. The next batch opens its own new
  winners group; winners of different batches are never
  mixed. When an angle's winners came from tests of two batches (a later
  wave added a test to an older angle), the campaigns go to the winners
  group of the newest batch that contributed a winner. Names `<angle> -
  winning ads[ - <platform>]` and `<angle> - Yahoo & AOL[ - <platform>]`,
  both in the winners group, with `campaign_group_id` on `create_campaign`;
  when the group cannot be created, both are built without it (`media-plan.md`,
  When a group cannot be created). Built paused as in `media-plan.md`,
  with `create_native_item` for every title × thumbnail pair in each; the
  Yahoo & AOL campaign carries `publisher_targeting` INCLUDE with the Yahoo
  and AOL publishers (`growth.md` §2 says where the list comes from). The
  test campaigns keep running; from its first full day each new campaign is
  judged by the campaign cube like any other. **If the account cannot carry
  a publisher list yet** (`create_campaign` answers 403 with
  `publisher_targeting`), build the run-of-network campaign as planned and
  the Yahoo & AOL campaign too, without its placement list, paused, with
  its ads — the 403 ladder in `media-plan.md` — record
  `winners: … yahoo <id> WL pending AM <date>`, and tell the user once to
  ask their account manager to apply the Yahoo & AOL whitelist to campaign
  <id>; it is activated only when the user says the list is on it.
  **Manual and Semi-auto: a yes/no question**, never built by the run on
  its own. Yes → build both; no → `winners: … campaigns declined <date>`, offered
  again only when the set of winners grows; no answer (an unattended run)
  → written to the digest as `winners campaign: offer open`, shown again by
  the day's later firings and by the next full run until answered. **Auto
  (not available in the beta): the run builds it and activates it** at the
  opening budget, and the digest reports it under "done today". One side
  only (titles but no thumbnails yet, or the reverse) → nothing is offered
  and nothing is said in chat; the digest file notes what is still missing.
- The Yahoo & AOL campaign (`growth.md` §2) and the proven ads for any
  later build (`growth.md` §5) read the same winners.

## 6. What is recorded

Config: `creative_mode`, `creative_tests:`, `winners:` (with the
winning-ads campaign id once built), and in `batches:` each batch's winners
group id with its campaign ids. **Digest file**, section **Ads in
creative tests**: per test, one line per ad — component, spent, sales,
cost per sale against the goal, status (Losing line n / Winning / Not
determined) — then what was ticked, paused or declined, and the
winners-campaign offer and its answer (built / declined / open) when one
was made. **In chat** only two things from the tests ever appear: the
pause boxes for losing ads (§4) and the winners offer (§5); the per-ad
table and the "not determined" ads stay in the digest file.
