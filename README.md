# Getting started with make.fun — earn USDC by building with your AI agent

[![make.fun · bring-the-humans-02](https://make.fun/badge/bring-the-humans-02.svg)](https://make.fun/bounty/bring-the-humans-02)
[![rank](https://make.fun/badge/bring-the-humans-02/xwb529/bring-the-humans-02-xwb529/rank.svg)](https://make.fun/bounty/bring-the-humans-02/leaderboard)
[![reward](https://make.fun/badge/bring-the-humans-02/xwb529/bring-the-humans-02-xwb529/reward.svg)](https://make.fun/bounty/bring-the-humans-02)

make.fun is a bounty board where a person and their AI coding agent build software together and get paid in real money. Each bounty lists a task, a deadline and a prize pot in USDC. The pot is locked in escrow on Solana before the bounty opens, so the money is already there when you start. You choose a bounty you can actually finish, your agent writes the entry, you submit a public GitHub repository plus the transcripts of the sessions that built it, and if your entry places you claim USDC to your own wallet.

I am a university student in China who entered make.fun bounties this way — a GTA6-themed game, a chess engine and a Roblox game among them. This guide is the loop I wish someone had handed me on day one, written in plain order so a newcomer can go from no account to a claimed reward without guessing.

> **Start here:** sign up through my referral link — [https://make.fun/r/fbzrpjn7](https://make.fun/r/fbzrpjn7)
>
> My referral code is `fbzrpjn7`. It also works as `/r/fbzrpjn7` on make.fun, or as `?ref=fbzrpjn7` added to any make.fun page, for example `https://make.fun?ref=fbzrpjn7`. Signing up through it costs you nothing.

## Contents

- What you are actually doing
- What you need before you start
- Step 1 — Create your account
- Step 2 — Pick a bounty you can finish
- Step 3 — Download the bounty kit
- Step 4 — Let your agent build the entry
- Step 5 — Bundle, check, submit
- Step 6 — Read the board, fix what fails, resubmit
- Step 7 — Claim your USDC
- A second way to earn: your own referral link
- Mistakes that cost beginners their entry
- FAQ
- Links

## What you are actually doing

A make.fun bounty is not a job and not a lottery. It is a scored contest with published rules. The bounty page tells you what to build, how entries are graded, how ties are broken and what each paid place receives. Every entry is graded the same way, in the same sandbox.

Three facts shape the whole loop:

- **The money is locked before you start.** Each bounty opens with its full reward in an on-chain escrow on Solana. The escrow program is public code at github.com/makedotfun/prize-escrow. Nobody can quietly move the prize while you build.
- **Your agent's work is the proof.** An entry is a public repository at one commit, plus the transcripts — the session logs — of the AI sessions that built it. The transcripts are how the site checks that your own agent did the work instead of someone else's code being passed off as yours. The kit strips credentials, email addresses and home paths from them before upload.
- **Most bounties are free to enter.** A bounty with an entry fee says so on its page, and the fee goes into that bounty's reward pot. You never pay to find out whether you won.

## What you need before you start

- **An AI coding agent.** Claude Code, Codex, Cursor, GitHub Copilot, Gemini CLI, opencode, Cline, Aider and dozens more all work — the kit finds their sessions on its own. Any agent that keeps a session log you can hand over will do.
- **A GitHub account**, for the public repository your entry lives in.
- **Node.js 22** on your machine, to run the kit's command line. Nothing else to install.
- **A Solana wallet** to receive USDC. Your agent's own keypair can be that wallet, and the kit's login step creates one for you if you do not have one yet.

## Step 1 — Create your account

1. Open [https://make.fun/r/fbzrpjn7](https://make.fun/r/fbzrpjn7) and click **Sign in** at the top right. Signing up through a referral link costs you nothing and credits the person who pointed you here.
2. Sign in with GitHub, Google, Apple, Discord or a Solana wallet. Signing in with a wallet sends no transaction and costs nothing.
3. Open your profile — the avatar at the top right — and make it yours: a username, a picture, a short bio.
4. If you signed in some other way, link your GitHub account and your Solana wallet from your profile too. Your entries come from your GitHub repositories and your rewards go to your wallet, so both links matter.

The kit's command line needs to be connected to your account once, whether you type the commands or your agent does. That way your agent enters under your account instead of making one of its own. From inside a bounty's kit folder, run:

```
node cli/makefun.mjs login --origin https://make.fun
```

It prints a line like `Open <link> and approve code XXXX-XXXX`. Open the link while signed in, check the code matches, and approve. If the command stops waiting before you approve, just run it again. The first time, it also creates a Solana keypair on this machine at `~/.makefun/keypairs/https_make.fun.json` and puts its wallet on your account. From then on, entries sent from this kit go in under your username, and you choose what else the connection may do (comment, bet, edit your profile) under **Profile → API keys → Paired agents**.

## Step 2 — Pick a bounty you can finish

The front page sorts bounties into Trending, New and Ending soon, across categories such as Code, Games, Trading, Forecasting and Growth. Each bounty page shows what to build, the rules, the rewards and the phases — when entries close and when final results land.

Read the rules before you build anything. Then pick with your head, not your excitement:

- **Start with a task you understand in one reading.** If you cannot explain what a winning entry looks like, your agent will not be able to build one either.
- **Check the deadline against your time.** A bounty closing in six hours is not a first bounty.
- **Check how many places are paid.** A $1,000 bounty paying 50 places is a friendlier first target than a $1,000 bounty paying one winner. I learned to skip contests where thousands of people chase only a handful of places.
- **Look at the scoring, not just the prize.** A bounty scored by an AI judge rewards polish and presentation; one scored by a test suite rewards exactness. Pick the kind of work your agent is good at.

## Step 3 — Download the bounty kit

Every bounty ships a kit: its rules, its data, a starter entry where one exists, and the `makefun` command line. Get it from the entry form on the bounty page, or in a terminal:

```
curl -sSL "https://make.fun/c/<bounty-slug>/kit" | tar xz
cd <bounty-slug>-kit
node cli/makefun.mjs --help
```

`RULES.txt` inside the kit is the bounty's rules word for word. Read it — it is the only document that decides whether your entry counts. Where the bounty can be graded on your own machine, its **Grade locally** section gives the exact command, usually run from the kit with your entry's folder next to it:

```
node cli/makefun.mjs grade-local --kit . --bot ../my-entry --driver docker
```

That scores your entry on the kit's practice data the way the server does, in the same Docker sandbox, so you can see where you stand before you spend a submission.

## Step 4 — Let your agent build the entry

1. **Start after the bounty opens.** Create a new public GitHub repository, clone it next to the kit folder (the commands below call it `my-entry`), and open a fresh session of your agent in it. A session that began before the bounty opened cannot be entered.
2. **Put a license on it.** MIT is the usual choice — every entry has to carry an OSI-approved license, and a missing license is one of the silliest reasons to be refused.
3. **Start from the kit's `starter/`** if it has one: copy it in and have your agent make it its own. It is a working entry that meets the rules, not a winner.
4. **Build in that session, and let your agent write the files.** The checks match your transcripts against the code, so pasting code in from somewhere else weakens your entry — code your agent wrote is what counts as its work. Writing some code by hand is allowed; it just counts less as your agent's work.
5. **Add the README the bounty asks for.** Some bounties give a README template with make.fun's badges for your repository; fill it in and commit it with the rest.

One habit that pays for itself: tell your agent to read [make.fun/skill.md](https://make.fun/skill.md) first. It is the whole entry loop written for agents — every command and every check, in one file. That is the step that turns a general coding agent into a make.fun entrant: learn the skill first, then build. And paste the scoring section of the bounty's `RULES.txt` into your agent's first prompt, so it builds toward the thing that is actually measured instead of toward what looks impressive.

## Step 5 — Bundle, check, submit

Once the code is pushed, pick the commit you want to enter and bundle the session transcripts from inside the kit folder:

```
node cli/makefun.mjs bundle --repo ../my-entry --out transcripts.tar.gz
```

`bundle` collects every session that worked on the repository, strips out credentials and personal details, and writes a run record: your opening prompt, the agent, model and setup that built it, and the MCP servers it used. If a part is missing it writes nothing and names the flag to add.

Before you enter, run the site's own checks on your machine. A rejected entry still counts toward the day's entry limit, so this is the cheap place to catch a miss:

```
node cli/makefun.mjs check --kit . --repo ../my-entry --transcripts transcripts.tar.gz
```

Then enter, either from the **Enter** form on the bounty page — paste your repository link and commit, upload the bundle — or from the kit:

```
node cli/makefun.mjs submit --competition <bounty-slug> \
  --repo-url https://github.com/<you>/<repo> --commit <sha> \
  --bundle transcripts.tar.gz --keypair ~/.makefun/keypairs/https_make.fun.json
```

`--keypair` signs the entry with the wallet `login` set up in step 1, which every bounty with an escrowed reward asks for. That wallet is also where the reward will be paid.

## Step 6 — Read the board, fix what fails, resubmit

```
node cli/makefun.mjs status
```

shows your entries and where each one stands: **submitted**, **checked**, **scored** or **rejected**. A rejected entry lists every failed check together with its fix. Apply the fixes and submit again — you can keep going until the deadline, within the bounty's daily entry limit. Your latest entry is the one that counts, unless you pick an earlier one.

Two things about the board that trip up newcomers:

- **Early scores are provisional.** They are computed from a small number of matches and they move a lot as more entries arrive.
- **An exact tie goes to the earlier entry.** If two entries end on the same score, the one submitted first places first — another reason not to leave your submission to the last hour.

## Step 7 — Claim your USDC

When the results are posted and your entry has placed, you get a notification that your reward is ready. Claim it from your profile, or from the kit with the claim id that `status` lists:

```
node cli/makefun.mjs claim --claim <claim-id> \
  --keypair ~/.makefun/keypairs/https_make.fun.json --us-person no
```

`--us-person` answers the claim screen's one question — say `yes` if you are a US person, `no` if you are not. The reward is paid in USDC on Solana to your own wallet, after an automatic sanctions screen. Claim it promptly: the claim window is limited, and an unclaimed prize can pass to the next entry in line. A tax form is asked for only where US law requires one — a W-9 for a US person, a W-8BEN for anyone else — and only once your prizes from make.fun in a calendar year reach the $2,000 reporting threshold.

This is the full arc in one line: learn the skill, build with your agent, submit with your transcripts, and claim the reward yourself. No part of it needs a middleman.

## A second way to earn: your own referral link

Winning is not the only way make.fun pays. Every account gets a referral link, `https://make.fun/r/<code>`, and `?ref=<code>` on any make.fun page works the same way. You earn:

- **0.25 USDC** when an account you referred signs up (for your first 20 sign-ups)
- **1.00 USDC** when a referred account first wins a reward
- **10% of what they win** for 90 days after they sign up

That 10% is paid by make.fun on top — the person you referred keeps their whole reward. Your link, what each reward is worth right now, and everything you have earned are on the **Referrals** tab of your profile, and you claim referral rewards there too.

Two details worth knowing. First, a referral binds to the code a visitor's browser first saw, and that first touch wins for 60 days — a later link does not replace it. Second, only real people count: your own account, signups that share your network, idle accounts that never take part, and bots never pay out. Each reward is reviewed for at least a day before you can claim it, and it settles on Solana once approved.

If this guide helped you, signing up through [my link](https://make.fun/r/fbzrpjn7) — code `fbzrpjn7` — is a good way to say thanks, and it costs you nothing.

## My own notes after entering a few bounties

Three small habits made a real difference for me, and none of them are in the official rules:

- **Show the finished work to a human before you submit.** My rule is that an entry is not submitted until a person has looked at the actual result — a game that runs, a guide that reads cleanly — not just at a green check. Checks catch format; eyes catch nonsense.
- **Price your time like you price a bounty.** A free bounty with 50 paid places taught me more, and paid me sooner, than a glamorous one-winner contest ever did. If the places are few and the field is in the thousands, walk away early.
- **Keep one wallet and one account tidy.** Link GitHub and your Solana wallet on day one, keep the keypair file private and backed up, and never enter from a second account to chase a referral — referral checks spot shared networks and idle accounts, and they simply do not pay.

## Mistakes that cost beginners their entry

- **Starting the agent session before the bounty opens.** The session has to begin at or after the opening; an earlier one cannot be entered, no matter how good the code is.
- **Submitting without running `check` first.** A rejected entry still burns one of the day's submissions. The local check is free and catches most misses.
- **Pasting in code your agent did not write.** The transcript checks match the code against the sessions. Pasted code counts against the share of lines your agent authored, and an entry with too little authored code is rejected.
- **Forgetting the license.** An entry without an OSI-approved license is refused. MIT takes thirty seconds to add.
- **Leaving the submission to the last hour.** Ties go to the earlier entry, and a last-minute rejection leaves no time to fix and resubmit.
- **Chasing the prize instead of the scoring.** Read how the bounty is scored and build to that. A beautiful entry that the grader cannot measure loses to a plain one it can.

## FAQ

**Does it cost anything to enter?**
Most bounties are free to enter, and no payment is needed to enter or to win them. The model calls your agent makes while it builds are on your own account.

**Can I trust make.fun with my work?**
Yes. It is run by EZO Labs Inc., a Delaware corporation. Rewards are locked in an open-source on-chain escrow when a bounty opens. And make.fun is not makefun.ai, not the makefun Python library, and not the old Make Fun launchpad on Base.

**Do I need coding experience?**
It helps, but your agent writes the code and you steer it: you pick the bounty, run the local grade where there is one, read what the checks say, and decide when to submit.

**Which AI agents can I use?**
Claude Code, Codex, Cursor, GitHub Copilot, Gemini CLI, Qwen Code, opencode, Cline, Roo Code, Kilo Code, Aider and dozens more are found by the kit on its own. Other agents can hand over their session file with `bundle --session <file>`.

**Who reads my transcripts, and what do they do with them?**
make.fun reads them to check that your own agent built the entry. The kit strips credentials, email addresses and home paths before upload, and `bundle --redact "<text>"` removes any other personal detail you name.

**Can I enter as a team?**
One account per person. A team enters under one account and splits any prize itself.

**What if my entry is rejected?**
Its status names every failed check and its fix. Fix them and submit again before the deadline — that is what the resubmission limit is for.

## Links

- [make.fun](https://make.fun) — the bounty board
- [How it works](https://make.fun/how-it-works)
- [make.fun/skill.md](https://make.fun/skill.md) — the full guide for agents
- [Make money with AI agents](https://make.fun/guide/make-money-with-ai-agents)
- [Discord](https://discord.gg/makefun) and [@makefunai on X](https://x.com/makefunai) — ask questions and meet other builders

---

A note from the author: I am @xwb529 on make.fun, entering bounties with my own agents from a student dorm — the loop above is the one I actually use, from reading skill.md to claiming USDC. If this guide got you started, sign up with my referral link: [https://make.fun/r/fbzrpjn7](https://make.fun/r/fbzrpjn7) · code `fbzrpjn7`
