# Onboarding — first run on an account (one time)

Goal: end with `config/scope.md` and one `config/<account>.md` per account
written, the daily run scheduled, the checklist said, and the launch media
plan offered — built if they accept (`media-plan.md`), skipped if they say
"skip media plan building" (existing advertisers with running campaigns
usually do). Ask the questions one at a time. Everything answered here is
never re-asked. Steps 1–7 run once per account; steps 0 and 8 once per folder.

0. **Accounts in scope.** "Which Realize account ids should I manage? I'll
   work on those and nothing else." Resolve each with `search_accounts`,
   confirm name + id, and write them to `config/scope.md` (format below). An
   account not on that list is never read or changed later; "add account
   <id>" extends the list and runs steps 1–7 for it.
1. **Account.** From step 0, per account: `account_id` (string) and name.
   Note `currency`, `time_zone_name` and `type`. The account must be
   an advertiser account (type PARTNER): a NETWORK account can list its child
   campaigns but cannot manage them. If the user gives a network, list its
   child advertiser accounts and onboard each one separately — one config per
   account.
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
   optimize for — not an aspiration. CPA goal only (ROAS goals are not
   supported in this version). Then compute the **15 budget levels** from G
   (3, 5, 7.5, 10, 12.5, 15, 20, 25, 30, 40, 50, 60, 70, 80, 100 × G, rounded
   to the nearest 50 below 1,000 and the nearest 100 above; for yen-, rupee-
   or similar-scale currencies round to the currency's natural step), show
   them to the user as amounts ("your budget levels: $300, $500, $750 …"), and
   store them in config as `levels: <15 amounts>`.
4. **Mode.** Offer two for now, then ask:
   - **Manual** — every run ends with the recommendations as tick boxes; only
     what the user ticks is executed, after a final preview. Recommended to
     start.
   - **Semi-auto** — the run applies cuts and pauses itself and reports
     them; raises still come as tick boxes for the user. The usual next step
     after a few weeks: the downside is handled daily, the upside stays in
     the user's hands.
   **Auto** (the run also executes raises, never above the agreed maximum
   daily cap) is not offered now: write `auto_unlock: <onboarded + 28 days>`
   in config; on the first run on or after that date say once "Auto mode is
   now available — say 'switch to auto' if you want it", and switch only on
   an explicit request. If the user asks for Auto earlier, explain the
   four-week wait and keep the mode as chosen.
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
   - No running campaigns, or a new product → go to `media-plan.md` now.
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
     the app's keep-awake setting on; a missed day gets one catch-up run);
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
   Then the schedule: confirm the run time (11:00 in the account's time zone,
   converted to the machine's local time) and, when the scheduler tool
   (`create_scheduled_task`) is available, create the task — id
   `ecomm-media-buying-daily`, cron `0 <converted hour> * * *` in the
   machine's local time,
   title "Ecomm media buying — daily review", prompt:
   > Open the account folder <absolute path>. Use the media-buyer agent (if it
   > is not available, follow the ecomm-media-buying skill directly). Read
   > config/scope.md; if status is not active, say so in one line and stop.
   > For every active account: run the full review, ask for approval as the
   > account's mode requires, execute what is approved or allowed, write the
   > digest, then the growth check if due. Report the digest summary.
   Without the tool (plain Claude Code), hand the same sentence for their own
   scheduler plus the command `claude --agent media-buyer` to run in this
   folder. Record `schedule:` in `scope.md`.
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
mode: manual                max_daily_cap: 10000      (mode: manual | semi-auto | auto)
run_time: daily 11:00 <account time zone>
levels: 300, 500, 750, 1000, 1250, 1500, 2000, 2500, 3000, 4000, 5000, 6000, 7000, 8000, 10000
media_plan: skipped         (or: built 2026-10-02)
onboarded: 2026-10-02
auto_unlock: 2026-10-30     (onboarded + 28 days; Auto offered from this date)
groups:                     (one line per group: <group name>: <id>, <id>)
creative_check: weekly Monday       last_creative_ask: —
growth: good_week: — · display: — · predictive: — · split: —
conversion_source: realize
cpa_goal_history: 100 (2026-10-02)
```

`config/scope.md`:

```
accounts: 1234567 — Acme (onboarded 2026-10-02); 2345678 — Acme CA (onboarded 2026-10-02)
status: active                      (active | paused since YYYY-MM-DD | stopped YYYY-MM-DD)
schedule: daily 11:00 Europe/London — desktop task ecomm-media-buying-daily
checklist: app ✓ realize-signin ✓ keep-awake ✓ auto-update ✓ remote-control suggested
realize_plugin: present | disabled | not installed
```

The review reads full days only, so the run must sit after midnight in the
account's time zone; late morning is fine. Step 8 created or handed over the
schedule. The on-demand phrase *"Review my account"* works any time.
