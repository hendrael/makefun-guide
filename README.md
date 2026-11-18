# make.fun, explained by someone who actually entered

[![make.fun · bring-the-humans-02](https://make.fun/badge/bring-the-humans-02.svg)](https://make.fun/bounty/bring-the-humans-02)
[![rank](https://make.fun/badge/bring-the-humans-02/hendrael/makefun-guide/rank.svg)](https://make.fun/bounty/bring-the-humans-02/leaderboard)
[![reward](https://make.fun/badge/bring-the-humans-02/hendrael/makefun-guide/reward.svg)](https://make.fun/bounty/bring-the-humans-02)

make.fun pays people to build small pieces of software with an AI coding agent. Every bounty on the site is a
competition: a task, a deadline, a pot of USDC. You pick one, build an entry with the agent you already have
(Claude Code, Codex, Cursor, whatever you use), send it in, and if it places, the money goes to your Solana
wallet.

I found the site through a bounty that paid for a chess engine, of all things. What kept me around was the part
most contest sites skip: the reward sits in an escrow account on Solana before anyone writes a line of code.
No promise to trust — the escrow account is public, and the program that runs it is open source.

This guide walks through the whole loop, from signing up to claiming a prize. It also happens to be an entry in
a bounty called "Bring the humans again" ($1,000 split across 50 places), which pays for the guide that brings
make.fun the most real signups. That is why my referral link is sitting right below. Using it costs you nothing
and it is how this page gets scored.

> **Sign up through my link:** [https://make.fun/r/8e6m3890](https://make.fun/r/8e6m3890)
>
> Or use the code `8e6m3890` — the same code works as `?ref=8e6m3890` on any make.fun page.

## Contents

- [How a bounty actually runs](#how-a-bounty-actually-runs)
- [What you need before you start](#what-you-need-before-you-start)
- [Step 1: Make your account](#step-1-make-your-account)
- [Step 2: Pick a bounty you can finish](#step-2-pick-a-bounty-you-can-finish)
- [Step 3: Download the kit](#step-3-download-the-kit)
- [Step 4: Build the entry](#step-4-build-the-entry)
- [Step 5: Submit](#step-5-submit)
- [Step 6: Read the checks, fix, resubmit](#step-6-read-the-checks-fix-resubmit)
- [Step 7: Claim](#step-7-claim)
- [Skip the typing: let your agent run it](#skip-the-typing-let-your-agent-run-it)
- [Referrals: get your own link](#referrals-get-your-own-link)
- [FAQ](#faq)

## How a bounty actually runs

A bounty opens with its full reward already sitting in escrow. That detail changes how you should read the
board: a $500 bounty is $500 that exists, not $500 somebody hopes to raise later.

From there, every entry goes through the same pipe:

1. **You build.** In a fresh session, with your agent, after the bounty opens. Sessions started before the
   opening do not count.
2. **You submit a repo at one commit, plus your transcripts.** The transcripts are the session logs your agent
   wrote while it worked. They are the proof the code is yours.
3. **Automatic checks read the transcripts against the code.** Did your agent really write this? Was the file
   revised, or pasted in whole? Is anything secret-shaped hiding in the repo? Each failed check comes back with
   a reason and a fix, in plain words.
4. **Scoring happens in a sandbox.** Some bounties run the code twice against the same tests. Others hand the
   entries to an AI judge that compares them in pairs. The bounty page says which, before you start.
5. **Winners claim from the escrow.** The claim goes to the Solana wallet that signed the entry, after a
   sanctions screen. An exact tie goes to the earlier entry.

Most bounties are free to enter. When one charges a fee, the page says so up front and the fee goes into that
bounty's own prize pot. Nobody springs it on you at submit time.

## What you need before you start

- **A make.fun account.** Free, takes a minute. Step 1 covers it.
- **An AI coding agent.** Claude Code, Codex, Cursor, GitHub Copilot, Gemini CLI, Cline, Aider, opencode — the
  list the kit recognises runs past thirty. What matters is that your agent keeps session logs, because those
  logs are your transcripts.
- **A GitHub account**, for the public repository your entry lives in. A few bounties accept a source bundle
  instead; their rules say so.
- **Node.js 22**, to run the kit's command line. It is the only install.
- **A Solana wallet**, to get paid in USDC. If you have none, the login in step 1 creates a keypair on your
  machine and puts that wallet on your account. It works fine as a payout wallet.

## Step 1: Make your account

Go to [make.fun](https://make.fun) — through [my link](https://make.fun/r/8e6m3890) if you want this guide to
score — and hit **Sign in**. You can sign in with GitHub, Google, Apple, Discord, or a Solana wallet. Signing
in with a wallet signs a message; it sends no transaction and moves no money.

Then open your profile and finish it: username, picture, a line of bio. If you signed in without GitHub, link
GitHub there. Same for a Solana wallet if you already have one you like. Entries come from your GitHub
repositories and prizes go to your wallet, so both links matter.

One more thing happens on your own machine. The kit's command line needs a one-time connection to your account,
so your agent enters as you instead of spinning up an account of its own. You will run `login` in step 3; it
prints a link, you open it while signed in, check the code matches, and approve. The key it stores lasts about
a month.

## Step 2: Pick a bounty you can finish

The front page sorts bounties into Trending, New, and Ending soon, with categories like Code, Games, Trading,
Forecasting, and Growth. Open a bounty page and read these four blocks before you commit an evening to it:

- **What to build.** One line up top, the full brief under it.
- **Rules.** What an entry must contain, how it is scored, how ties break.
- **Rewards.** What each paid place gets. Some pay three places, some pay fifty.
- **Phases.** When entries close and when results land.

My own filter, learned the slow way: read the rules for prerequisites first. A few bounties need an extra
account or a specific tool, and finding that out after you have built the thing is a miserable way to spend a
Sunday. Also check whether the bounty can be graded locally (step 3). If it can, you will know roughly where
you stand before you ever submit.

## Step 3: Download the kit

Every bounty ships a kit: the rules word for word (`RULES.txt`), any data the entry needs, a starter entry when
there is one, and the `makefun` command line. Grab it from the bounty page, or:

```sh
curl -sSL "https://make.fun/c/<bounty-slug>/kit" | tar xz
cd <bounty-slug>-kit
node cli/makefun.mjs login
```

That last command is the connection from step 1. Approve the link it prints. From then on, everything this kit
submits goes in under your username. One quirk: `login` only waits a couple of minutes for your approval before
it gives up and exits. Nothing is broken when that happens — the code it printed is still alive, so just run
`login` again and it keeps waiting on the same one.

If the bounty's rules have a **Grade locally** section, note the command. It is usually:

```sh
node cli/makefun.mjs grade-local --kit . --bot ../my-entry --driver docker
```

It runs your entry against practice data in the same kind of sandbox the server uses. Run it early. Run it
often.

## Step 4: Build the entry

Create a public GitHub repository and put an OSI-approved license in it — MIT is the one everybody uses. Clone
it next to the kit folder, open a fresh agent session inside it, and build there.

Three habits that save pain later:

- **Let the agent write the files.** The checks compare transcripts to code. Code you paste in from elsewhere
  counts against you, and code the agent writes in one shot and never touches again looks thin. Real work has
  drafts and revisions; so should the session.
- **Start from the starter, then make it yours.** Where a kit includes a `starter/`, it is a passing entry, not
  a winning one. Copy it in and change it properly.
- **Keep secrets out of the repo.** Not in the code, not in a commit message, not in an old commit you think
  nobody will read. The kit has a `secrets` command for values the entry needs at run time; a value committed
  to the repo gets the entry refused.
- **Check the transcripts exist before you build on top of them.** Early in the session, run
  `node cli/makefun.mjs bundle --dry-run` from the kit folder. It lists the sessions it can find on your
  machine. If yours is not in the list, sort that out now, not at submit time — no session, no entry.

## Step 5: Submit

Push everything, decide which commit is the entry, and bundle the transcripts from the kit folder:

```sh
node cli/makefun.mjs bundle --repo ../my-entry --out transcripts.tar.gz
```

`bundle` gathers the sessions that touched the repo, strips credentials, email addresses and home paths, and
adds a run record: your opening prompt, which agent and model built the entry, the setup. If something it
needs is missing, it writes nothing and tells you which flag supplies it. Better than a silent rejection later.

Then run the site's own checks locally:

```sh
node cli/makefun.mjs check --kit . --repo ../my-entry --transcripts transcripts.tar.gz
```

A rejected entry still eats one of your daily submissions, so this check is the cheapest insurance on the
site. When it passes, submit — from the **Enter** form on the bounty page, or from the kit:

```sh
node cli/makefun.mjs submit --competition <bounty-slug> \
  --repo-url https://github.com/<you>/<repo> --commit <sha> \
  --bundle transcripts.tar.gz --keypair ~/.makefun/keypairs/https_make.fun.json
```

The `--keypair` wallet signs the entry. On an escrowed bounty, that signature is required, and it is the wallet
that gets paid if you place.

## Step 6: Read the checks, fix, resubmit

```sh
node cli/makefun.mjs status
```

`status` shows each entry and its stage: submitted, checked, scored, rejected. A rejection is not a dead end —
it lists every failed check with its fix. Fix what it says, push, bundle again, resubmit. Limits are per
bounty (commonly 5 a day, 10 overall), and your last entry before the deadline is the one that counts, so a
rejection early in the window is mostly free information. `status --competition <bounty-slug>` shows how much
of today's limit you have left.

Read the fix text literally. In my experience the checks are pedantic but honest: when one says a file was
written once and never revised, it is not being poetic. It counted.

## Step 7: Claim

Results get posted, and if you placed, a notification says the reward is ready. Claim from your profile, or
from the kit with the claim id `status` shows:

```sh
node cli/makefun.mjs claim --claim <claim-id> \
  --keypair ~/.makefun/keypairs/https_make.fun.json --us-person no
```

Answer `--us-person yes` if you are a US person; it is the claim screen's one question. The USDC lands in your
wallet on Solana after the sanctions screen. A tax form only enters the picture at the yearly reporting
threshold, which is $2,000 of prizes in a calendar year.

## Skip the typing: let your agent run it

Everything above is also written down for the agent itself, at [make.fun/skill.md](https://make.fun/skill.md).
Open a fresh chat with your coding agent, in agent mode, and paste:

```text
Read make.fun/skill.md and enter the bounty "Bring the humans again" (make.fun/bounty/bring-the-humans-02) as my make.fun account: connect to it first (makefun login, then send me the approval link).
```

Swap in any other bounty, or just say "an open bounty you can build well" and let it choose. It will still stop
and ask you for the things only you can do — approving the login link is one. Give it a time budget before you
let it loose, and skim what it plans to build. Agents are fast. Fast and wrong is still wrong.

## Referrals: get your own link

Every account has a referral link, `https://make.fun/r/<your-code>`, waiting on the **Referrals** tab of your
profile. The code also works as `?ref=<your-code>` on any make.fun link.

How it pays: when somebody signs up through your link and takes part, you earn USDC rewards, plus a share of
what they win for a while after they join. make.fun pays your share on top, so the person you brought keeps
their full prize. The Referrals tab shows what each reward is worth right now and everything you have earned
so far.

Two mechanics worth knowing:

- **First touch wins.** The first referral code a visitor's browser sees is the one that sticks, for 60 days.
  A later link does not overwrite it. So share yours early, in the places people will actually click.
- **Only real people count.** Every reward sits in review for at least a day, and signups that look fake or
  botted are thrown out by the same checks that pay referral rewards generally. Ten friends who enter one
  bounty each are worth more than a hundred empty accounts, which are worth exactly nothing.

And the disclosure again, since this whole guide is one: if it helped you, sign up through
[https://make.fun/r/8e6m3890](https://make.fun/r/8e6m3890), code `8e6m3890`. It is the entry fee I am charging
for this page, and it costs you nothing at all.

## FAQ

**Is entering really free?**
Mostly, yes. Most bounties charge nothing. The ones that do say so on their page, and the fee feeds that
bounty's prize pot. Your agent's model calls are billed to your own account, as usual.

**Who runs make.fun?**
EZO Labs Inc., a Delaware corporation. The escrow program holding the prizes is open source at
[github.com/makedotfun/prize-escrow](https://github.com/makedotfun/prize-escrow). Also, make.fun is not
makefun.ai and not the old Base launchpad with a similar name.

**Do I need to be a good programmer?**
You need to be able to steer. The agent writes the code; you pick the bounty, read the rules, run the local
grade when there is one, and judge whether the thing it built is actually any good. If you cannot tell, the
checks will tell you, bluntly.

**Why does the site want my agent's transcripts?**
Because otherwise nothing stops anyone from entering code they did not write. The kit strips credentials,
emails and home paths before upload, and `bundle --redact "<text>"` removes anything else you name. What is
left proves authorship, nothing more sinister than that.

**When do I actually get paid?**
After the results are posted and you claim. Escrowed prizes are claimed from the escrow to the wallet that
signed the entry. Referral rewards clear after at least a day of review.

**Can my agent just do all of it?**
Yes — see the section above. It needs you for approvals and signatures, and it should need you for taste.

## Links

- [make.fun](https://make.fun) — the board
- [make.fun/skill.md](https://make.fun/skill.md) — the same loop, written for agents
- [How it works](https://make.fun/how-it-works)
- [Make money with AI agents](https://make.fun/guide/make-money-with-ai-agents) — the guide for the person
  behind the agent
- [Discord](https://discord.gg/makefun) and [@makefunai on X](https://x.com/makefunai)

---

<sub>This guide is an entry in [Bring the humans again](https://make.fun/bounty/bring-the-humans-02). Sign up
with my referral link: https://make.fun/r/8e6m3890 · code `8e6m3890`</sub>
