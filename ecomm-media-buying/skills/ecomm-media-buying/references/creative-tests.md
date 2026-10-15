# Creative tests — daily ad verdicts (beta)

Beta: the verdicts are rules; what is executed follows the account's
`creative_mode` (Manual / Semi-auto — Auto is not available in the alpha).
Step 6b of every full run, after the campaign verdicts and before the digest
closes. Everything is relative to the account's goal G and its zone lines.

## 1. Which campaigns are tests

- **Built by this skill** (launch plan, weekly creative flow): the group
  name says it — "<angle> creative test", "<angle> title test",
  "<angle> thumbnail test" — and the type is known from the build.
- **Any other managed running campaign** is checked once from its active
  ads (`list_items`): two or more active ads that all share **one title and
  differ in thumbnail** → a **thumbnail test**; all share **one thumbnail and
  differ in title** → a **title test**; anything else is not a test and its
  ads are never judged. Confirm once: "12345678 looks like a thumbnail test —
  1 title × 6 thumbnails. Treat it as one?" The user can also declare it:
  "treat 12345678 as a title test". Record every test in config:
  `creative_tests: 12345678: thumbnail test (<angle>) · 23456789: title test (<angle>)`.
- A campaign with a single active ad, or a proven-creatives / winners /
  split / Mail / Display / Predictive campaign (by its group), is never a
  test.

## 2. Data per ad

For each test, `list_items` (the ads and their creation dates) and the
ad-level performance report (the dynamic report broken down by ad, or the
connector's ad breakdown report — whichever `get_dynamic_report_settings`
shows), **full days only, through yesterday**, on the main conversion rule
(campaign conversions when the report cannot split by rule, said in the
digest): spend, conversions, CPA, **since the ad went live** and L7D. A CPA
of 0 or blank is zero sales, never a winner. CTR and CPC never decide a
verdict.

## 3. Verdicts — check Losing first, then Winning, else Not determined

Each ad is judged on its own numbers since it went live. Titles are compared
only inside the title test, thumbnails only inside the thumbnail test.

| Status | Rule (any line = that status) | At G = $100 | What follows |
|---|---|---|---|
| **Losing** | 1. zero sales after spend ≥ 3×G · 2. CPA above 2×G after spend ≥ 5×G · 3. 5 or more sales at a CPA above 20/17 of G (zone D or E) · 4. *starved*: 14 or more full days live and still under 3×G spent | 1. no sales after $300 · 2. over $200 per sale after $500 · 3. 5+ sales at over $117.65 · 4. under $300 spent after two weeks | pause the ad (§4) |
| **Winning** | 5 or more sales at a CPA at or under 10/11 of G (zone A or B). *Deep winner:* 10 or more sales at or under 2/3 of G | 5+ sales at ≤ $90.91 · deep: 10+ at ≤ $66.67 | keep; its component is a winner (§5) |
| **Not determined yet** | fewer than 5 sales and no Losing line hit — or 5 or more sales at goal (CPA between 10/11 and 20/17 of G): a keeper, not a winner | under 5 sales · or 5+ sales at $90.91–117.65 | leave it running, re-read tomorrow; never paused, never copied |

Why 5 sales: below that one extra sale moves an ad's CPA by 20% or more.
The spend floors are the campaign quick-pause floors, so the advertiser
learns one set of numbers.

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
- Auto — not available in the alpha.
Default `creative_mode` = the account's mode, asked once at onboarding
("Same for pausing losing ads in your creative tests?"). A single question
about an ad is advisory and pauses nothing.

## 5. Winners

- A winning ad's **component** (its title in a title test, its thumbnail in
  a thumbnail test) is recorded in config under the test's angle:
  `winners: <angle>: titles [..] · thumbnails [..]`. It is the fixed element
  of the next test offered in the weekly creative ask (`growth.md` §1).
- **Winners campaign.** When one angle has **at least 2 winning titles and
  at least 2 winning thumbnails**, offer once, with the money:
  > "<angle> has 2 winning titles and 3 winning thumbnails. Shall I open a
  > winners campaign — those titles × those thumbnails, same platforms as the
  > tests — at $<opening> a day?"
  Opening budget: exactly as a new test campaign — the standard rule in
  `opening-budgets.md` (2.5× to 8× the goal by the goal's size: 8× under
  about $60, 5× at $100, 2.5× at $400, nearest 50). One **new campaign group
  per angle**, "<angle> winners", never shared with the tests or another
  angle; names `<angle> winners - <platform>`. Built paused as in `media-plan.md`,
  with `create_native_item` for every title × thumbnail pair; the test
  campaigns keep running. Yes → build; no → `winners: … campaign declined
  <date>`, offered again only when the set of winners grows. One side only
  (titles but no thumbnails yet, or the reverse) → nothing is offered; the
  digest says what is still missing.
- The publisher split (`growth.md` §2) and the proven ads for any later
  build (`growth.md` §5) read the same winners.

## 6. What is recorded

Config: `creative_mode`, `creative_tests:`, `winners:`. Digest, section
**Ads in creative tests**: per test, one line per ad — component, spent,
sales, cost per sale against the goal, status (Losing line n / Winning /
Not determined) — then what was ticked, paused or declined, and the
winners-campaign offer and its answer when one was made.
