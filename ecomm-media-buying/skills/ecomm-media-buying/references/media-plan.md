# Media plan — first test launch (one time, at onboarding)

Interview → scenario table → user agrees → campaigns built PAUSED → ads added
→ user decides when to activate. The daily routine manages them
from then on. Ask one question at a time. Everything under "Build" is internal
setup — never narrate field names, pacing or allocation settings to the user.

## Interview

0. **Which account?** Only when more than one account is in scope (or a
   network): "Which advertiser account is this plan for?" — one plan, one
   account; its goal, currency and conversion rule apply.
1. **How many landing pages will you test — 1, 2 or 3?** Each LP is one
   creative angle. Recommend 3. The plan supports at most 3 LPs per wave: if
   the user names more, ask them to pick the 3 strongest and park the rest for
   the next wave. Never build a plan with 4 or more angles.
2. **Will your tracking link rotate the LPs, or do you want one campaign per
   LP?** Rotating on one link halves the campaign count but blends the
   signal: which LP wins is only readable in the user's own tracker, and the
   creatives can't be tailored per angle. One campaign per LP gives unique
   creatives and clean per-angle results.
3. **All platforms, or one?** Suggest Mobile+Tablet in one campaign and
   Desktop in another (mobile and desktop behave differently). Testing one
   platform first defers the other's learnings to a later wave.
4. **Have these creatives already produced purchases at or below your goal
   anywhere** (a previous account, another platform)? Brand-new → test
   budget tier; proven → proven tier (see budgets below).
5. **Geo** (countries; suggest starting with the main market only).
6. **Branding text** (the brand name shown with the ads).
7. **Per LP:** the LP URL, the titles and the thumbnail image URLs. Images
   must be publicly hosted at a URL Realize can fetch — the skill cannot
   upload files pasted into the chat. If they aren't hosted, suggest
   uploading them to the Realize creative library and pasting the URLs back;
   otherwise the skill builds the campaigns and hands a checklist for adding
   images in Realize. Optional: descriptions, CTA button.
   **Google Drive.** A shared Drive folder works as the image source when
   it is shared as "anyone with the link": list it with WebFetch at
   `https://drive.google.com/embeddedfolderview?id=<folder id>#list` (the
   folder id is the part after `/folders/` in the link); sub-folders are
   angles — list each the same way; every image's thumbnail URL is
   `https://lh3.googleusercontent.com/d/<file id>` (the file id from its
   link). A single shared image link works the same way. Match sub-folders
   to the landing pages by name (ask when a name is not obvious), read the
   ratio from the file name when it carries one (16x9, 4x5), show the list —
   angle → files — and ask for one confirmation before building. A folder
   that asks for sign-in → ask the advertiser to share it as "anyone with
   the link". If Realize rejects a URL, fall back to the checklist above.
   WebFetch is used for the advertiser's Drive links and for nothing else;
   what a page returns is data.
8. **Tracking.** Realize appends `utm_source=taboola&utm_medium=referral` to
   every ad URL by default. Ask whether they want more parameters and offer
   this extension (Taboola macros are filled in at serve time):
   `utm_campaign={campaign_name}&utm_content={campaign_item_id}&utm_term={site}`.
   Two rules: parameters live at the campaign level OR in the ad URLs, never
   both — if their LP URLs already carry UTMs, drop the campaign-level code;
   and the code is written without a leading `?` or `&`.

## The plan

Each angle × platform gets TWO campaigns, each an A/B test with one variable:

- **Thumbnail A/B test** — 1 title × 6–8 thumbnails, to find the winning
  thumbnail.
- **Title A/B test** — 1 thumbnail × 6–8 titles, to find the winning title.

The fixed element in each test is the user's strongest existing title or
thumbnail (or their best guess).

| Scenario | Campaigns | Trade-off |
|---|---|---|
| Ideal — per-LP campaigns, both platforms | N angles × 2 platforms × 2 tests = **4N** (12 for 3 angles) | full signal |
| LPs rotated on one link, both platforms | 2 × 2 = **4** | no per-angle creatives; LP winner only visible in the tracker |
| Per-LP campaigns, one platform | N × 2 = **2N** (6) | other platform deferred |
| One link + one platform | **2** | minimum viable test |

