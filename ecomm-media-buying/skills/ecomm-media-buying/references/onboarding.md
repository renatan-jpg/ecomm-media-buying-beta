# Onboarding — first run on an account (one time)

Goal: end with `config/scope.md` and one `config/<account>.md` per account
written, the daily run scheduled, the checklist said, and the launch media
plan offered — built if they accept (`media-plan.md`), skipped if they say
"skip media plan building" (existing advertisers with running campaigns
usually do). Ask the questions one at a time. Everything answered here is
never re-asked. Steps 1–7 run once per account; steps 0 and 8 once per folder.

First line, before any question: "I'll keep your settings and daily digests
in <absolute path of this folder>." If that folder is the plugin itself
or the folder it was unpacked into (it holds `.claude-plugin/plugin.json`,
`agents/media-buyer.md` or `skills/ecomm-media-buying/SKILL.md`, or a
sub-folder `ecomm-media-buying/` holding any of them), stop there: "This is the plugin's own
folder — make a folder for your account, open it in the Claude app and say
'set up' there." Nothing is written in the plugin folder.

0. **Accounts in scope.** "Which Realize account ids should I manage — or
   your network id, if you want every account under it? I'll work on those
   and nothing else." Resolve each id with `search_accounts` and confirm
   name + id.
   - **Advertiser account ids** → write them to `config/scope.md` (format
     below). An account not on that list is never read or changed later;
     "add account <id>" extends the list and runs steps 1–7 for it.
   - **A network id** (`type` NETWORK) → list its advertiser accounts with
     their running campaign counts (`list_campaigns` on the network id,
     grouped by `advertiser_id`; or `search_accounts`), then ask one
     single-choice question: "I'm seeing network <name> (<id>) with <N> live
     campaigns across <M> accounts: <account — n campaigns, …>. Shall I run
     on all live campaigns in this network, or only on certain accounts?" —
     options "All accounts in the network" / "Only some — I'll name them".
     All → `network: <id> — <name> (all advertiser accounts)` in
     `scope.md`; the account list is refreshed from the network on every run,
     and an account that appears later is offered steps 1–2 once before it
     is managed. Some → the named accounts, as above. Campaigns are always
     read, judged and changed through their own advertiser account id; the
     network id is only used to list.
1. **Account.** From step 0, per account: `account_id` (string) and name.
   Note `currency`, `time_zone_name` and `type` (PARTNER = advertiser
   account; every managed account is one). With a network in scope, steps
   1–2 run per account and steps 3–5 are asked **once** for the network and
   written into every account's config — "one goal, one mode and one maximum
   for the whole network, or different per account?" decides; an account
   that needs its own values gets them in its own config. Record the answer
   in `scope.md` as `network_defaults: goal <G> · mode <mode> · max <cap>`
   or `network_defaults: per account`; an account that appears under the
   network later takes the defaults, or is asked steps 3–5.
2. **Main conversion event.** `get_conversion_rules` on the account. One
   purchase rule → confirm it. Several → list them and ask which is the main
   purchase event; all CPA math runs on that rule. No purchase rule → stop:
   the account can't be managed against a CPA goal until purchase tracking
   exists; point the user to set it up in Realize.
   **Say this plainly:** "Every raise, cut and pause will be decided from the
   purchases this rule reports, so it has to be accurate — a pixel that
   under-reports will get good campaigns paused, one that over-reports will
   get bad campaigns scaled." Then do a quick sanity check together: pull the
   last 7 days of conversions on that rule for the account and ask the user
   whether the count is in line with the orders they attribute to this
   channel. A clear mismatch → fix tracking first; the routine starts after.
