# The machine can live anywhere. Herdr is how you sit down at it.

Article + screenshots. Sequel to [Your AI Coding Agent Deserves Its Own Cloud Machine](https://dev.to/timtech4u/your-ai-coding-agent-deserves-its-own-cloud-machine-1ino).

**Canonical:** [article.md](article.md)

**Medium-ready:** [medium.md](medium.md) — same piece, with notes for import.

## Thesis

Compute is everywhere now: a Vercel Sandbox, a Grok Bot computer, a Cursor VM, a Hetzner box, a GPU pod. Those products give you a machine. [Herdr](https://herdr.dev) `--remote` is how you sit down at any of them without changing the ritual.

```bash
herdr --remote box@grey
```

## Screenshots

- `images/herdr-grok.png` — Herdr TUI on the remote box, Grok 4.6 in the pane
- `images/tailscale-machines.png` — the tailnet used as one SSH pipe (not required)

## Publish

- **GitHub:** this repo is the source of truth
- **DEV.to:** paste `article.md` (relative images work if you upload them)
- **Medium:** see `medium.md`. No Medium API from here — import the GitHub raw URL or paste.

## License

Writing: [CC BY 4.0](LICENSE). Screenshots: taken on my machines, same license. Herdr, Tailscale, Vercel, Grok, Cursor are their owners' marks.