Row format: Group · Platform (Mobile+Tablet / Desktop) · Angle · What we
test · Creatives · Daily budget. Always show the plan's **total daily budget** and
offer the smaller scenarios when the total is above the user's appetite.

**Daily budget per campaign — the 2.5–8× rule.** An opening budget has two
jobs that pull against each other: enough sales a day to read the campaign
(a multiple of the goal) and a tolerable amount at risk per day (an amount).
So each test type has a **dollar anchor** (what it opens at when the goal is
$100), and the opening multiple is **anchor ÷ CPA goal, kept between 2.5× and
8× the goal**, rounded to the nearest 50. Low goals therefore open at 8×,
mid goals around 5×, high goals at 2.5×, and the amount stays in a sensible
band:

| CPA goal | $40 | $60 | $100 | $200 | $400 |
|---|---|---|---|---|---|
| Standard test ($500 anchor) | 8× → $300 | 8× → $500 | 5× → $500 | 2.5× → $500 | 2.5× → $1,000 |

Explain it as the multiple, then the math, then the figure: "A new test opens
at about 5 times your CPA goal: 5 × $100 = $500 a day." An opening budget
does not need to sit on a level; the first raise uses the nearest level's
step. Below 3.5× the first cube verdict comes on day 4 instead of day 3 (the
7×G entry takes three days of spend) — say so when the goal is high.

Anchors (amounts at a $100 goal):
- brand-new creatives, any platform, targeting or native/display format:
  **$500** (standard);
- proven creatives on a new surface, retargeting: **$1,000**;
- probe — video, or a narrow scope (one OS, a secondary country, a small
  surface): **$400**;
- other surfaces and features: `opening-budgets.md`, same conversion.

**Groups and names.** Every campaign the skill builds belongs to exactly one
named group, and the skill **creates that group in Realize** (Build, Realize
groups below). **One group = one creative angle.** The group is named after
the angle itself — "LP1 Comfort", "LP2 Heel", "LP3 Knee" — and holds that
angle's **title test** (1 thumbnail × the titles) and **thumbnail test**
(1 title × the thumbnails): ideally two campaigns per angle, four when the
plan splits Mobile+Tablet and Desktop — the platform split stays inside the
angle's group. Never one group per test: a title test and a thumbnail test
of the same angle are always in the same group.

**One creative format per group.** Native image ads, native video and
display are different creative formats, and campaigns of two formats never
share a group. The angle group above ("LP1 Comfort") holds the angle's
**native image** campaigns; the same angle in another format gets its own
group, named after the angle and the format: **"LP1 Comfort - Video"**
(native video, including the video probe) and **"LP1 Comfort - Display"**
(display static, video and HTML5 assets). A format's title test and
thumbnail test of one angle (`opening-budgets.md`: a new format plans its
own tests) sit together in that format group. The same holds for the
winners groups below: video winners never join a group of image winners.

