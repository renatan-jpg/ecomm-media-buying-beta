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
named group; a group is one objective, one KPI, one test family. Group name =
the most obvious test: `<angle or surface> <test family>` — "LP1 creative
test", "LP1 publisher split", "Predictive audiences", "Display test", "<GEO>
launch". Campaign name = `<group key> - <variable>[ - <platform>]`, the
platform only when the plan splits platforms: "LP1 - title test - Mobile",
"LP1 - thumbnail test - Desktop", "Yahoo & AOL - LP1 - Desktop", "Non-Yahoo -
LP1 - Mobile", "Predictive tier 1 - LP1", "Display - LP1". Add the product
name if the account runs several. After a build, write the group and its
campaign ids into the config `groups:` block; the review and the digest list
campaigns under these headings.

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

Realize groups: a campaign created through the connector lands in its own
automatic Realize group ("AutoGen - <campaign name>") and that assignment can
never change; the connector has no way to choose a group. Say this once per
build: "If you want these campaigns under one Realize group, create the group
in Realize first and build the campaigns from it; I'll manage them by name
either way." Never suggest a group budget or Realize's budget allocator;
budgets stay per campaign.

Finish with: "N campaigns created, paused, with their ads. Want me to
activate them now, or after you've reviewed them in Realize?" Activate only
on an explicit yes (`update_campaign` is_active true). Record the plan in
`digests/<account_id>/YYYY-MM-DD-launch.md`.

## Waves — a new country, market or product (any time after onboarding)

A wave is the plan above run again for what is new, with everything else
reused from config and the launch digest. Ask only: what is new (country /
market, or product); the landing page URLs for it; the same creatives or new
ones (new → thumbnail and title tests as above; same → one campaign per
angle with the proven ads, picked as in `growth.md` §5); platform split
(default: as the main market); then confirm in one question that the same
CPA goal applies — a different goal per market is not supported in this
version: say so plainly and keep the account goal. Show the scenario table
with its total, get the user's yes, build paused in group "<GEO> launch" or
"<product> launch" with names `<GEO> - <angle> - <platform> - <test>`, offer
activation exactly as above, record the plan in
`digests/<account_id>/YYYY-MM-DD-launch-<key>.md`. Opening budgets: the standard anchor;
proven creatives on a new market still open at the standard number, because
the market is the unknown.
