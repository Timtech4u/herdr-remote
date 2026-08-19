# Medium draft

**Do not publish yet.** Featured image: https://raw.githubusercontent.com/Timtech4u/herdr-remote/main/images/featured.png


Medium has no connector in this setup, so this file is the import-ready post.

## How to publish

1. Open [medium.com/new-story](https://medium.com/new-story) while signed in as [timtech4u](https://timtech4u.medium.com/).
2. *More → Import a story* and paste the raw GitHub URL for `article.md` after this repo is public.
3. Or paste the body below. Upload the two PNGs from `images/` when Medium strips relative paths.
4. Tags that fit your existing Medium: `AI agents`, `Developer Tools`, `Remote Work`, `DevOps`, `Open Source`.
5. Canonical link back to this repo or to DEV.to if you publish there first (avoid duplicate-SEO fights).

## Image URLs (fill after first push)

Replace after the repo exists:

- Herdr + Grok: `https://raw.githubusercontent.com/Timtech4u/herdr-remote/main/images/herdr-grok.png`
- Tailnet: `https://raw.githubusercontent.com/Timtech4u/herdr-remote/main/images/tailscale-machines.png`

---

# The machine can live anywhere. Herdr is how you sit down at it.

In April I wrote that [your AI coding agent deserves its own cloud machine](https://dev.to/timtech4u/your-ai-coding-agent-deserves-its-own-cloud-machine-1ino). I meant it. I moved Claude Code onto a GCP VM, put it in tmux, tunneled ports back to localhost, and treated the laptop as a thin client. From my phone I checked on the agent while grabbing coffee.

That article was about *where the work runs*. This one is about *how you sit down at it*.

The missing piece is [Herdr](https://herdr.dev) — specifically `herdr --remote`.

Yesterday Duyet posted that he installed Tailscale and Herdr on a [Grok Bot](https://x.com/_duyet/status/2089665924454633766) computer, then attached from his Mac:

```
herdr --remote box@duyetbot
```

I did the same thing today, on a box named `grey`. One command from the MacBook. On the other side: a persistent Herdr server, a workspace, and Grok 4.6 already running.

![Herdr on a remote Linux box. Grok 4.6 is in the pane. The sidebar already sees the grok agent.](https://raw.githubusercontent.com/Timtech4u/herdr-remote/main/images/herdr-grok.png)

I did not SSH in first and then remember tmux binds. I did not rebuild a layout. The session was already there.

That is the product.

## The gap after tmux

tmux keeps a shell alive. That is why I used it in April. Close the laptop, come back, `tmux attach`. It works.

What tmux does not do:

- know which pane is an agent
- tell you which one is blocked, working, idle, or done
- give you a thin client that feels like the machine is in front of you
- install itself on a fresh box with one command
- stream the same UI from a Vercel sandbox, a Grok Bot computer, a Cursor VM, or a Hetzner VPS without changing how you work

[Herdr's own compare page](https://herdr.dev/compare/) puts it cleanly: tmux sees panes. Herdr sees agents. Manager apps (Conductor, Emdash, the rest) put the herd in a window — quit the window, the herd goes with it. Herdr is the other way around. The server owns the terminals. Every UI is a client. Detach, crash the TUI, close the lid. The agents do not notice.

Can Celik, who built Herdr, said the quiet part in the [Y Combinator announcement](https://herdr.dev/blog/herdr-is-joining-y-combinator/) (F26, runtime stays Apache-2.0):

> Once you have the runtime, where they run stops mattering. You should be able to run them anywhere and keep them running.

And then the roadmap sentence that this post is really about:

> A laptop, a VPS for the six-hour job, a sandbox for risky code. Herdr can run anywhere today, but those machines are disconnected. They should be connected.

`--remote` is how they start connecting.

## What `--remote` actually is

Two official paths, from [How to work with Herdr](https://herdr.dev/docs/how-to-work/):

**1. SSH first, then Herdr** — tmux-style. Use this on a phone.

```
ssh you@server
herdr
```

**2. Thin client from the machine you are holding.** This is the one that feels like the box is local. Your Herdr talks SSH, starts or attaches the remote server, and streams the UI back. Local keybinds stay local. A screenshot on your Mac clipboard can land in the remote pane.

```
herdr --remote workbox
herdr --remote ssh://you@server:2222
herdr --remote box@grey
```

If the remote does not have Herdr yet, an interactive `--remote` will install it. Can's line: depending on the network, you are a couple of seconds from an agent running.

Detach with `ctrl+b q`. The server stays. Reattach with the same command.

That is the inversion. The laptop is no longer the computer. The laptop is the chair.

## The menu of machines

Compute got cheap and weird. You can rent a place for an agent in a dozen shapes before lunch. None of these are theoretical — they all speak SSH, or something close enough.

- **Your laptop** — `herdr`. Fast UI, bad thermals.
- **A cloud VM** (GCP, AWS, Hetzner, Fly) — `herdr --remote user@vm`. Always-on, you pay for the hour.
- **[Vercel Sandbox](https://vercel.com/docs/sandbox)** — Firecracker microVM, persistent by default, SSH as of January 2026. `sandbox ssh <id>` then `herdr`.
- **Grok Bot's computer** — a real Linux box next to the chat. Tailscale (or any SSH) + `herdr --remote box@grey`.
- **Cursor cloud / cloud agent VM** — SSH if the environment exposes it, then Herdr.
- **A GPU pod** — the job that should not run on a MacBook. Same `--remote`.
- **Your phone** — no Herdr app. Any SSH client, then `herdr`. The TUI adapts.

The providers will keep multiplying. The attach command does not have to.

Vercel is the example people keep asking about, so here is the honest version. [Sandbox](https://vercel.com/docs/sandbox) is a Firecracker microVM for untrusted or agent-generated code. It is persistent by default: stop it, the filesystem snapshots, resume later. In January 2026 they shipped [`sandbox ssh`](https://vercel.com/changelog/ssh-into-running-sandboxes-with-the-sandbox-cli). While you are connected, the timeout extends in 5-minute increments, up to five hours. That is not a 24/7 VPS. It is a scratch computer you can actually sit down at. Put Herdr on it and the session outlives the tab you used to create the sandbox — for as long as the sandbox itself is allowed to live.

Grok Bot, Cursor Cloud, Codespaces, a home lab: same pattern. The product gives you a machine. Herdr gives you a place to inhabit it with the agent UI you already use.

## A worked example: Grok Bot + Tailscale + a Mac

I am not going to pretend this is the only way. It is the way I actually did it today, after Duyet's tweet.

**On the box** (the Grok Bot computer):

```
curl -fsSL https://herdr.dev/install.sh | sh
herdr server
grok
```

**A pipe.** `--remote` is SSH. I used Tailscale because the box has no public IP I wanted to open. Same tailnet on the Mac and the box. Hostname: `grey`.

![Same tailnet: the MacBook, a phone, a devbox, and grey — the Herdr host.](https://raw.githubusercontent.com/Timtech4u/herdr-remote/main/images/tailscale-machines.png)

If you mix Google and GitHub identities, you will land on two tailnets and the machines will not see each other. I did that. Fix: pick the identity your laptop already uses.

**On the Mac:**

```
herdr --remote box@grey
```

Grok was already in the session. I did not start it from the laptop.

**The catch:** closing the Grok Bot chat is fine. Quitting Grok Bot, or anything that sleeps or recreates that computer, is not. Treat Grok Bot as a convenient host, not an SLA. For a real always-on herd, use a VM you control. The attach command stays the same.

## How I work now

```
herdr --remote box@grey
```

A throwaway repro? Vercel Sandbox, `sandbox ssh`, `herdr`, do the dangerous thing, kill it.

A six-hour eval? The GPU box. Same `--remote`.

Phone? `ssh box@grey` then `herdr`.

## What this is not

It is not "SSH, but pretty." It is a runtime with clients.

It is not a replacement for Cursor, Claude Code, Grok Build, or Codex. Herdr does not wrap them. It owns their terminals.

It is not locked to Tailscale. If `ssh you@host` works, `herdr --remote you@host` works.

The April article was: give the agent a machine, put tmux on it, make the laptop thin.

This one is: the machines are already everywhere. Herdr is how you sit down at any of them without changing the ritual.

The chair is portable. Put it on every computer you actually want to use.

---

*Sources in the [repo](https://github.com/Timtech4u/herdr-remote). Screenshots from 19 Aug 2026.*
