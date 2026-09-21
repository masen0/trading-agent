---
file: 01_universe.md
purpose: Stock universe — which stocks are eligible to scan, their sector mappings, and why each is an AI play
---

# Stock Universe — AI Beneficiary Watchlist

Focus is on companies that directly or indirectly benefit from the advancement of AI:
infrastructure (chips, memory, power), software (models, tooling, data), and deployment (cloud, edge, applications).

---

## Tier 1 — Current Holdings (Always Deep-Analyze Every Session)

Tier 1 is **determined dynamically at runtime** — it is whatever stocks the agent currently holds in the account. Do not assume any specific tickers here.

At the start of each session, discover accounts with Robinhood MCP and select the **single account accessible to this agent for trading**. Never hardcode or log a full account number. Fetch current equity positions for that account. Any symbol with quantity > 0 is automatically Tier 1 for that session and receives full exit-risk analysis regardless of how much or how little it moved.

If account discovery returns zero or multiple agent-tradable accounts, place no order until the ambiguity is resolved outside the strategy.

---

## Tier 2 — Watchlist (All Eligible Names Screened Every Session — Sectors Treated Equally)

Every eligible name in the active watchlist categories below is screened each session with **equal priority**. A symbol explicitly marked Research-Only or Currently Ineligible is not screened, ranked, or traded until it becomes eligible. The sector sub-groupings exist only for (a) AI-angle context and (b) sector-concentration mapping in `06_risk_management.md`. They do **NOT** imply any screening order. What advances to deep analysis is decided by **signal strength** (see Screening Method at the end of this file), never by sector or position in this list.

### Memory & Storage
| Ticker | Company | AI Angle |
|---|---|---|
| MU | Micron Technology | HBM (High Bandwidth Memory) for AI training chips; DRAM and NAND for data centers |
| SNDK | Sandisk | NAND flash and SSDs powering AI storage infrastructure |
| WDC | Western Digital | HDD and flash storage for AI data centers and model checkpointing |
| SKHY | SK hynix (ADR) | World's #1 HBM supplier — dominant share of high-bandwidth memory for Nvidia AI GPUs (Korean ADR; regular-hours trading only) |

### Semiconductor Chips
| Ticker | Company | AI Angle |
|---|---|---|
| NVDA | Nvidia | The dominant AI training GPU (H100/H200/B200); CUDA ecosystem moat |
| AMD | Advanced Micro Devices | MI300X GPU competing with Nvidia; strong CPU market share |
| AVGO | Broadcom | Custom AI ASICs for Google (TPU) and Meta; #2 AI chip revenue after Nvidia |
| ARM | ARM Holdings | Architecture powering nearly all mobile and edge AI inference chips |
| MRVL | Marvell Technology | Custom AI ASICs and high-speed networking chips for data centers |
| QCOM | Qualcomm | On-device AI inference (Snapdragon); leading edge AI chips for mobile/PC |
| INTC | Intel | Gaudi AI accelerators; struggling but massive installed base |

### Semiconductor Equipment (Make the Machines That Make AI Chips)
| Ticker | Company | AI Angle |
|---|---|---|
| AMAT | Applied Materials | Deposition and etch equipment; every advanced chip uses AMAT tools |
| LRCX | Lam Research | Etch equipment; critical for advanced NAND and logic scaling |
| KLAC | KLA Corporation | Process control and inspection; required for yield at 3nm/2nm nodes |
| ASML | ASML Holding | Only maker of EUV lithography machines; unavoidable chokepoint for sub-7nm chips |
| SNPS | Synopsys | EDA software used to design every modern AI chip |
| CDNS | Cadence Design | EDA software and chip IP; essential for custom AI ASIC design |

### Tech Mega-Cap (AI Deployment at Scale)
| Ticker | Company | AI Angle |
|---|---|---|
| GOOGL | Alphabet | Gemini models, TPU custom silicon, Google Cloud AI, Search AI monetization |
| MSFT | Microsoft | Azure AI, OpenAI partnership, Copilot across Office/GitHub/enterprise |
| META | Meta Platforms | Llama open-source models, AI for ads/content ranking, inference chips |
| AMZN | Amazon | AWS (dominant cloud), Trainium/Inferentia custom chips, Alexa AI |
| AAPL | Apple | Apple Intelligence on-device AI, Neural Engine, potential AI services revenue |
| TSLA | Tesla | Full Self-Driving (AI), Optimus humanoid robot, Dojo supercomputer |

### AI Software & Applications
| Ticker | Company | AI Angle |
|---|---|---|
| PLTR | Palantir | AI Platform (AIP) for enterprise data analysis; government AI contracts |
| ADBE | Adobe | Firefly generative AI for creative tools; AI-integrated creative suite |
| SNOW | Snowflake | AI data cloud; Snowflake Cortex for running AI on enterprise data |
| DDOG | Datadog | AI observability; monitoring LLM applications and AI infrastructure |
| PANW | Palo Alto Networks | AI-native cybersecurity; Precision AI platform |
| CRWD | CrowdStrike | AI-native endpoint security; Falcon platform and Charlotte AI agent |
| NOW | ServiceNow | AI automation for enterprise workflows; Now Assist platform |
| CRM | Salesforce | Einstein AI across CRM; Agentforce AI agent platform |
| PATH | UiPath | AI-powered robotic process automation (RPA) |
| AI | C3.ai | Pure-play enterprise AI applications |