3. **CPA goal (G).** The number the user manages to, in account currency.
   The rules are all relative to G, so ask them to give the number they truly
   optimize for — not an aspiration. CPA goal only (a revenue-based ROAS
   goal is not supported in this version). The goal is the advertiser's
   number: never propose one from the account's results, never start with an
   example. Then compute the **four zone lines** (the exact fractions G ×
   2/3, 10/11, 20/17, 4/3 — at $100: 66.6667, 90.9091, 117.6471, 133.3333),
   store them to four decimals as `zone_lines:` and say them in plain words,
   rounded: "about $67 per sale or less is a deep winner, up to about $91 a
   winner, up to about $118 at goal, up to about $133 over goal, above that
   a blowout — every verdict reads your campaigns against these lines."
   Then compute the **15 budget levels** from G
   (3, 5, 7.5, 10, 12.5, 15, 20, 25, 30, 40, 50, 60, 70, 80, 100 × G — for a
   goal under $50 or the equivalent, drop the 3× level so the ladder starts
   at 5×G: 14 levels, e.g. $100, $150, $200 … $2,000 at a $20 goal — rounded
   to the nearest 50 below 1,000 and the nearest 100 above; for yen-, rupee-
   or similar-scale currencies round to the currency's natural step), show
   them to the user as amounts ("your budget levels: $300, $500, $750 …"), and
   store them in config as `levels: <15 amounts>`.
4. **Mode.** Ask one single-choice question, in exactly these words:
   - **Manual (recommended to start)** — "You'll get our recommendations;
     only what you tick is applied in Realize."
   - **Semi-auto** — "We handle the cuts (decreasing a campaign's daily
     budget) and the pauses; you approve the campaign budget raises by
     ticking them."
   (Manual: every run ends with the recommendations as tick boxes and a
   final preview. Semi-auto: the run applies cuts and pauses itself and
   reports them; raises still come as tick boxes.) Then one yes/no: "Same
   for pausing losing ads in your creative tests?" — yes → `creative_mode`
   = the mode; no → ask which of the two, and write it.
   **Auto** (the run would also execute raises) is **not available in the
   alpha**: it is not offered, not mentioned unless the user asks, and never
   switched on. If the user asks for it: "Auto isn't available in the alpha
   — Manual or Semi-auto for now." Write `auto: not available (alpha)` in
   config.
   Say also: these modes decide whether you approve first or the run applies
   the rules by itself; they are not Claude's own permission settings.
5. **Maximum daily cap.** The highest daily budget the skill may ever set on a
   single campaign (default: the top level, 100×G). Frame it as: "we scale
   gradually, only profitable campaigns, one level at a time — this is just
   the ceiling. Once a campaign reaches it, I'll suggest opening a new
   campaign instead of raising further."
6. **Scope.** The routine manages only purchase-objective campaigns attached
   to the main conversion rule; other campaigns are listed as out of scope
   each run. Say so.
7. **Launch plan — offer it, with a clear skip.** Say: "Want me to build
   your first test campaigns now, or skip media plan building and go straight
   to the daily raise / hold / cut / pause review of what's already running?"
   - No running campaigns, or a new product → go to `media-plan.md` now
     (with a network in scope, first ask which advertiser account the plan
     is for).
   - Running campaigns and the user says **"skip media plan building"** (or
     declines in any words) → write `media_plan: skipped` in config, say
     "Got it — I'll manage the campaigns you already have, starting with the
     next review. Ask me for a launch plan any time." and finish onboarding.
     Offer the first review right away: "Want me to run today's review now?"
   The phrase "skip media plan building" works at any point of onboarding,
   not only at this step; it never skips the CPA goal, the conversion-event
   check or the mode question.

