# Ecomm media buying — install note (v2.0.21)

This plugin gives you a media buyer for your Realize account: every day it
reviews your running campaigns against your CPA goal, recommends raise /
hold / cut / pause with the evidence, applies what you approve (or applies
the rules by itself, in the modes you choose later), and builds your test
campaigns. One agent, `media-buyer`, runs it.

## Install (once, about five minutes)

1. Install the Claude desktop app and sign in. The daily run needs the
   desktop app; Claude Code in a terminal works for everything except the
   automatic schedule.
2. Store the access token you received, once. The plugin lives in a
   private source; Claude never asks for a password, it uses what your
   computer has stored, so this step comes first. In the Terminal (Mac) or
   Git Bash (Windows) paste this line with your token in place of TOKEN:
   ```
   printf 'protocol=https\nhost=github.com\nusername=renatan-jpg\npassword=TOKEN\n' | git credential approve
   ```
   Check it: this prints one line and no error.
   ```
   git ls-remote https://github.com/renatan-jpg/ecomm-media-buying-beta
   ```
   Then add the source and install the plugin, either in the same terminal:
   ```
   claude plugin marketplace add https://github.com/renatan-jpg/ecomm-media-buying-beta
   claude plugin install ecomm-media-buying@ecomm-media-buying-beta
   ```
   or inside any Claude session, as `/plugin marketplace add` and
   `/plugin install` with the same address and name. Then inside Claude:
   `/plugin` → Marketplaces → ecomm-media-buying-beta → Enable auto-update.
   New versions of the rules arrive by themselves after that, using the
   same stored token. (Received a zip instead? In a terminal, run
   `claude --plugin-dir /path/to/ecomm-media-buying --agent media-buyer`
   for each session; a zip does not update itself.)
3. Make an empty folder for your account, for example `Acme media buying`
   in your home folder — it will hold the settings and the daily digests,
   and the media buyer writes only there. Inside it, create a file
   `.claude/settings.json` containing
   ```
   { "agent": "ecomm-media-buying:media-buyer",
     "permissions": { "allow": [ "WebFetch(domain:drive.google.com)" ] } }
   ```
   — the first line makes every Claude session opened in that folder your
   media buyer; the second lets it read a Google Drive folder you share for
   thumbnails without asking each time. Then open that folder in
   the Claude desktop app (Open folder). The first thing the setup says is
   which folder it will use; if you opened the wrong one, it tells you. (In
   a terminal you can instead run `claude --agent media-buyer` in that
   folder.)
4. Say **"set up"**. When asked, sign in to Realize once (`/mcp`).
5. Say **"check setup"**. You get seven lines: agent, rulebook version,
   Realize connection, folder, accounts in scope, daily task, mode — each
   ending in OK or what to fix. If the first line does not say the media
   buyer agent is active, or you get anything other than those seven lines,
   send the answer to the person who gave you the plugin. "check setup"
   works at any time later too.

## What the setup asks

