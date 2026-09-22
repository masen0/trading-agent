---
file: 01_universe.md
purpose: Stock universe — which stocks to screen, the canonical sector map, and why each is an AI play
---

# Stock Universe — AI Beneficiary Watchlist

Focus is on companies that directly or indirectly benefit from the advancement of AI:
infrastructure (chips, memory, power), software (models, tooling, data), and deployment (cloud, edge, applications).

---

## Tier 1 — Current Holdings (Always Deep-Analyze Every Session)

Tier 1 is **determined dynamically at runtime** — it is whatever stocks the agent currently holds in the account. Do not assume any specific tickers here.

At the start of each session, use the available Robinhood MCP tool for fetching current equity positions (see [Robinhood's tool documentation](https://robinhood.com/us/en/support/articles/trading-with-your-agent/) for the current tool name) with `account_number='494502131'`. Any symbol with quantity > 0 is automatically Tier 1 for that session and receives full deep analysis regardless of how much or how little it moved.

---

## Tier 2 — Watchlist (All Screened Every Session — Sectors Treated Equally)

Every name below is screened each session with **equal priority**. The sector sub-groupings give AI-angle context and mirror the **Canonical Sector Map** further down, which is the authoritative mapping for risk limits. They do **NOT** imply any screening order. What advances to deep analysis is decided by **signal strength** (see Screening Method at the end of this file), never by sector or position in this list.

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

### Space & Satellite Connectivity
| Ticker | Company | AI Angle |
|---|---|---|
| SPCX | SpaceX (Class A) | Starlink connectivity for edge/distributed AI and remote data; AI-driven autonomous launch/landing. Indirect AI play — high growth, high volatility. |

---

## Canonical Sector Map (authoritative for concentration limits and sector-ETF rules)

Every symbol belongs to **exactly one** sector for risk purposes. Where a company could plausibly sit in two groups, the assignment below is the one that counts — use it for the 40% sector-concentration cap in `06_risk_management.md` and for the sector-ETF triggers in `00_overview.md` and `07_decision_framework.md`.

| Sector | Members | Sector ETF proxy |
|---|---|---|
| Memory & Storage | MU, SNDK, WDC, SKHY | **SMH** |
| Semiconductor Chips | NVDA, AMD, AVGO, ARM, MRVL, QCOM, INTC | **SMH** |
| Semiconductor Equipment | AMAT, LRCX, KLAC, ASML, SNPS, CDNS | **SMH** |
| Tech Mega-Cap | GOOGL, MSFT, META, AMZN, AAPL, TSLA | **QQQ** |
| AI Software & Applications | PLTR, ADBE, SNOW, DDOG, PANW, CRWD, NOW, CRM, PATH, AI | **XLK** |
| Cloud & GPU Infrastructure | ORCL, CRWV, NBIS, NET | **XLK** |
| AI Server Hardware | SMCI, DELL, HPE, VRT | **XLK** |
| Power & Energy | CEG, VST, NRG, ETR, GEV | **XLU** |
| Networking | ANET, CSCO | **XLK** |
| Space & Satellite | SPCX | *(none — no liquid proxy; sector-ETF rules do not apply)* |

**Notes:**
- **Proxies are sector-level approximations, chosen for correlation, not exact index membership.** For example, GEV is an industrial but trades with the power buildout, and VRT likewise with AI hardware. The one correction worth knowing: Tech Mega-Cap uses **QQQ**, not XLK, because four of its six names (AMZN, TSLA, META, GOOGL) are not in the technology sector ETF at all — all six are in the Nasdaq-100.
- **NVDA** counts once, under Semiconductor Chips — not under Tech Mega-Cap.
- **SMH covers three sectors** (Memory, Chips, Equipment). They are separate for the *concentration* cap but share one ETF trigger. Because they are highly correlated, also apply the combined check below.
- **Combined semiconductor exposure**: Memory + Chips + Equipment together may not exceed **50% of account value** (`06_risk_management.md`). They routinely move as one bloc, so treating them as three independent 40% buckets understates the real risk (this is how the account became ~70% memory/semis in July).
- **SPCX has no ETF proxy** — the sector-ETF screen and the sector-breakdown exit simply do not apply to it. Judge it on its own price action.

---

## Screening Method (sector-neutral)

Two tiers only:
- **Tier 1 — Holdings**: every symbol currently owned (`get_equity_positions`, quantity > 0). Always deep-analyzed each session — primarily to check exit conditions (stops, trend breaks).
- **Tier 2 — Watchlist**: every other name in this file, screened with **equal priority regardless of sector**.

Each session:
1. **Quick-screen ALL of Tier 2 first** (cheap — just pull % change vs. previous close and volume for each). Complete this across every sector *before* committing budget to any deep analysis, so no sector is skipped for being lower on a list.
2. **Flag by signal, not by sector** — a name is flagged only if it trips a quick-screen trigger (`00_overview.md`): >3% move vs. previous close, volume >2× average, RSI extreme, 50/200-day SMA cross, or a sector-ETF move. A flagged name then faces the eligibility gates (`07_decision_framework.md` Step 0), and only names that pass them receive deep analysis.
3. **When multiple names flag and budget is limited, prioritize by signal strength, not sector.** Do not spend the whole deep-analysis budget on one sector — if flagged names span several sectors, ensure representation across them before going deep on multiple names from the same sector.

> This replaces the old sector-ordered priority list, which biased attention toward whichever sectors appeared first (a contributor to over-concentration in memory/semis). Sector position in this file now carries **no** weight; only live signals do.
