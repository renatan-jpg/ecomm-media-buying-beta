# Ecomm media buying — install note (v2.0.0)

This plugin gives you a media buyer for your Realize account: every day it
reviews your running campaigns against your CPA goal, recommends raise /
hold / cut / pause with the evidence, applies what you approve (or applies
the rules by itself, in the modes you choose later), and builds your test
campaigns. One agent, `media-buyer`, runs it.

## Install (once, about five minutes)

1. Install the Claude desktop app and sign in. The daily run needs the
   desktop app; Claude Code in a terminal works for everything except the
   automatic schedule.
2. In a terminal, add the plugin source with the address and the access
   token you received, then install the plugin:
   ```
   claude plugin marketplace add https://github.com/<owner>/<repo>
   claude plugin install ecomm-media-buying@ecomm-media-buying-alpha
   ```
   If git asks for a username and password, type your name as the username
   and the access token as the password; it is remembered after the first
   time. Then inside Claude: `/plugin` → Marketplaces →
   ecomm-media-buying-alpha → Enable auto-update. New versions of the rules
   arrive by themselves after that. (Received a zip instead? In a terminal,
   run `claude --plugin-dir /path/to/ecomm-media-buying --agent media-buyer`
   for each session; a zip does not update itself.)
3. Make a folder for your account (it will hold the settings and the daily
   digests). Inside it, create a file `.claude/settings.json` containing
   `{ "agent": "ecomm-media-buying:media-buyer" }` — this makes every Claude
   session opened in that folder your media buyer. Then open the folder in
   the Claude desktop app. (In a terminal you can instead run
   `claude --agent media-buyer` in that folder.)
4. Say **"set up"**. When asked, sign in to Realize once (`/mcp`).

## What the setup asks

The Realize account ids to manage (it will never touch any other), your main
purchase event, your CPA goal, the mode — **Manual** (you tick what runs) or
**Semi-auto** (cuts and pauses run by themselves, raises wait for your tick);
Auto becomes available after four weeks — the maximum daily budget it may
ever set, the run time, and whether to build your first test campaigns now.
It then creates the daily task.

## Every day

The review runs at the time you chose, on this computer, while the Claude
app is open and the computer is awake (turn the app's keep-awake setting on).
A missed day gets one catch-up run; nothing is ever decided on one day's
numbers. In Manual mode the run waits for your ticks — Remote Control lets
you answer from your phone. When Claude asks for permission the first time,
allow it to write files in your folder (every mode) and, if you chose
Semi-auto, to use the plugin's campaign tools; otherwise an unattended run
waits on that prompt and nothing happens. Every run is written to `digests/`
in your folder, one sub-folder per account.

## Timing on a new campaign

A new campaign is paused from its first morning if it is clearly failing,
raised from its second morning only if it is exceptional (ROAS 150%+ on both
days), raised on the normal rules from its third, and paused on the normal
rules from its fifth. Cuts come later, once a campaign has a track record.

## Stopping

Say "stop" at any time. You will be asked whether to pause the daily
suggestions, change the accounts in scope, or stop entirely. Nothing is ever
changed in your account without your yes in Manual mode, and never above the
maximum you agreed.

Version 2.0.0 — the Decision Cube and the launch plan are the settled part;
the weekly creative offer, the expansion offers and the conversion-file,
new-market and goal-change processes are in beta: they only suggest.
