# Growth — what to offer and when (beta)

Beta: these rules suggest; nothing is built or changed without your explicit
yes. Every build goes through a plan table and `media-plan.md`.

Step 6 of a run, after execution, before the digest closes. At most one
question or offer per run. Record every ask, offer and answer in the config
`growth:` line and in the digest's Growth section. In an unattended run with
no answer, write the question to the digest as open and do not repeat it
before its next due date.

## 1. Weekly creative ask — first run of each week

On the first full run of the week (Monday, or the first run after it), unless
`creative_check: off`. Ask one single-choice question: **"Any new creatives
to test this week?"** — options: "Yes — new titles and/or thumbnails", "Yes —
a new landing page or angle", "Not this week", "Stop asking weekly".
- Not this week → `last_creative_ask: <date> — no`; ask again next week.
- Stop asking weekly → `creative_check: off` (a later "new creatives"
  message still runs the flow).
- Yes → at most three questions: which angle (one of the landing pages from
  the launch digest, or a new URL); what is new (titles, thumbnails, both) —
  when the angle already has winners (`creative-tests.md` §5), propose the
  winning component as the fixed element ("your winning thumbnail × the new
  titles");
  the titles and the thumbnails (public image URLs, or a Google Drive
  folder or file shared as "anyone with the link" — `media-plan.md`
  interview step 7, "Google Drive").
  Then the plan table and the build as in `media-plan.md`: titles only → a
  **title test** (1 thumbnail × the new titles); thumbnails only → a
  **thumbnail test** (1 title × the new thumbnails); both → both tests. One
  campaign per platform of the launch split (two when the launch split
  Mobile+Tablet and Desktop). Platform split, geo, branding, tracking code,
  conversion rule and the CPA goal are reused from config and the launch
  digest — never re-asked. Opening budget: the standard anchor through the
  2.5–8× rule. Group "<angle> creative test". Built paused; activation only on
  an explicit yes. Record `last_creative_ask: <date> — yes, <what>`.
Any day the user brings creatives, run the same flow without waiting for
Monday.

## 2. A creative test has winners → publisher split (campaign-level)

Checked every run for every managed native campaign that is not already a
publisher-split, Mail, Predictive or Display campaign (by its group). The
trigger: **L7D at least 10 conversions, L7D zone A or B, and at least one ad
with 3 or more conversions at CPA ≤ G over L7D** (ad-level report, §5).
Offer once per campaign, as one yes/no question with the money:
> "<campaign> has found winning ads (<n> ads at or under goal). Shall I put
> them on the publisher split — four campaigns, Yahoo & AOL and everything
> else, each on desktop and mobile, at $<opening> a day each?"
Yes → plan table: Yahoo & AOL (desktop), Yahoo & AOL (mobile), Non-Yahoo
(desktop), Non-Yahoo (mobile); the winning ads (§5) in each; publisher names
from `search_publishers` for "yahoo" and "aol", shown to the user, as
`publisher_targeting` INCLUDE on the Yahoo pair and EXCLUDE on the other
pair; opening budget from the whitelist anchor in `opening-budgets.md`
through the 2.5–8× rule; group "<angle> publisher split"; built paused as in
`media-plan.md`. No → `growth: split: declined <campaign id>`; never
re-offered for that campaign.
**Mail follows.** When a publisher-split campaign has run 14 days and sits in
a winning panel (L7D zone A or B), offer once: one Mail-placements campaign
per platform with the same ads, horizontal images only, no video, opening
from the In-Mail anchor. Group "<angle> mail".

## 3. The account's good week → display, then predictive (account-level)

Trigger: the managed campaigns together closed L7D at CPA ≤ G with at least
10 conversions. It fires once (`growth: good_week: <date>`). From then on the
offers come one at a time, in this order, the next only after the previous
one is built, declined or not possible yet. Declined "not now" → offer again
after 14 days, at most twice; "never" → mark it and stop.

**3a. Display.** Needs new assets. Ask for **300×250 and 300×600 first** —
they win the most placements across mobile and desktop — then optionally
320×50, 728×90, 970×250, 160×600, 250×250, 720×1280 and 1280×720. File rules
to state: JPG / PNG / GIF up to 2 MB, MP4 5–60 seconds up to 200 MB, HTML5 as
one zip up to 2 MB with index.html at the root; every file at a public https
URL. Plan: one campaign per winning angle (the angles with winning ads, §5),
opening from the display anchors (static or video) in `opening-budgets.md`,
group "Display test", names "Display - <angle>". Build: `create_campaign`
exactly as in `media-plan.md`, then one `create_display_item` per asset with
`asset_url` (never send dimensions with an asset URL). If Realize rejects the
display campaign or an item, stop, say so, and hand a build sheet (campaign
settings and the asset list) for the user to create it in Realize; the daily
routine manages it from the next run.

**3b. Predictive audience.** Eligibility: **at least 100 purchases on the
main conversion rule in the last 30 days** (the L30D account total from Step
1). Not yet → say how many they have ("64 of the 100 purchases Realize needs
in 30 days") once, then re-check silently every run. Eligible → look for a
pixel-based predictive audience with `search_lookalike_audiences`. None →
give the steps once: "In Realize: Audiences → + New Audience → Predictive
Audiences → seed: Pixel Audience → pick your purchase audience → Create. It
takes up to 48 hours to populate; tell me when it shows Ready, or I'll see it
on a later run." Re-check every run. Found → plan: one campaign per winning
angle, `lookalike_audience_targeting` with the audience's `rule_id` and
`similarity_level` 1 (tier 1), the winning ads, opening from the predictive
anchor, group "Predictive audiences", names "Predictive tier 1 - <angle>".
When a tier-1 campaign hits the ceiling, SCALE HORIZONTALLY opens the same
campaign on tier 2, then 3; never mix tiers in one campaign.

**3c. Publisher split**, if §2 has not already offered it for a winning
campaign.

## 4. Timing, in one place

| When | What is offered | Source |
|---|---|---|
| First run of each week | new creatives? → creative test | weekly ask |
| A creative test has winners | publisher split (Yahoo & AOL / Non-Yahoo × desktop / mobile) | campaign trigger |
| Split running 14 days with a winner | Mail placements | follow-up |
| The account's first good week | display, then predictive (when eligible), then the split if not yet | account trigger |

Order and timing are working rules for the alpha; the publisher split mirrors
how scaled accounts are structured, the rest is not outcome-tested yet.

## 5. Winning ads (used by every build above)

Pull the source campaign's ads (`list_items`) and their L7D and lifetime
performance (the ad-level dynamic report, or the breakdown report by ad where
that is the available tool). Candidates: ads with **3 or more conversions and
CPA ≤ G over L7D**, ranked by conversions then CPA, up to 8. Fewer than 3
candidates → add the best by lifetime CPA among ads with 2 or more
conversions. Show the list — title, thumbnail, sales, CPA — and ask for one
confirmation. Create each in the new campaign with `create_native_item` from
the source ad's title, URL, thumbnail, and description / CTA when present.
Never edit the source ads.

## 6. What is recorded

Config `growth:` line, e.g. `growth: good_week: 2026-11-03 · display: offered
2026-11-03, built 2026-11-05 · predictive: not eligible (64/100) · split:
built 2026-10-28 for 11111111`. Digest "Growth" section: the question or offer
of the day and the answer, or "open".
