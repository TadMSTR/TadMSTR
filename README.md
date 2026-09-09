# Hey, I'm Ted

Sysadmin — Windows infrastructure mostly. Active Directory, M365, Entra ID, PowerShell. Before 2026 the most I'd written was bash and PowerShell to make my own job less repetitive.

In February 2026 someone in a homelab Discord suggested I try Claude instead of ChatGPT. I dropped ChatGPT the next day and hit 91% of my weekly usage within three days.

Everything on this account from 2026 forward was built with Claude. I do the architecture and make the decisions — what the failure modes are, where the trust boundaries sit, what "done" means. Claude writes the code.

---

## The Platform

**[homelab-agent](https://github.com/TadMSTR/homelab-agent)** — a reference build for running a team of AI agents on a single server.

Five agents — sysadmin, developer, research, writer, security — work semi-autonomously or fully unattended, coordinating through a task queue and communicating over Matrix. Each one gets a scoped tool surface controlled by a manifest, persistent multi-tier memory (Milvus for vector search, OpenSearch for full-text), and an event ledger recording every cross-agent handoff.

The part I find most interesting: the agents build the platform. Research plans a feature, developer writes it, writer documents it, security audits the result — then the new tool becomes available to the agents that built it. The three servers below were all built that way.

The host is **forge** — a Minisforum MS-A2, Ryzen 9 9955HX, 96 GB, RTX 2000 Ada, Debian 13. 60+ containers across 21 stacks, 30+ PM2 services.

The repo documents every piece of it and the reasoning behind each decision. It's meant to be copied.

---

## Selected Work

**[searxng-mcp](https://github.com/TadMSTR/searxng-mcp)** · TypeScript · [npm](https://www.npmjs.com/package/@tadmstr/searxng-mcp)

Private web search for AI agents. Self-hosted SearXNG metasearch, results reranked by a local ML model, then a four-tier fetch cascade — Firecrawl, Crawl4AI, raw HTTP, Wayback — with per-domain learning about which tier actually works where. Optional Ollama query expansion and synthesis. No query ever reaches a third-party search API.

The SSRF guard is the part I'd point at. Validating a URL string blocks literal private IPs but not a public hostname that resolves to one, and pre-resolving then fetching leaves a TOCTOU gap. So the check is installed as undici's connect-time DNS hook — the address validated is the exact one the socket connects to, re-checked on every redirect hop.

**[scoped-mcp](https://github.com/TadMSTR/scoped-mcp)** · Python · [PyPI](https://pypi.org/project/scoped-mcp/) · [docs](https://tadmstr.github.io/scoped-mcp/)

Per-agent MCP tool proxy. One process per agent: it loads only the tools that agent's manifest allows, scopes backend resources to that agent's namespace, injects credentials so the agent never sees them, and writes every tool call to a structured audit trail. `mcp_proxy` wires any existing MCP server into a manifest without custom code.

Optional hardening: per-agent rate limiting, HashiCorp Vault credentials with background renewal, argument-filter middleware that decodes base64/URL chains to catch obfuscated payloads, and operator-in-the-loop approval gating selected calls on an explicit decision — with a shadow mode for silent dry runs first.

**[githost-mcp](https://github.com/TadMSTR/githost-mcp)** · Python

Unified git access — local operations plus GitHub, Gitea, GitLab and Woodpecker — with a per-agent audit trail and a central workspace policy deciding who can write where.

Most of the interesting code is in the refusals. `git remote add x "ext::sh -c '…'"` runs a shell command on the next fetch, so scp-style remote parsing carries a lookahead specifically to stop `ext::` matching the `host:path` shape. Credentials in a remote URL are refused rather than redacted, because redaction would silently store a broken remote while the token persisted in `.git/config`. Write globs normalise paths before matching, since `fnmatch` has no path-segment awareness and `docs/../src/x.py` otherwise matches `docs/**`.

**[webhook-doorman](https://github.com/TadMSTR/webhook-doorman)** · Python

A fail-closed inbound webhook router. One ingress, per-source verification declared in YAML, durable delivery with retry and a dead-letter queue.

It exists because a service I'd written earlier verified signatures with `if not SECRET: return True` on an internet-reachable bind. "Verification skipped" is the outcome that must never be reachable, so here an absent secret disables the source and rejects, HMAC is computed over the raw bytes as received, every credential comparison is constant-time, and unverified sources match the socket peer against a CIDR allowlist — never `X-Forwarded-For`, which the caller controls.

Newest repo here, and the one held to the highest standard: it opened with 145 tests at 95% enforced coverage on the first commit.

**[vikunja-mcp](https://github.com/TadMSTR/vikunja-mcp)** · Python · [container](https://github.com/TadMSTR/vikunja-mcp/pkgs/container/vikunja-mcp)

The Vikunja REST API as scoped per-agent MCP tools. Agents file their own tickets when they find something they can't fix in scope, with idempotency keys and commit backlinks so the tracker stays honest about what actually shipped.

---

## How This Gets Built

Every build goes through the same loop: a plan, an implementation, then an independent security audit filed as a written report with a per-finding verdict, then remediation, then merge. The audit is a real gate — it runs inside the PR window and it regularly kills the build's own fix. One recent finding disproved a credential-redaction commit by showing that Node's `fetch()` embeds the raw URL in its own `TypeError`, leaking the password the commit existed to protect.

Repos are held to a written standard with three tiers, and a checker that measures conformance rather than assuming it. The rule that keeps it honest: **every requirement has to name the incident it prevents, or it gets dropped.** A requirement with no evidence behind it is an opinion, and opinions are how a standard rots without anyone noticing.

---

## How I Work

I think in systems. Layers, tiers, separation of concerns — not something I studied, just how my brain organises things. Storage gets tiered. Networks get segmented. Memory architectures get scoped. I see the structure of a problem before I think about implementation.

When something grabs me I go deep and hours disappear. That's how "I should try this AI thing" became a production platform. It's also why some days on my commit graph look absurd.

Context switching costs me a lot. I build structured systems and write thorough docs partly because I know future-me won't remember the context otherwise. The platform's memory system wasn't only designed for the agents — it came from knowing what it's like to need external persistent context to function well.

The habit that matters most is refusing to let a proxy stand in for the thing itself. A 200 can come from a different service that happens to hold the port. Configured is not the same as reachable. A green test suite can sit on top of a tool that's been dead for months. Most of what I've learned the hard way is some version of that, and it's the reason AI-assisted work comes out solid instead of merely plausible.

I'm direct, and I'd rather ship something that works than polish something that doesn't.

---

<!-- Add once the Discord actually has channels and a few people in it: -->
<!-- ## Community -->
<!-- Support and discussion for the projects above: [Discord](https://discord.gg/XXXX) -->

[58 public repos →](https://github.com/TadMSTR?tab=repositories)
