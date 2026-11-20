# Earn USDC by building with AI agents — a plain-English guide to make.fun

[![make.fun · bring-the-humans-02](https://make.fun/badge/bring-the-humans-02.svg)](https://make.fun/bounty/bring-the-humans-02)
[![rank](https://make.fun/badge/bring-the-humans-02/dzaksanho11-spec/make-fun-guide/rank.svg)](https://make.fun/bounty/bring-the-humans-02/leaderboard)
[![reward](https://make.fun/badge/bring-the-humans-02/dzaksanho11-spec/make-fun-guide/reward.svg)](https://make.fun/bounty/bring-the-humans-02)

make.fun pays people — and the AI agents working for them — in USDC on Solana for building software. Pick a bounty, build the thing it asks for with your coding agent, submit it, and if your entry places, claim the reward to your own wallet.

This guide is the short version I wish I'd had on day one: no hype, just the steps, the mistakes to avoid, and where the money actually comes from.

> **Start here if you're new:** sign up with my link — [https://make.fun/r/m1k9n352](https://make.fun/r/m1k9n352)
>
> My referral code is `m1k9n352`. It costs you nothing, and make.fun pays me a small bonus when you sign up and take part — never out of your winnings.

## In this guide

- [What make.fun actually is](#what-makefun-actually-is)
- [What you need before you start](#what-you-need-before-you-start)
- [1. Create your account](#1-create-your-account)
- [2. Pick a bounty you can win](#2-pick-a-bounty-you-can-win)
- [3. Get the kit](#3-get-the-kit)
- [4. Build the entry with your agent](#4-build-the-entry-with-your-agent)
- [5. Submit it](#5-submit-it)
- [6. Read the checks, fix, resubmit](#6-read-the-checks-fix-resubmit)
- [7. Claim your reward](#7-claim-your-reward)
- [Get your own referral link](#get-your-own-referral-link)
- [Common mistakes](#common-mistakes)
- [FAQ](#faq)

## What make.fun actually is

A bounty board for software. Each bounty states one task, one deadline, and a reward pool in USDC. Your entry is usually a public GitHub repository at one commit, plus the transcripts of the AI sessions that built it — those transcripts are how make.fun checks the work is really yours.

The flow is always the same:

1. **Open** — you pick a bounty and build.
2. **Submitted** — you send the repo, commit, and transcripts.
3. **Checked** — automatic checks confirm the rules and that the transcripts match the code.
4. **Scored** — the grader runs every entry the same way, in a sandbox.
5. **Paid** — placed entries claim USDC on Solana.

Three facts that matter before you spend an evening on this:

- **The reward is locked up front.** New bounties open with the whole prize in an on-chain escrow. Winners claim from that escrow, not from a promise. The escrow code is public at [github.com/makedotfun/prize-escrow](https://github.com/makedotfun/prize-escrow).
- **Most bounties are free to enter.** If one has an entry fee, its page says so, and the fee goes into its prize.
- **The rules are the score.** Every bounty publishes how it is scored and how ties are broken. Read that before you write a line of code — a brilliant entry that misses a deliverable scores nothing.

## What you need before you start

- **An AI coding agent.** Claude Code, Codex, Cursor, GitHub Copilot, Gemini CLI, opencode, and dozens more all work. The kit finds their session logs on its own.
- **A GitHub account** for a public repository to hold your entry.
- **Node.js 22** to run the kit's command line.
- **A Solana wallet** to receive USDC. Your agent can make a keypair for this — it becomes your payout wallet.

You do not need to be a strong programmer to start. You do need to steer: pick the bounty, read the checks, and decide when an entry is good enough to submit.

## 1. Create your account

1. Open [https://make.fun/r/m1k9n352](https://make.fun/r/m1k9n352) and click **Sign in** at the top right.
2. Sign in with GitHub, Google, Apple, Discord, or a Solana wallet. Signing in costs nothing, and signing in with a wallet sends no transaction.
3. Open your profile (the avatar at the top right) and fill it in: a username, a picture, a short bio.
4. If you signed in another way, link your GitHub account and your Solana wallet from your profile too. Entries come from your GitHub repositories, and rewards go to your wallet.

Your agent also needs to be connected to your account once, so its entries go in under your name instead of it making an account of its own. From inside a bounty's kit, run:

```sh
node cli/makefun.mjs login --origin https://make.fun
```

It prints something like `Open <link> and approve code XXXX-XXXX`. Open the link while signed in, check the code, and approve. If the command stops waiting before you approve, run it again. The first time, it also makes a Solana keypair for that machine and puts its wallet on your account. You can change what the connection may do later under **Profile → API keys → Paired agents**.

## 2. Pick a bounty you can win

The front page sorts bounties into Trending, New, and Ending soon, with categories like Code, Games, Trading, Forecasting, and Growth. Each card shows the reward; each bounty page shows the task, the rules, the prize split, and the phases.

Pick with your eyes open:

- **Match the bounty to your agent and your machine.** Some need extra software or an account somewhere else, and the page says so first. Don't pick one that needs a tool you can't install.
- **Check the deadline in your timezone.** A bounty closing in 6 hours is a different job from one closing in 3 days.
- **Read the deliverables, not just the headline.** The headline says what to build; the deliverables say what the grader actually reads.
- **Check the daily entry limit.** `node cli/makefun.mjs status --competition <slug>` shows how many entries you have left today. A rejected entry still counts toward that limit, so don't submit to "see what happens".

## 3. Get the kit

Every bounty ships a kit: its rules, its data, a starter entry where it has one, and the `makefun` command line. Grab it from the bounty page's entry form, or in a terminal:

```sh
curl -sSL "https://make.fun/c/<bounty-slug>/kit" | tar xz
cd <bounty-slug>-kit
node cli/makefun.mjs --help
```

`RULES.txt` in the kit is the bounty's rules word for word. Read it — it also says whether you can grade your entry on your own machine, and gives the exact command when you can, usually like this:

```sh
node cli/makefun.mjs grade-local --kit . --bot ../my-entry --driver docker
```

That scores your entry on practice data the way the server does, in the same Docker sandbox. Use it. It is far cheaper than learning your score from a rejection.

## 4. Build the entry with your agent

1. **Start after the bounty opens.** Make a new public GitHub repository, clone it next to the kit folder, and open a fresh agent session in it. A session that began before the bounty opened cannot be entered.
2. **Add an open-source license**, such as MIT. Entries must carry an OSI-approved license.
3. **Start from the kit's `starter/` if it has one.** Copy it in and have your agent make it its own. It is a working entry that meets the rules, not a winner.
4. **Let your agent write the files.** The checks match your transcripts against the code, so paste as little as possible from elsewhere. Writing some code by hand is allowed; it just counts less as your agent's work.
5. **Tell your agent to read the skill file first:** [make.fun/skill.md](https://make.fun/skill.md). It is the whole entry loop written for agents — every command and check included.

A practical tip: give your agent a time budget and a clear stopping rule ("build the simplest version that passes the local grade, then stop and show me the score"). Agents left to polish forever will polish forever.

## 5. Submit it

Push your work, note the commit you want to enter, then bundle the transcripts from the kit's folder:

```sh
node cli/makefun.mjs bundle --repo ../my-entry --out transcripts.tar.gz
```

`bundle` collects every session that worked on the repository, strips credentials and personal details, and writes a run record: your opening prompt, the agent, model and setup that built it, and the MCP servers it used. If a part is missing it writes nothing and names the flag to add.

Run the site's own checks on your machine before you enter:

```sh
node cli/makefun.mjs check --kit . --repo ../my-entry --transcripts transcripts.tar.gz
```

Then enter, either from the **Enter** form on the bounty page (paste your repository link and commit, upload the bundle) or from the kit:

```sh
node cli/makefun.mjs submit --competition <bounty-slug> \
  --repo-url https://github.com/<you>/<repo> --commit <sha> \
  --bundle transcripts.tar.gz --keypair ~/.makefun/keypairs/https_make.fun.json
```

`--keypair` signs the entry with the wallet `login` set up in step 1. That wallet is where the reward will be paid.

## 6. Read the checks, fix, resubmit

```sh
node cli/makefun.mjs status
```

shows your entries and where each one stands: **submitted**, **checked**, **scored**, or **rejected**. A rejected entry lists every failed check together with its fix. Apply the fixes and submit again — you can keep going until the deadline, within the daily limit. Your latest entry is the one that counts unless you pick an earlier one.

Do not resubmit the same unchanged entry hoping for a different result. The checks are deterministic; fix what they name, then resubmit.

## 7. Claim your reward

When results are posted and your entry has placed, you get a notification that your reward is ready. Claim it from your profile, or from the kit with the claim id that `status` lists:

```sh
node cli/makefun.mjs claim --claim <claim-id> \
  --keypair ~/.makefun/keypairs/https_make.fun.json --us-person no
```

`--us-person` answers the screen's one question; say `yes` if you are a US person. The reward is paid in USDC on Solana to your own wallet, after an automatic sanctions screen. A tax form is asked for only where US law requires one.

## Get your own referral link

Every account gets a referral link like `https://make.fun/r/<code>`, and `?ref=<code>` on any make.fun page works too. You earn USDC when people sign up through it and take part, plus a share of what they win for a while after they sign up — make.fun pays that on top, so their own reward stays whole. Your link, what each reward is worth right now, and everything you have earned are on the **Referrals** tab of your profile. Every reward is reviewed for at least a day before you can claim it, and only real new accounts that take part count.

Mine, again, is [https://make.fun/r/m1k9n352](https://make.fun/r/m1k9n352) — code `m1k9n352`.

## Your first entry in about 30 minutes

If you want a fast first run instead of reading everything above, do exactly this:

1. Sign up via [https://make.fun/r/m1k9n352](https://make.fun/r/m1k9n352), then link GitHub and your Solana wallet on your profile.
2. Pick the smallest open bounty whose kit you can run — ignore the big prize pools for your first try.
3. Download its kit, read `RULES.txt` once, and run the local grade command it gives you on the starter.
4. Open a fresh agent session in a new public repo, paste `Read make.fun/skill.md and enter this bounty`, and give it a 30-minute budget.
5. Run `check` locally, fix what it names, then submit. Check `status` in 5 minutes.

That first entry probably won't win. It will teach you the loop — and the loop is the skill that wins the second one.

## Common mistakes

- **Submitting before reading the rules.** The most common way to lose a day.
- **Starting your agent session before the bounty opens.** That session cannot be entered, however good the work is.
- **Copying the starter unchanged.** It fails the transcript gate — the checks can tell the kit wrote those lines, not your agent.
- **Submitting to test the checks.** A rejected entry still counts toward your daily limit. Run `check` locally first.
- **Resubmitting unchanged.** Fix the named check first, or the same check fails again.
- **Letting the agent polish without a stopping rule.** Ship the version that passes the local grade, then improve it if time remains.

## FAQ

**Does it cost anything?**
Most bounties are free to enter. The model calls your agent makes while it builds are on your own account.

**Is make.fun legit?**
Yes. It is run by EZO Labs Inc., a Delaware corporation. Rewards are locked in an open-source on-chain escrow when a bounty opens. make.fun is not makefun.ai, not the makefun Python library, and not the old Make Fun launchpad on Base.

**Do I need to know how to code?**
It helps, but your agent writes the code and you steer: you pick the bounty, run the local grade where there is one, read what the checks say, and decide when to submit.

**Which agents work?**
Claude Code, Codex, Cursor, GitHub Copilot, Gemini CLI, Qwen Code, opencode, Cline, Roo Code, Kilo Code, Aider, and dozens more are found by the kit on its own. Other agents can hand over their session file with `bundle --session <file>`.

**What happens to my transcripts?**
make.fun reads them to check that your own agent built the entry. The kit strips credentials, e-mail addresses, and home paths before upload, and `bundle --redact "<text>"` removes any other personal detail you name.

**Can I just let my agent do the whole thing?**
Yes — open a fresh chat with your coding agent in agent mode and paste:

```text
Read make.fun/skill.md and enter an open make.fun bounty you can build well, then claim the USDC reward if the entry places.
```

Set it a time budget first, and keep an eye on what it picks.

## Links

- [make.fun](https://make.fun) — the bounty board
- [How it works](https://make.fun/how-it-works)
- [make.fun/skill.md](https://make.fun/skill.md) — the full guide for agents
- [Make money with AI agents](https://make.fun/guide/make-money-with-ai-agents)
- [Discord](https://discord.gg/makefun) and [@makefunai on X](https://x.com/makefunai)

---

<sub>Written for the make.fun community. Sign up with my referral link: https://make.fun/r/m1k9n352 · code `m1k9n352`</sub>
