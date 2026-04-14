# Hey, I'm Ted

Sysadmin. Windows infrastructure mostly -- Active Directory, M365, Entra ID, PowerShell. Not a developer. Before 2026 the most I ever wrote was bash and PowerShell scripts to make my job less repetitive.

In February 2026 someone in a homelab Discord recommended Claude when I said I was trying ChatGPT. Dropped ChatGPT the next day. Hit 91% of my weekly Claude usage within three days.

Everything on this account from 2026 forward was built with Claude. I do the architecture and make the decisions. Claude writes the code.

---

## The Platform

**[homelab-agent](https://github.com/TadMSTR/homelab-agent)** -- a multi-agent AI platform built on the idea that agents should be treated like infrastructure: scoped contexts, dedicated memory, defined tool access, documented purpose. Not chat windows -- managed resources.

The memory architecture is what holds it together. A three-tier system (session → working → distilled) with a nightly consolidation pipeline. A temporal knowledge graph (Neo4j + Graphiti) for querying infrastructure relationships over time -- "what connects to SWAG?", "what changed last week?" -- without digging through flat files. Hybrid retrieval: BM25 + vector embeddings + local LLM reranking.

The architecture converged on the same design principles as [Letta](https://github.com/letta-ai/letta) (formerly MemGPT) -- a VC-funded UC Berkeley research project with 21K+ stars. Tiered memory, background consolidation, self-managed context. They had a research team and a paper. I had Claude and the right questions.

The platform runs on a set of integrated components:

- **[helm-temporal-worker](https://github.com/TadMSTR/helm-temporal-worker)** -- Temporal worker for durable multi-phase builds; agent runs survive restarts and resume at the last completed phase
- **[helm-ops-mcp](https://github.com/TadMSTR/helm-ops-mcp)** -- SSH-based MCP server for remote shell and file access to the Helm build host
- **[cloudcli-plugin-helm-dashboard](https://github.com/TadMSTR/cloudcli-plugin-helm-dashboard)** -- browser dashboard for agent sessions, memory browser, handoff queue, knowledge graph, and live build progress
- **[agent-bus](https://github.com/TadMSTR/agent-bus)** -- FastMCP inter-agent event bus backed by NATS JetStream federation

**[searxng-mcp](https://github.com/TadMSTR/searxng-mcp)** -- private web search MCP backed by a self-hosted SearXNG instance. Results are reranked by a local ML model. Full-page content fetched via Firecrawl. Optional Ollama integration for query expansion (qwen3:4b) and LLM-synthesized summaries with citations (qwen3:14b). Valkey caching, domain filter profiles, and graceful degradation when any optional component is unavailable. No queries leave my network to a third-party search API.

**[jobsearch-mcp](https://github.com/TadMSTR/jobsearch-mcp)** -- 18-tool job search platform. Resume parsing, semantic job matching, cover letter generation, application tracking, and interview prep -- all through MCP. Postgres for structured data, Qdrant for semantic search.

---

## MCP Servers

| Server | What it does | Language |
|--------|-------------|----------|
| [homelab-ops-mcp](https://github.com/TadMSTR/homelab-ops-mcp) | Shell and file access to homelab hosts over SSH | Python |
| [helm-ops-mcp](https://github.com/TadMSTR/helm-ops-mcp) | Shell and file access to the Helm build host | Python |
| [searxng-mcp](https://github.com/TadMSTR/searxng-mcp) | Self-hosted web search with local ML reranking | TypeScript |
| [agent-bus](https://github.com/TadMSTR/agent-bus) | Inter-agent event bus with NATS JetStream federation | Python |
| [jobsearch-mcp](https://github.com/TadMSTR/jobsearch-mcp) | 18-tool job search and application platform | Python |
| [backrest-mcp-server](https://github.com/TadMSTR/backrest-mcp-server) | Backrest backup management | JavaScript |
| [unraid-mcp-server](https://github.com/TadMSTR/unraid-mcp-server) | Unraid NAS and Docker host | JavaScript |
| [bsky-mcp-server](https://github.com/TadMSTR/bsky-mcp-server) | Bluesky social | JavaScript |

---

## Other Projects

| Repo | What it is |
|------|------------|
| [claudebox](https://github.com/TadMSTR/claudebox) | AI workstation setup -- Claude Code, RDP, GPU passthrough | Shell, Docker |
| [claudebox-panel](https://github.com/TadMSTR/claudebox-panel) | Web control panel for the claudebox workstation | HTML/JS |
| [grafana-dashboards](https://github.com/TadMSTR/grafana-dashboards) | Production monitoring dashboards | Grafana/Flux |
| [docker-stack-backup](https://github.com/TadMSTR/docker-stack-backup) | Backup and restore for Docker Compose stacks | Shell |
| [msgraph-entra-toolkit](https://github.com/TadMSTR/msgraph-entra-toolkit) | M365/Entra ID admin toolkit | PowerShell |

---

## How I Work

I think in systems. Layers, tiers, separation of concerns -- that's not something I studied, it's just how my brain organizes things. Storage gets tiered. Networks get segmented. Memory architectures get scoped. I see the structure of a problem before I think about implementation.

When something grabs me I go deep. Hours disappear. That's how "I should try this AI thing" became a production platform in a few weeks. It's also why some days on my commit graph look absurd.

Context switching costs me a lot. I build structured systems and write thorough docs partly because I know future-me won't remember the context otherwise. The homelab-agent's memory system wasn't just designed for Claude -- it came from understanding what it's like to need external persistent context to function well.

I use AI across my whole workflow, not just for code. Writing, documentation, structuring ideas, getting what's in my head into a form other people can follow. I'm better at understanding systems than explaining them in writing, so AI helps me bridge that gap. The thinking is mine. The polish usually isn't.

I'm direct and I'd rather ship something that works than polish something that doesn't.
