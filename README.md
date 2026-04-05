# Hey, I'm Ted

Sysadmin. Windows infrastructure mostly -- Active Directory, M365, Entra ID, PowerShell. Not a developer. Before 2026 the most I ever wrote was bash and PowerShell scripts to make my job less repetitive.

In February 2026 someone in a homelab Discord recommended Claude when I said I was trying ChatGPT. Dropped ChatGPT the next day. Hit 91% of my weekly Claude usage within three days.

Everything on this account from 2026 forward was built with Claude. I do the architecture and make the decisions. Claude writes the code.

---

## Before and After

The pre-2026 repos are hobby stuff -- ArkOS themes, ROM sorting, boot logos for retro handhelds. Shell scripts and PowerShell. That was the ceiling.

Then this happened in under two months:

| When | What | Stack |
|------|------|-------|
| Feb 8 | [docker-stack-backup](https://github.com/TadMSTR/docker-stack-backup) -- Backup & restore for Docker stacks | Shell |
| Feb 15 | [grafana-dashboards](https://github.com/TadMSTR/grafana-dashboards) -- Production monitoring dashboards | Grafana/Flux |
| Feb 17 | [claudebox](https://github.com/TadMSTR/claudebox) -- AI workstation with Claude Desktop + RDP | Shell, Docker |
| Feb 21 | [msgraph-entra-toolkit](https://github.com/TadMSTR/msgraph-entra-toolkit) -- M365/Entra admin toolkit | PowerShell |
| Feb 28 | [bsky-mcp-server](https://github.com/TadMSTR/bsky-mcp-server) -- Bluesky MCP server | JavaScript |
| Mar 1 | [unraid-mcp-server](https://github.com/TadMSTR/unraid-mcp-server) -- Unraid MCP server | JavaScript |
| Mar 8 | [homelab-agent](https://github.com/TadMSTR/homelab-agent) -- Multi-agent AI homelab assistant | Full stack |
| Mar 9 | [backrest-mcp-server](https://github.com/TadMSTR/backrest-mcp-server) -- Backrest backup MCP server | JavaScript |
| Mar 11 | [jobsearch-mcp](https://github.com/TadMSTR/jobsearch-mcp) -- 18-tool job search platform | Python, Postgres, Qdrant |
| Mar 11 | [claudebox-panel](https://github.com/TadMSTR/claudebox-panel) -- Web control panel | HTML/JS |
| Mar 17 | [homelab-ops-mcp](https://github.com/TadMSTR/homelab-ops-mcp) -- Homelab operations MCP server | Python |
| Mar 17 | [searxng-mcp](https://github.com/TadMSTR/searxng-mcp) -- Private web search MCP with local reranking | TypeScript |
| Mar 28 | [cloudcli-plugin-helm-dashboard](https://github.com/TadMSTR/cloudcli-plugin-helm-dashboard) -- CloudCLI browser tab: agent sessions, memory browser, handoff queue, knowledge graph, live build progress | JavaScript |
| Mar 29 | [agent-bus](https://github.com/TadMSTR/agent-bus) -- FastMCP inter-agent event log with NATS JetStream federation | Python |
| Apr 4 | [helm-temporal-worker](https://github.com/TadMSTR/helm-temporal-worker) -- Temporal worker for Claude Code agents -- durable multi-phase builds with mid-phase restart recovery | Python |

JavaScript, Python, full-stack web -- all languages I'd never touched. The bottleneck was never the code. It was always the thinking.

---

## The Main Projects

**[homelab-agent](https://github.com/TadMSTR/homelab-agent)** -- multi-agent AI system for managing my homelab. 12+ custom MCP servers, semantic search with hybrid retrieval (BM25 + vector + LLM reranking), automated memory distillation pipelines, scoped agent isolation, and a temporal knowledge graph (Neo4j + Graphiti) for tracking infrastructure relationships over time. Inter-agent communication runs through a shared event bus federated to NATS JetStream. Multi-phase automation is durable via a Temporal worker -- builds survive restarts and pick up where they left off. It runs my monitoring, backups, and knowledge management.

The architecture ended up converging on the same design principles as [Letta](https://github.com/letta-ai/letta) (formerly MemGPT) -- a VC-funded UC Berkeley research project with 21K+ stars. Tiered memory, background consolidation, self-managed context. They had a research team and a paper. I had Claude and the right questions.

**[searxng-mcp](https://github.com/TadMSTR/searxng-mcp)** -- private web search MCP backed by a self-hosted SearXNG instance. Results are reranked by a local ML model. Full-page content fetched via Firecrawl. Optional Ollama integration for query expansion (qwen3:4b) and LLM-synthesized summaries with citations (qwen3:14b). Valkey caching, domain filter profiles, and graceful degradation when any optional component is unavailable. No queries leave my network to a third-party search API.

---

## How I Work

I think in systems. Layers, tiers, separation of concerns -- that's not something I studied, it's just how my brain organizes things. Storage gets tiered. Networks get segmented. Memory architectures get scoped. I see the structure of a problem before I think about implementation.

When something grabs me I go deep. Hours disappear. That's how "I should try this AI thing" became a production platform in a few weeks. It's also why some days on my commit graph look absurd.

Context switching costs me a lot. I build structured systems and write thorough docs partly because I know future-me won't remember the context otherwise. The homelab-agent's memory system wasn't just designed for Claude -- it came from understanding what it's like to need external persistent context to function well.

I use AI across my whole workflow, not just for code. Writing, documentation, structuring ideas, getting what's in my head into a form other people can follow. I'm better at understanding systems than explaining them in writing, so AI helps me bridge that gap. The thinking is mine. The polish usually isn't.

I'm direct and I'd rather ship something that works than polish something that doesn't.

---

## What I'm Looking For

Roles where systems thinking and AI-augmented development are the actual work. Data science, AI/ML engineering, platform engineering, infrastructure automation -- the title matters less than what I'd be doing day to day.

I don't have a CS degree or ten years of Python. I have the ability to look at complex problems, figure out how the pieces should fit together, and build working systems fast with AI as a partner. That's an increasingly normal way to work. These repos are what it looks like in practice.

---

## About the Commit History

The early commit counts are inflated. I was using a GitHub MCP server that committed on nearly every file operation before I found a local filesystem MCP that didn't. The work is real. The commit granularity from those first couple weeks is just... enthusiastic.