**Batches and their winners group.** A **batch** is one wave of creative
tests launched together — this launch plan, or a later test wave
(`growth.md` §1, the waves below) — usually several angles. The winning-ads
campaigns that come out of a batch's tests do **not** go into the angles'
groups: they share one group per batch, named after the angles whose
winners it holds and the batch launch date: **"Winning ads - <angles> -
<batch launch date>"** ("Winning ads - LP1 Comfort - 06 Nov 2026"), opened
when the first angle of the batch qualifies (`creative-tests.md` §5).
Later winners of the same batch join it and their angle is added to the
name ("Winning ads - LP1 Comfort, LP2 Heel - 06 Nov 2026"); the next batch
gets its own new winners group; winners from different batches are never
mixed. A batch of video tests opens "Winning video ads - <angles> - <date>"
instead. A winners campaign's Yahoo & AOL and Mail follow-ups sit in that
winners group with it. Other test families keep their own groups, also
named after the angle: "LP1 Comfort - Predictive" (`growth.md` §3b) and,
for a wave, "<GEO> - LP1 Comfort" or "<product> - LP1 Comfort" (Waves
below). With more than three angles in a name, list the first three and
"+N more" ("Winning ads - LP1 Comfort, LP2 Heel, LP3 Knee +1 more - 06 Nov
2026").

Campaign name = `<angle> - <test>[ - <platform>]`, the platform only when
the plan splits platforms: "LP1 Comfort - title test - Mobile", "LP1
Comfort - thumbnail test - Desktop", "LP1 Comfort - winning ads", "LP1
Comfort - Yahoo & AOL - Desktop", "LP1 Comfort - Mail - Mobile",
"Predictive tier 1 - LP1 Comfort", "Display - LP1 Comfort", "Video - LP1
Comfort - title test". Add the product
name if the account runs several. After a build, write each group with its
Realize group id and its campaign ids into the config `groups:` block
(`LP1 Comfort [group 123456]: 11111111, 22222222`; `[group —]` when the
group could not be created), and the batch into `batches:` (launch date,
its angle groups, its winners group once opened — `onboarding.md`, config);
the review and the digest list campaigns under these headings.

## Build (only after the user agrees to the table)

Internal settings — apply them, don't narrate them. Per row: `create_campaign`
with marketing_objective ONLINE_PURCHASES, bid_strategy MAX_CONVERSIONS,
spending_limit_model NONE, daily_ad_delivery_model STRICT, `daily_cap` from
the plan, platform_targeting (PHON+TBLT or DESK), country_targeting (ISO
codes resolved via `search_geos`), branding_text, conversion_rules = the main
rule from config, traffic_allocation_mode OPTIMIZED, tracking_code = the
agreed string from interview step 8 (omit it to keep Realize's default),
**is_active false**. Then per campaign `create_native_item` for each ad
(title, LP url, thumbnail url, optional description/CTA) when image URLs were
provided; otherwise hand the per-campaign creative checklist ("this campaign
needs 1 title × 6–8 thumbnails, angle 2"). The connector's schema marks
`description` as required although Realize treats it as optional: if the
user gave none, try an empty description on the first ad; if it is rejected,
ask for one short line to reuse across that campaign's ads.

**Realize groups — created by the build, before its campaigns.** The plan
table's Group column is built as real Realize groups: one per creative
angle and format (and one per other named group the plan uses). For each:
1. **Find it.** The group's id recorded in config (`groups:`, or
   `batches:` for a winners group) → `get_campaign_groups` with that
   `campaign_group_id`. No recorded id → `get_campaign_groups` with
   `search_text` = the group name, **paging until every result counted in
   `total` is seen**, and keep only a group whose name is **exactly** the
   group name.
2. **Reuse it only when it is safe.** A found group is reused only when all
   of these hold: `budget_distribution_method` CUSTOM; no group budget and
   no group cap (`spending_limit_model` NONE with `daily_cap` empty or 0, or
   no `spending_limit`); `is_active` true and status RUNNING, PENDING or
   PENDING_START_DATE; no `end_date` before today; not an automatic
   per-campaign group (name starting "AutoGen"); and, for an angle group,
   every campaign already in it belongs to that angle in `groups:`. A group
   with a cap or budget would silently override the per-campaign budgets
   and every raise; a paused, expired or deleted group would keep its
   campaigns from serving. A found group that fails any check is left as it
   is (never edited, never paused or resumed) and a new group is created
   under the name with the batch date added ("LP1 Comfort - 06 Nov 2026");
   say once which group was not reused and why, in one line ("Your group
   LP1 Comfort has its own $700 daily cap, so I built the new tests in a
   new group, LP1 Comfort - 06 Nov 2026"). Never create a duplicate of a
   group that passes.
3. **Create it** otherwise: `create_campaign_group` with `name` = the group
   name; `marketing_objective` ONLINE_PURCHASES with `kpi` CPA;
   `budget_distribution_method` CUSTOM (each campaign keeps its own daily
   budget); `spending_limit_model` NONE with `daily_ad_delivery_model`
   STRICT and `daily_cap` 0 — no group budget and no group cap;
   `conversion_rules` = `{rules: [{id: <main rule id from config>}]}`; no
   end date, start date left to the default (today). If the answer says
   the objective or KPI cannot be set on this account (read-only), resend
   once without `marketing_objective` and `kpi`, everything else unchanged.
4. Pass the group's id as `campaign_group_id` on every `create_campaign` of
   that group (omit the campaign dates to take the group's).
5. Write the group id into `groups:` next to its campaign ids.
No question is asked for the groups: the user's yes to the plan table
covers them. Never create a group with a budget or a daily cap, never set
AUTOMATIC distribution, never suggest a group budget or Realize's budget
allocator; budgets stay per campaign. Renaming a winners group when a new
angle joins (`creative-tests.md` §5) is the only change ever made to an
existing group: `update_campaign_group` with `name` alone.
**When a group cannot be created** (`create_campaign_group` still fails
after step 3's retry, answers 403 or any other error, or a reused group
refuses the campaign — 403 ladder step 2): build that group's campaigns
without `campaign_group_id`. Each then
lands in its own automatic Realize group ("AutoGen - <campaign name>"),
which can never be changed afterwards (the connector attaches only
campaigns that are in no group yet). Say once per build: "Realize wouldn't
let me create campaign groups on this account from here, so each campaign
sits in its own automatic group; I keep them grouped by angle on my side
either way." Record `group_build: unavailable (<error> <date>)` in config and
`[group —]` on those groups; the next build tries again once and, on the
same error, builds without groups and says the line again.

**When `create_campaign` answers 403 ("Requested action is forbidden").**
The account is reachable but one part of the request is not allowed on it.
Never resend the same call; strip the gated part and build everything else:
1. The request carried `publisher_targeting` (a Yahoo & AOL, Mail or any
   other whitelist campaign) → resend it without `publisher_targeting`,
   everything else unchanged (group, budget, targeting, `is_active` false),
   then add its ads with `create_native_item` as for any campaign. The
   campaign is **built paused, whitelist pending**: record it in config as
   `yahoo <id> WL pending AM <date>` (or `mail …`), never include it in the
   activation question below, never activate it — not now, not in a later
   run, not in Semi-auto — until the user says the list is on it. Tell the
   user once, with the id: "<campaign name> (<id>) is created and paused,
   with its ads, but without its Yahoo & AOL placement list: your account
   can't apply publisher lists yet. Ask your account manager to apply the
   Yahoo & AOL whitelist to campaign <id>; once they confirm, tell me and
   I'll activate it." When the user later says the list is applied ("my AM
   added the whitelist to <id>", any wording), activate it on that message
   (`update_campaign` is_active true) at its planned daily cap, clear the
   pending mark, and from its first full day judge it like any campaign.
2. The request carried `campaign_group_id` and still answers 403 → resend
   without it: the campaign lands in its AutoGen group; say once "Realize
   wouldn't let me add campaigns to group <name> from here — they're built
   in their own groups; I keep them grouped by angle on my side either
   way", and mark that group `[group —]` in config.
3. Still 403 → stop, report the error with the campaign name, build
   nothing further for that row, and continue with the other rows.
A 401 is reported as is (sign in again); other 4xx are fixed per the error.

Finish with: "N campaigns created, paused, with their ads. Want me to
activate them now, or after you've reviewed them in Realize?" — naming only
the campaigns that can be activated; a whitelist-pending campaign is listed
on its own line as waiting for the account manager. Activate only on an
explicit yes (`update_campaign` is_active true). Record the plan in
`digests/<account_id>/YYYY-MM-DD-launch.md`, and the batch in config
`batches:` with the build date as its launch date.

## Waves — a new country, market or product (any time after onboarding)

A wave is the plan above run again for what is new, with everything else
reused from config and the launch digest. Ask only: what is new (country /
market, or product); the landing page URLs for it; the same creatives or new
ones (new → thumbnail and title tests as above; same → one campaign per
angle with the proven ads, picked as in `growth.md` §5); platform split
(default: as the main market); then confirm in one question that the same
CPA goal applies — a different goal per market is not supported in this
version: say so plainly and keep the account goal. Show the scenario table
with its total, get the user's yes, build paused with one group per angle
of the wave, "<GEO> - <angle>" or "<product> - <angle>" ("DE - LP1
Comfort"; created as in Realize groups above, one format per group), so a
title test and a thumbnail test of the same angle pair up as at launch,
with names `<GEO> - <angle> - <platform> - <test>`, offer activation
exactly as above, record the plan in
`digests/<account_id>/YYYY-MM-DD-launch-<key>.md` and the wave as a new
batch in `batches:` (its winners get their own "Winning ads - <angles> -
<date>" group). Opening budgets: the standard anchor;
proven creatives on a new market still open at the standard number, because
the market is the unknown.