The Realize account ids to manage — or your network id, in which case it
asks whether to run on every account under the network or only some (it
will never touch any other) — your main purchase event, your CPA goal (the
number you give is the only goal it ever uses), the mode — **Manual**: you
get the recommendations and only what you tick is applied in Realize; or
**Semi-auto**: it handles the cuts (decreasing a campaign's daily budget)
and the pauses, and you approve the budget raises by ticking them; a fully
automatic mode is not available in the beta — the maximum daily budget it
may ever set, the run time, and whether to build your first test campaigns
now. It then creates the daily task.

For test campaigns it needs your titles and landing pages in the chat, and
the thumbnails as public image links or a Google Drive folder shared as
"anyone with the link" (one sub-folder per angle works well).

## Every day

The review runs every day at the hour you chose, in your own time (10:00
in your account's time zone unless you picked another), on this computer,
while the Claude app is open and the computer is awake (turn the app's
keep-awake setting on). If you were not there to answer, it asks again
every hour for the next nine hours; if the computer was off at your hour,
the first hour it is on within those nine hours does the review. Each
day's review opens as its own session, "Ecomm media buying — daily
review", in the Claude app's sidebar; allow notifications for Claude so the
app can tell you when it asks you something. To change the time, say
"send my review at 14:00". A day with the computer off for all nine hours
is skipped and the next day reviews as usual; nothing is ever decided on one day's numbers. In Manual
mode the run waits for your ticks — Remote Control lets you answer from
your phone. Every run is written to `digests/` in your folder, one
sub-folder per account.

**Claude's own permission prompt.** The first time the media buyer writes a
file in your folder, creates the daily task, or changes a campaign, Claude
itself asks you — a box with choices like "Yes", "Yes, and don't ask again"
and "No". Pick the "don't ask again" option, once, while you are at the
keyboard: an unattended run cannot answer that box, so without it the run
waits and nothing happens, not even the digest. This prompt is Claude's
permission system, not the media buyer's mode: the Manual / Semi-auto
choice you make at setup decides whether you approve budget changes before
they happen, and you never need to switch Claude to its own "Auto"
permission setting.

## Timing on a new campaign

A new campaign is paused from its first morning if it is clearly failing,
raised from its second morning only if it is exceptional (at or under
two-thirds of your goal per sale since launch, and no day over
ten-elevenths of it), raised on the normal rules from its third, and paused
on the normal
rules from its fifth. Cuts come later, once a campaign has a track record.

## If you see more sales than Realize does

Realize only reports the purchases its own tracking saw. If your numbers
differ, send the media buyer your file and it will reconcile the two — this
is a feature of the media buyer, not of Realize. Two kinds of file work:

- **An attributed file**: your orders already matched to Realize — one row
  per order with the attribution date, the Realize campaign id and ad id
  (`item_id`) and, best of all, the Realize click id (for example `date,
  campaign_id, campaign_name, item_id, item_name, order_id, purchases,
  purchase_value_usd, click_id`). This counts the way Realize counts, so you can say
  **"for 15 October go by my file, the rest
  by Realize"**: that day's count is replaced by yours in every reading
  (yesterday, last 3 days, last 7 days, since launch), the other days stay
  Realize's, and every card touching that day says so ("15 Oct from your
  file: 5 sales, Realize 11"). A campaign with no row in the file that day
  counts as 0 from your file. You can also make it the whole source.
- **A purchase file**: one row per order with its date and time (or counts
  per campaign per day), the campaign id or `utm_campaign` name on every
  row, and the time zone of the times. It counts on the day of purchase,
  which is not how Realize counts, so it can only replace Realize entirely
  ("use my file from now on"), never one day inside a week.

Either file covers every campaign it manages, through yesterday, in one
CSV or XLSX. A file with only dates and totals cannot be matched to
campaigns, so you will be asked to add the campaign to your tracking link
first. In "use my file from now on", a fresh file through yesterday is
needed before each run; a missing day means that day's review waits.

## Creative tests

A title test (one thumbnail, several titles) or a thumbnail test (one
title, several thumbnails) is read every day at the ad level: an ad is
losing when it has no sales after 3× your goal spent, costs more than
twice your goal per sale after 5× spent, has 5 or more sales well over
goal, or sat two weeks without spending; it is winning with 5 or more sales
at or under goal. Losing ads are paused (ticked by you, or by the run in
Semi-auto, your choice at setup); winning titles and thumbnails are kept.
Once the title test and the thumbnail test of one angle (one campaign
group) have two winning titles and two winning thumbnails between them,
you are offered the two campaigns the test was run for, together: "<angle>
- winning ads", run of network, every winning title × every winning
thumbnail, at the budget a new test would open with (2.5–8× your goal by
its size — $500 at a $100 goal); and "<angle> - Yahoo & AOL", the same ads
on Yahoo and AOL placements only, at $750 at a $100 goal. One pair for each
platform the tests ran on, on the tests' landing page, in the same campaign
group as the tests — one group = one creative angle, holding its title
test, thumbnail test and these two; if your account was set up with a
group per test, the media buyer offers to merge them once, and you can
also say "these two are one creative angle" any time. The offer names only
the winners, and it is one yes for the pair, not a pick list. You can
answer with other budgets; nothing is built without your yes, and the new
campaigns are then reviewed daily like any other. If your account cannot
carry publisher lists yet, both campaigns are still built: the Yahoo & AOL
one is created paused, with its ads but without its placement list, and
you are asked to have your account manager apply the Yahoo & AOL whitelist
to it; tell the media buyer once that is done and it is activated. Tests you
already run can be included by saying "treat <campaign id> as a title
test".

## Stopping

Say "stop" at any time. You will be asked whether to pause the daily
suggestions, change the accounts in scope, or stop entirely. Nothing is ever
changed in your account without your yes in Manual mode, and never above the
maximum you agreed.

Every verdict is explained as cost per sale against your goal; the ROAS
column you see in Realize is revenue-based and is not what the rules read.

Version 2.0.21 — the Decision Cube and the launch plan are the settled part;
the weekly creative offer, the expansion offers and the conversion-file,
new-market and goal-change processes are still experimental: they only suggest.