8. **Checklist and the daily schedule (once per folder).** Say the checklist
   plainly and record it in `scope.md`:
   - the Claude desktop app installed and signed in (the daily run happens on
     this machine, while the app is open and the computer is awake — turn
     the app's keep-awake setting on; the run tries every hour from 10:00
     to 19:00 account time until the review has run and been answered, and
     a day with the computer off for that whole window is skipped);
   - Realize signed in through this plugin's connection (`/mcp`), once;
   - auto-update switched on for the plugin's marketplace (plugin menu →
     Marketplaces), so rule updates arrive by themselves;
   - Manual mode: Remote Control lets you tick the day's boxes from your
     phone when the run finishes while you are away;
   - if the Realize plugin is also installed (its tools show up under the
     prefix `mcp__plugin_realize-plugin_realize-mcp__`), say: "You can keep
     it, or disable it under /plugin to avoid a second Realize sign-in; this
     plugin brings its own connection." Never try to disable anything;
   - for Semi-auto and Auto: allow this plugin's campaign tools once in
     Claude's permissions, and in every mode allow file writes inside this
     folder, so an unattended run never waits on a prompt (a prompt nobody
     answers means the cut or pause, and the digest, simply do not happen).
     Say it this way: "The first time I write a file here or change a
     campaign, Claude itself will ask you — a box with choices like 'Yes',
     'Yes, and don't ask again' and 'No'. Pick the 'don't ask again' one.
     That is Claude's own permission prompt; it has nothing to do with the
     Manual / Semi-auto mode you chose, and you never need to switch Claude
     to its own 'Auto' permission mode."
   Then the schedule: confirm the first run time (10:00 in the account's
   time zone, converted to the machine's local time; "10:00 your account's
   time, or another hour?") and say that it retries every hour until 19:00
   if nobody answered. When the scheduler tool (`create_scheduled_task`) is
   available, create the task — id `ecomm-media-buying-daily`, cron
   `0 <converted first hour>-<converted last hour> * * *` in the machine's
   local time (10:00–19:00 account time; split into two tasks only if the
   conversion crosses midnight), title "Ecomm media buying — daily review",
   prompt:
   > Open the account folder <absolute path> as the media-buyer agent. Read
   > config/scope.md; if status is not active, say so in one line and stop.
   > Decide from today's digest: no digest → run the full review for every
   > active account, ask for approval as the account's mode requires, execute
   > what is approved or allowed, write the digest, then the growth check if
   > due; approval pending → show the pending items again and finish them;
   > done → stop silently. Outside 10:00–19:00 account time → one line, stop.
   **Then verify:** call `list_scheduled_tasks` and find
   `ecomm-media-buying-daily` in the list. Only then write
   `schedule: daily 10:00–19:00 hourly <tz> — desktop task ecomm-media-buying-daily`. If
   the scheduler tool is missing, the create call fails, or the task is not
   in the list, write `schedule: NOT CREATED — <reason>` instead, say so in
   one line, and hand the same sentence for their own scheduler plus the
   command `claude --agent media-buyer` to run in this folder. The task line
   is never written on an assumption. Record `folder: <absolute path>` in
   `scope.md`.
   **Connection check:** the run must use this plugin's own Realize
   connection (its tools carry the prefix
   `mcp__plugin_ecomm-media-buying_realize-mcp__`). If those tools are not
   available, stop and say "sign in to Realize through this plugin: /mcp →
   realize-mcp → authenticate"; never fall back to another Realize
   connection, and never write a digest from one.
   **Cloud runs (an option the skill never sets up; to be tested):** for
   Semi-auto or Auto, Anthropic's scheduled cloud sessions can run the
   routine while the machine is off, if the Realize connector is added as a
   claude.ai connector, the account folder lives in a git repository, and
   the user has a paid claude.ai plan. Mention it only when the user asks
   about weekends or an always-on run.

Write `config/<account>.md`:

```
account_id: <string>        account_name: <name>
currency: USD               time_zone: <tz>
cpa_goal: 100               main_conversion_rule: <id> — <name>
zone_lines: 66.6667, 90.9091, 117.6471, 133.3333   (G × 2/3, 10/11, 20/17, 4/3, four decimals)
mode: manual                max_daily_cap: 10000      (mode: manual | semi-auto | auto)
run_time: daily from 10:00 <account time zone>, hourly retries until 19:00
levels: 300, 500, 750, 1000, 1250, 1500, 2000, 2500, 3000, 4000, 5000, 6000, 7000, 8000, 10000
media_plan: skipped         (or: built 2026-10-02)
onboarded: 2026-10-02
auto: not available (alpha)
groups:                     (one line per group: <group name>: <id>, <id>)
creative_check: weekly Monday       last_creative_ask: —
creative_mode: manual               (manual | semi-auto — how losing ads in creative tests are paused)
creative_tests: —                   (12345678: thumbnail test (<angle>) · 23456789: title test (<angle>))
winners: —                          (<angle>: titles [..] · thumbnails [..] · campaigns <id>, <id> built / declined / offer open / yahoo pending permission <date>)
growth: good_week: — · display: — · predictive: — · split: —
conversion_source: realize    conversion_file_tz: —   (the time zone of the advertiser's purchase file, asked with the first file)
conversion_overrides: —       (per day: <date>: file <name> · <campaign id>: <n> … — client-requests.md §1 option 3)
cpa_goal_history: 100 (2026-10-02)
```

`config/scope.md`:

```
accounts: 1234567 — Acme (onboarded 2026-10-02); 2345678 — Acme CA (onboarded 2026-10-02)
network: 7654321 — Acme Network (all advertiser accounts, refreshed each run)   (this line only when a network is in scope)
network_defaults: goal 100 · mode manual · max 10000   (or: per account; only with a network)
folder: /Users/<name>/Acme media buying
status: active                      (active | paused since YYYY-MM-DD | stopped YYYY-MM-DD)
schedule: daily 10:00–19:00 hourly Europe/London — desktop task ecomm-media-buying-daily
checklist: app ✓ realize-signin ✓ keep-awake ✓ auto-update ✓ remote-control suggested
realize_plugin: present | disabled | not installed
```

The review reads full days only, so the run must sit after midnight in the
account's time zone; 10:00 is the default. The task fires hourly until
19:00: the first firing that finds the machine on does the review, later
firings only re-ask what is still pending, and a day with no firing in the
window is skipped (SKILL.md, Run types). Step 8 created or handed over the
schedule. The on-demand phrase *"Review my account"* works any
time.