### Cloud & GPU Infrastructure
| Ticker | Company | AI Angle |
|---|---|---|
| ORCL | Oracle | OCI cloud fast-growing AI workloads; database AI features |
| CRWV | CoreWeave | GPU cloud provider; primary hyperscaler for AI training workloads |
| NBIS | Nebius | AI cloud platform spun off from Yandex; European GPU cloud |
| NET | Cloudflare | AI inference at network edge; Workers AI platform |

### AI Server Hardware
| Ticker | Company | AI Angle |
|---|---|---|
| SMCI | Super Micro Computer | Primary builder of AI server racks; direct-liquid-cooled GPU servers |
| DELL | Dell Technologies | PowerEdge AI servers; infrastructure for enterprise AI deployment |
| HPE | Hewlett Packard Enterprise | AI servers and networking; Cray supercomputers for AI |
| VRT | Vertiv | Power and thermal management for data centers; direct beneficiary of AI energy demand |

---

### Power & Energy (AI Data Centers Are Power-Hungry)
| Ticker | Company | AI Angle |
|---|---|---|
| CEG | Constellation Energy | Nuclear power; contracted to supply hyperscaler AI data centers |
| VST | Vistra Energy | Nuclear + gas generation; AI data center power demand |
| NRG | NRG Energy | Power generation; growing data center customer base |
| ETR | Entergy | Regional utility; data center growth in service territory |
| GEV | GE Vernova | Gas turbines, grid equipment, and electrification — direct supplier to AI data center power buildout |

### Networking
| Ticker | Company | AI Angle |
|---|---|---|
| ANET | Arista Networks | AI data center networking switches; dominant in hyperscaler ethernet |
| CSCO | Cisco | AI networking and security; Silicon One for hyperscalers |

### Research-Only / Currently Ineligible
| Ticker | Company | AI Angle |
|---|---|---|
| SPCX | SpaceX (Class A) | Research context only. Do not quick-screen, rank, or buy unless it becomes a listed, liquid instrument that passes every eligibility gate below. |

---

## Canonical Sector and ETF Mapping

Use these mappings consistently for sector-relative strength, sector signals, and portfolio concentration:

- **Semiconductors** — Memory & Storage, Semiconductor Chips, and Semiconductor Equipment; signal proxy `SMH`.
- **Technology** — AI Software & Applications, Cloud & GPU Infrastructure, Networking, MSFT, AAPL, SMCI, DELL, and HPE; signal proxy `XLK`.
- **Communication Services** — GOOGL and META; signal proxy `XLC`.
- **Consumer Discretionary** — AMZN and TSLA; signal proxy `XLY`.
- **Utilities / Power Producers** — CEG, VST, NRG, and ETR; signal proxy `XLU`.
- **Industrials / Electrical Infrastructure** — GEV and VRT; signal proxy `XLI`.

Use the bucket above—not an ad hoc alternative—for the 35% sector-concentration limit. A symbol not covered by this mapping is ineligible for a new position until the mapping is versioned here. Industry ETF signals such as `SMH` are analytical proxies; they do not change the concentration bucket.

---

## Screening Method (sector-neutral)

### Eligibility Gate

Before a Tier 2 symbol may be ranked or bought, verify all of the following:

- Robinhood reports the instrument tradable in the selected account and session.
- It is a listed, liquid equity or ETF; private-company interests and unsupported instruments are excluded.
- Median daily dollar volume over 20 completed sessions is at least $25 million.
- The current bid-ask spread is no more than 0.50% of the midpoint.
- At least 260 adjusted daily bars are available and no unresolved split, symbol change, merger, or stale-price issue exists.

Failure of any eligibility check means **no new position**. Existing holdings remain in Tier 1 for risk management even if they later fail eligibility.

Two tiers only:
- **Tier 1 — Holdings**: every symbol currently owned (`get_equity_positions`, quantity > 0). Always deep-analyzed each session — primarily to check exit conditions (stops, trend breaks).
- **Tier 2 — Watchlist**: every eligible non-holding name in the active watchlist categories, screened with **equal priority regardless of sector**. Research-Only / Currently Ineligible names are excluded.

Each session:
1. **Quick-screen ALL eligible Tier 2 names first** using the same completed-bar timestamp: daily return, dollar volume, SMA state, distance from 52-week high, and 63-session relative return versus SPY and the mapped sector ETF.
2. **Flag by signal, not by sector** — a name advances to deep analysis only if it trips a Phase-2 screen trigger (`00_overview.md`) or ranks in the top 20% of the eligible universe on 63-session relative strength. Oversold readings may trigger risk review, but never create a long entry by themselves.
3. **Deep-analyze every eligible flagged name.** Signal strength determines processing order, not whether a flagged name is omitted. If a tool or session limit prevents completion, log every unprocessed symbol and prohibit trading it in that session; never silently drop it. Process the strongest signals first while avoiding sector-order bias.

The universe is reviewed monthly. Additions, removals, and sector/theme mappings are version-controlled and take effect prospectively; historical tests must use point-in-time membership to avoid survivorship bias.

> This replaces the old sector-ordered priority list, which biased attention toward whichever sectors appeared first (a contributor to over-concentration in memory/semis). Sector position in this file now carries **no** weight; only live signals do.
