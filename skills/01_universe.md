---
file: 01_universe.md
purpose: Stock universe — which stocks to scan, in what order, and why each is an AI play
---

# Stock Universe — AI Beneficiary Watchlist

Focus is on companies that directly or indirectly benefit from the advancement of AI:
infrastructure (chips, memory, power), software (models, tooling, data), and deployment (cloud, edge, applications).

---

## Tier 1 — Current Holdings (Always Deep-Analyze Every Session)

Tier 1 is **determined dynamically at runtime** — it is whatever stocks the agent currently holds in the account. Do not assume any specific tickers here.

At the start of each session, use the available Robinhood MCP tool for fetching current equity positions (see [Robinhood's tool documentation](https://robinhood.com/us/en/support/articles/trading-with-your-agent/) for the current tool name) with `account_number='494502131'`. Any symbol with quantity > 0 is automatically Tier 1 for that session and receives full deep analysis regardless of how much or how little it moved.

---

## Tier 2 — Core Watchlist (Quick Screen Every Session; Deep Analysis on Signal or >3% Move)

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
| NVDA | (see above) | |
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
| PLTR | (see above) | |
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

## Tier 3 — Extended Watchlist (Scan Weekly or on Specific Catalysts)

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
| SPCX | SpaceX (Class A) | Starlink connectivity for edge/distributed AI and remote data; AI-driven autonomous launch/landing. Indirect AI play — high growth, high volatility; scan on catalyst. |

---

## Screening Priority Order

Each session, screen in this order:
1. Tier 1 (always): whatever symbols are returned by `get_equity_positions` with quantity > 0
2. Tier 2 — Memory & Storage: MU, SNDK, WDC, SKHY
3. Tier 2 — Chips & Equipment: NVDA, AMD, AVGO, ARM, AMAT, LRCX, KLAC, ASML, MRVL
4. Tier 2 — Mega-cap: GOOGL, MSFT, META, AMZN, AAPL, TSLA
5. Tier 2 — Software/Cloud: PLTR, SNOW, DDOG, PANW, CRWD, NOW, CRM, ORCL, CRWV, NET
6. Tier 2 — Hardware: SMCI, DELL, VRT
7. Tier 3: only on specific catalyst or weekly cycle (Power & Energy incl. GEV; Space & Connectivity incl. SPCX)
