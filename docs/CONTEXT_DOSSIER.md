# KeeperHub Agents Onchain Hackathon — Context Engineering Dossier

**Research timestamp:** 2026-08-03 05:49–08:04 UTC  
**Participant context:** Adult resident of Nigeria; remote participation only  
**Status:** Research context, not a project commitment or implementation plan

## 0. Truth hierarchy

Resolve conflicts in this order:

1. Official DoraHacks event page for event rules, dates, prizes, and submission requirements.
2. Live KeeperHub API/MCP responses for currently executable hosted capabilities.
3. Current official KeeperHub documentation for intended use and public interfaces.
4. Current official KeeperHub source repositories for implementation and security behavior.
5. Public BUIDL pages, marketplace listings, and search indexes for field calibration only.

Do not treat marketing copy, stale README examples, or a successful HTTP response as proof of a settled transaction. A chain explorer receipt is the final proof of an onchain write.

---

## 1. Event truth

**Event:** KeeperHub — Agents Onchain Hackathon  
**Organizer:** KeeperHub  
**Platform:** DoraHacks  
**Format:** Virtual  
**Prize pool:** USD 5,000, paid in stablecoins

### Eligibility

Official page says:

- worldwide;
- solo or teams;
- age 18+;
- regions subject to applicable sanctions, including OFAC-restricted jurisdictions, are excluded;
- every entry must ship a working agent that executes through KeeperHub.

**Nigeria finding:** An adult resident of Nigeria appears eligible. Nigeria is not named as an excluded country on the event page. This is subject to the sanctions clause and any participant-specific screening. No physical attendance requirement is published.

**Not published or not verified:** team-size cap, KYC process, payout-wallet requirements, tax handling, event-specific IP assignment, or a requirement to form a company. Do not infer these.

### Timeline and WAT conversion

The event explicitly states all schedule times are UTC+2. WAT is UTC+1.

| Official milestone | Official time | WAT / Africa-Lagos |
|---|---:|---:|
| Build phase opens | 27 Jul 2026, 12:00 UTC+2 | **27 Jul 2026, 11:00 WAT** |
| Submission deadline | 13 Aug 2026, 12:00 UTC+2 | **13 Aug 2026, 11:00 WAT** |
| Judging | 13–20 Aug | one hour earlier than published times if times are later supplied |
| Finalist pitches | 17–19 Aug | exact slots not yet supplied |
| Winners announced | 20 Aug | exact time not supplied |

At 2026-08-03 05:48:58 UTC, the submission deadline was **10 days, 4 hours, 11 minutes** away.

DoraHacks' timeline card displays 10:00, but the detailed event text explicitly says 12:00 UTC+2. These represent the same instant: 10:00 UTC / 11:00 WAT. The card's separate `2026/07/27 05:01` submission-opening timestamp conflicts with the narrative's 12:00 UTC+2 opening by 4 hours 59 minutes; this does not affect the closing deadline.

### What entrants must build

Only one universal technical requirement is stated:

> KeeperHub must be the project's onchain execution layer.

The agent framework is unrestricted. The event names ElizaOS, OpenClaw, Hermes, CrewAI, LangChain, AutoGPT, and custom frameworks as examples.

The organizer explicitly prefers a real KeeperHub-routed transaction over a polished non-executing demo.

### Mandatory submission package

Before the deadline, submit a DoraHacks BUIDL containing:

1. a **GitHub source-code link**;
2. a **short demo video** showing the agent executing onchain through KeeperHub;
3. a **link to a transaction** the agent executed via KeeperHub.

The page header generically says GitHub/GitLab/Bitbucket is required, but the detailed submission section specifically says GitHub. Use GitHub to eliminate ambiguity.

Ten shortlisted teams must present live to judges during 17–19 August. The hackathon is virtual, but exact joining details and pitch duration are only promised to finalists. Availability during that window is therefore a practical requirement.

### Prizes

- First: USD 2,000
- Second: USD 1,200
- Third: USD 800
- Two stackable Best Onboarding UX Improvement bounty winners split USD 1,000

The onboarding bounty can reward a merged KeeperHub PR, starter template, tutorial, or evidence-backed teardown with proposed fixes. A bounty can be combined with a top-three award.

**Platform inconsistency:** DoraHacks' Bounties tab rendered no standalone bounty records during the audit even though the authoritative event detail page explicitly promises the USD 1,000 onboarding bounty. Preserve evidence from the detail page and ask KeeperHub how bounty submissions should be flagged.

### Judging

No numerical weights are published. The event says execution is weighted heavily and evaluates:

1. actual onchain execution through KeeperHub;
2. use of KeeperHub surfaces: MCP, CLI, x402, MPP, workflow builder, and audit trail;
3. reliability and observability, including failure handling, retries, gas, and audit evidence;
4. originality and real-world usefulness;
5. integration quality and developer experience.

Review is two-stage: KeeperHub reviews all entries, then ten finalists pitch live. Final ranking combines the scored review and pitch.

---

## 2. Correct KeeperHub mental model

KeeperHub is not the reasoning agent. It is the hosted/open-source workflow, signing, execution, payment, and observability layer between an agent's decision and an onchain result.

A credible application loop is:

```text
real trigger or user intent
  -> grounded observation
  -> agent recommendation/decision
  -> deterministic policy gate
  -> simulation or bounded preflight
  -> explicit approval where appropriate
  -> KeeperHub execution
  -> terminal execution status
  -> independent chain receipt/state verification
  -> visible audit record and safe failure path
```

KeeperHub must be causal in the final state transition. Merely reading KeeperHub documentation, displaying a workflow, or sending an already-broadcast transaction hash is not sufficient.

---

## 3. Hosted integration surfaces

### A. Authenticated MCP

- Endpoint: `https://app.keeperhub.com/mcp`
- Transport: Streamable HTTP MCP
- Live initialize response: server name `keeperhub`, version `1.2.0`, authentication required
- Interactive authentication: OAuth through the MCP client/browser
- Headless authentication: `Authorization: Bearer <organization-api-key>`
- OAuth scopes in source: `mcp:read`, `mcp:write`, `mcp:admin`
- Organization `kh_` keys are effectively full-access credentials rather than fine-grained OAuth grants; use a dedicated hackathon organization/key and rotate it after the event.

The current source registers **35 authenticated tools**:

- workflow: list/get/create/update/delete/execute/validate;
- execution: get one combined execution record;
- project/tag: list and create;
- AI generation and test-data preparation;
- action/plugin/integration/wallet discovery;
- template search/get/deploy;
- direct transfer, contract call, check-and-execute, and direct-status polling;
- protocol action search and execution;
- marketplace search/call/list/unlist/update/get-listing;
- tool documentation.

Mutating tools accept optional idempotency keys where supported. OAuth scope denial responses expose the required scope.

### A2. Official Hermes integration

KeeperHub publishes an Apache-2.0 Hermes plugin at `KeeperHub/hermes-plugin` / `keeperhub-hermes-plugin`:

- it uses the hosted KeeperHub MCP API;
- it requires a `kh_` organization key;
- it registers read-only `kh_*` tools by default;
- write and execution tools are structurally absent unless `KEEPERHUB_ENABLE_WRITES=true` is set;
- the audited test suite passed **11/11 tests** under Python 3.11 after installing its declared development dependencies.

For a Hermes-based entry, this is safer than recreating an unscoped wrapper. Product-specific policy and approval gates are still required after writes are enabled.

### B. Anonymous public MCP

- Endpoint: `https://app.keeperhub.com/mcp/public`
- Live capability check succeeded without authentication.
- Source uses a deny-by-default allowlist.

Live tools:

1. `list_action_schemas`
2. deprecated `search_plugins`
3. `get_plugin`
4. `search_templates`
5. `tools_documentation`
6. `search_protocol_actions`
7. `search_workflows`
8. `call_workflow`
9. `get_workflow_listing`

`call_workflow` does not auto-pay. A paid call returns HTTP 402 and requires a payment-capable wallet/client.

### C. REST Direct Execution API

Authenticated endpoints:

- `POST /api/execute/transfer`
- `POST /api/execute/contract-call`
- `POST /api/execute/check-and-execute`
- `GET /api/execute/{executionId}/status`

Required auth: organization `kh_` key. Direct execution limit: 60/minute/key. Authenticated general API rate: 100/minute; unauthenticated: 10/minute.

**Safe write sequence:** choose an enabled testnet -> submit the exact request with strict boolean `simulate: true` -> continue only if successful and non-reverting -> remove simulation and submit once with a unique `Idempotency-Key` -> persist execution ID -> poll according to `X-Poll-Interval-Hint` -> require a transaction hash and independently verify receipt/state.

Idempotency is organization-scoped and retained for 24 hours. Same key/body replays the original result; a changed body conflicts.

Two machine-readable API descriptions are public: `/api/openapi` covers management/direct-execution operations, while `/openapi.json` describes public marketplace/workflow calls for agent payment clients.

Known simulation limitation: when an organization routes writes through a Safe, simulation currently uses the organization's EOA as `from`, so `msg.sender`-dependent behavior may differ from broadcast through the Safe.

### D. Workflow engine

Live public schema probe returned:

- **442 actions**;
- **35 action categories**;
- **6 triggers:** Manual, Schedule, Webhook, Event, Block, Transfer;
- **22 live chain records**.

Large protocol surfaces already exist for Ajna, Sky, Morpho, Chainlink, Yearn, Ethena, Spark, Aerodrome, Aave V3/V4, Compound, CoW Swap, Curve, Lido, Pendle, Safe, Uniswap, Superfluid, Web3, and others. Generic protocol monitoring/rebalancing is therefore highly saturated unless the application contributes a new mechanism or a clearly underserved user job.

### E. Marketplace and agent payments

Listed workflows expose a typed input/output interface while hiding the internal graph. Read listings execute and return a result; write listings return unsigned `{to, data, value}` calldata for the caller to submit.

Marketplace settlement:

- x402: Base mainnet, USDC, gas submitted by facilitator;
- MPP: Tempo, USDC.e, facilitator pays network fee;
- creators receive 70%; KeeperHub receives 30%;
- successful paid calls only; failed calls are not charged;
- listings priced at least USD 0.05 can exempt paid calls from the creator's execution quota.

Live marketplace snapshot:

- 95 listings returned;
- 77 free, 18 paid;
- 92 read, 3 write;
- 50 had been listed since the hackathon opened.

This is a crowded discovery surface. A project should not be just another typed balance checker, risk score, swap quote, wallet monitor, or generic transaction executor.

### F. Wallets are separate concepts

1. **Organization/creator wallet:** Turnkey-backed wallet attached to the KeeperHub organization, used for workflow/direct writes and receiving creator revenue. It needs native gas for ordinary broadcasts unless a specific sponsorship path applies. If a Safe is the active sender, the EOA still pays gas while transactable assets live in the Safe.
2. **Agentic payment wallet:** separate zero-registration Turnkey sub-organization used to pay x402/MPP marketplace calls. It does not provide a general transaction signer.

First-party payment-wallet hard limits documented by KeeperHub:

- Base USDC and Tempo USDC.e contracts only;
- maximum 100 USDC per transfer/approval;
- Base/Tempo chain allowlist;
- default 200 USDC aggregate daily cap;
- client auto/ask/block hook plus server-side Turnkey policies;
- HMAC credential exists on disk even though the private key does not;
- current loss of `wallet.json` is unrecoverable and stranded funds cannot be recovered self-service.

Do not conflate paying for a workflow with funding the organization wallet that executes its internal writes.

### G. Published plan limits

KeeperHub's official `pricing-ui` data currently advertises a free/pay-per-execution tier with:

- 5,000 included executions per month;
- USD 1/month sponsored-gas credit;
- USD 0.01 per extra execution;
- all EVM chains plus Solana;
- rate-limited API;
- seven-day logs;
- MCP included.

These are published plan claims, not a substitute for verifying the actual newly created hackathon organization's entitlements. The event's broad “gas sponsorship on mainnet Ethereum” wording should not be interpreted as unlimited sponsorship; the pricing data exposes finite monthly gas credits.

---

## 4. Chains and funding

### Hackathon quickstart's conservative matrix

Recommended stable testnets:

| Chain | ID | USDC |
|---|---:|---|
| Ethereum Sepolia | 11155111 | `0x1c7D4B196Cb0C7B01d743Fbc6116a902379C7238` |
| Base Sepolia | 84532 | `0x036CbD53842c5426634e7929541eC2318f3dCF7e` |

Stable mainnets named there: Ethereum, Base, Arbitrum One, Optimism, Polygon. Experimental: 0G and 0G Galileo.

### Live-chain drift

The live `list_action_schemas` response currently marks 20 chains stable and 0G/0G Galileo experimental, including additional EVM networks, Solana, Plasma, Tempo, and testnets omitted from the pasted quickstart. The live API is broader than the quickstart.

**Decision rule:** use Ethereum Sepolia or Base Sepolia for the first judge-reproducible write because those are explicitly recommended by the event quickstart. Treat other live chains as available but requiring an explicit capability spike.

### Faucets/access

- Circle's testnet stablecoin faucet is public and permissionless, no account required, with 20 USDC per address per blockchain every two hours.
- Google Sepolia ETH faucet may require a Google account and may apply anti-abuse/mainnet-balance checks.
- Coinbase Base faucet access was not verified through static extraction.
- The organization wallet must be provisioned and funded with native gas before a standard write.

Do not depend on a single faucet during the final demo. Fund and verify the demo wallet early.

---

## 5. Security and truth boundaries

### Required application controls

- deterministic transaction policy outside the LLM;
- chain, target-contract, function-selector, recipient, token, and amount allowlists;
- strict typed parsing and decimal handling;
- testnet by default;
- simulation before broadcast;
- unique idempotency key per intent;
- explicit human confirmation for irreversible/value-moving operations unless the autonomy case is tightly bounded;
- timeout, retry with jitter, and terminal-state polling;
- do not retry ambiguous writes with a new idempotency key;
- independent RPC/explorer receipt and postcondition verification;
- redact `kh_`, `wfb_`, OAuth tokens, wallet HMAC secrets, webhook URLs, and third-party credentials from UI/logs/video/repository;
- distinguish `submitted`, `confirmed`, and `postcondition verified` states.

### Code action sandbox nuance

The docs accurately warn that `node:vm` is not a security boundary against determined attackers. Current source enforces a remote sandbox backend in production, while local/self-hosted fallback launches user code in a child Node process. SSRF filtering blocks private/reserved address ranges and non-HTTP schemes.

Therefore, do not describe self-hosted `node:vm` execution as safe for hostile multitenant code. Avoid accepting arbitrary public JavaScript in the hackathon app.

### MCP/tool safety nuance

The current implementation supplies read/write annotations and source-level OAuth scope gates. However, a tool annotation is agent guidance, not authorization. `call_workflow` can trigger internal workflows and paid calls involve an external wallet. Keep application-level approval and policy enforcement.

---

## 6. Source and version audit

Pinned on 2026-08-03 after the background source audit completed:

| Repository | Commit | License observed | Notes |
|---|---|---|---|
| `KeeperHub/keeperhub` | `70088b12ce65b9041203aab793789e6e143514be` | Apache-2.0 | hosted app/core, default branch `staging`; repository moved during the audit |
| `KeeperHub/cli` | `c78845283b9de48098e4b25797b1b54e56e66041` | MIT | Go 1.25; local audit image lacked Go |
| `KeeperHub/agentic-wallet` | `c517623f2cd0ca5cab58a4411116acb00e1825a1` | Apache-2.0 | payment wallet package |
| `KeeperHub/agentic-wallet-skills` | `de4243499a4422e8e9376d0c4885600765cda8b6` | no repository license found | distributed wallet skill; licensing must not be assumed |
| `KeeperHub/claude-plugins` | `d3ba8901f32ae355fcfa95c92529b5f0d275cf28` | MIT | several skills lag current MCP behavior |
| `KeeperHub/sdk` | `21b72bca94d26b90c4753c0e1a5ed3d7648f6e3d` | Apache-2.0 | TypeScript REST SDK v0.x; API still stabilizing |
| `KeeperHub/mcp` | `a98db00098273119b7dda710c6d97d510818d58d` | Apache-2.0 | TypeScript and Python MCP client kernels, v0.1.0 status |
| `KeeperHub/hermes-plugin` | `27a1bfb9cbd3230641e68123e5b51cb6b16f4e7d` | Apache-2.0 | official Hermes adapter; read-only by default |
| `KeeperHub/eve-plugin` | `3ea3380b5951b262939fe1c1cf4adc2aa58156f5` | Apache-2.0 | Eve adapter; writes opt-in with first-write approval default |
| `KeeperHub/pricing-ui` | `d4a29f033a39defb2f0ce6179f01cb12be3d6dc0` | UNLICENSED | public pricing data is inspectable but not reusable as open-source code |
| `KeeperHub/homebrew-tap` | `0cafb165852c3a17c51ca26abfdb3025f1d11d13` | no repository license found | CLI distribution metadata |

Important drift:

- current app/docs use Turnkey while portions of the core README still mention Para;
- the Claude plugin's execution skill still calls obsolete `get_execution_status` and `get_execution_logs`; current MCP exposes combined `get_execution`;
- plugin skill examples describe a workflow shape that differs from current MCP's nodes/edges schema;
- the live source registers 35 authenticated tools, more than some docs summaries;
- the quickstart chain table is narrower than the live chain list;
- the quickstart page supplied by the user was not discoverable at `/hackathon-quickstart` during the audit, so preserve the supplied text but use live API data for current support.
- the rendered documentation navigation exposed 171 routes while `llms.txt` listed only 46; `sitemap.xml` and `robots.txt` returned 404, so `llms.txt` is useful but not a complete inventory.

These are real onboarding friction points and possible bounty evidence, not permission to claim a bug without a reproducible issue/PR.

### Verification limits

- Public MCP initialization, tool listing, action schema discovery, chains, OpenAPI, and marketplace discovery were exercised successfully.
- No authenticated write was attempted because no legitimate hackathon account/key/wallet was supplied in this session.
- The official Hermes plugin's declared test suite passed **11 tests in 0.12 seconds** after its development dependencies were installed.
- Agentic-wallet dependencies installed under pnpm 10, but the test runner failed before collecting tests with an environment-level `ENOSPC`; this did not establish either passing or failing assertions. Generated dependencies were removed afterward.
- CLI tests were not run because the local audit image lacks Go 1.25.

---

## 7. Competitive field calibration

The official event BUIDL tab currently renders “No BUIDLs yet,” but web search already indexes many public DoraHacks project pages explicitly describing the KeeperHub hackathon. These may not all be formally attached/submitted, so they are **competitive signals, not an authoritative submission count**.

Already crowded themes include:

- generic “agent decides, KeeperHub executes” wrappers;
- wallet guardians and treasury sweepers;
- Aave health-factor monitoring/rebalancing;
- natural-language payments;
- transaction simulation/policy/audit consoles;
- approval revocation;
- zero-value proof registries;
- x402 pay-per-action services;
- execution monitors and starter kits;
- inheritance/dead-man switches;
- escrow release and outcome-verification rails.

Public examples include KeeperHub Agent, KeeperHub Agents Onchain, KeeperSense, KeeperGuard, AgentPay, RebalanceKeeper, VaultMind, KeeperHub Sentinel, KeeperPilot, ApprovalSentinel, Vigil, Recourse, ProofPulse, DeadHand, Kajota × KeeperHub, and others.

**Consequence:** do not choose a generic guardian, payment agent, Aave rebalancer, starter dashboard, or “safe executor” without a materially different mechanism, user, integration, and independently demonstrated outcome.

Marketplace saturation and hackathon saturation are separate: a workflow can be common in the marketplace but still support a novel application, while a novel marketplace slug does not make a duplicated product original.

### Adjacent infrastructure calibration

KeeperHub also competes or overlaps with established mechanisms:

| Adjacent surface | What it already covers | What a KeeperHub build must show beyond it |
|---|---|---|
| Coinbase AgentKit | framework-agnostic wallet/action providers, transfers, swaps, arbitrary contract calls across EVM/Solana | why KeeperHub's hosted workflow reliability, observability, marketplace, or payment routing is indispensable |
| Chainlink Automation | time, log, and custom-logic upkeeps with decentralized executors | why off-chain agent reasoning, richer integrations, or KeeperHub UX is needed rather than a normal upkeep |
| Safe Modules/Guards | programmable account authorization, transaction pre/post checks, automation | why execution is safely bounded without introducing a high-trust arbitrary-execution module |
| Gelato-style automation | historical off-chain TypeScript/HTTP logic plus automated transactions; KeeperHub itself markets migration from discontinued Gelato Web3 Functions | avoid presenting generic schedule/event-to-contract execution as novel |
| Olas/autonomous-agent stacks | deployable onchain agent services and agent economies | identify a user-facing outcome rather than another generic agent framework |
| OpenZeppelin Defender/relayer-style operations | monitored, policy-controlled transaction operations and automation | make the agent-specific decision loop and KeeperHub surface demonstrably better for the selected job |

The competitive claim should therefore be mechanism-specific: not “AI can transact,” but why this particular user, trigger, evidence, policy, settlement, and recovery loop is materially better than an automation rule, smart-account module, or direct AgentKit action.

### KeeperHub's previous-hackathon precedent

KeeperHub's official postmortem for ETHGlobal Open Agents reviewed **180 submissions** and provides unusually useful judge calibration:

- 52 used MCP, 40 used x402, roughly 17 used direct HTTP, roughly 25 used webhooks, and about 30 were shallow surface integrations;
- KeeperHub explicitly said the MCP/x402 cohort pointed toward the future, while one-workflow integrations with no meaningful execution loop were weak;
- the team only filled three main prize slots because the remaining work did not meet its “merge, adopt, or build on directly” bar;
- bug reports and specific onboarding findings were valued because they proved builders had exercised the real platform.

Previous winners already occupy broad territories:

1. **Tradewise Agentlab:** x402-priced Uniswap agent, KeeperHub monitoring/reputation/compliance workflows, transferable agent identity, reputation and revenue sharing; distinguished by a live deployment, 125 tests, and reproducible platform bug reports.
2. **Keeper-Gate:** framework-agnostic KeeperHub SDK with LangChain, ElizaOS and OpenClaw adapters; distinguished by a reusable core and thin per-framework adapters.
3. **ZW.ARM:** three-agent mainnet yield rotation across Aave, Compound and Morpho, with an independent critique agent and per-subscriber KeeperHub-managed wallets.

Implication: framework adapters, paid swap agents, generic multi-agent critique, yield rotation, and per-user managed wallets are not untouched ideas. A new entry must progress beyond these precedents rather than merely reproduce them on a different frontend.

---

## 8. Risk register

### Integration/access risk

- account creation, email verification, organization wallet provisioning, and `kh_` key creation not yet exercised;
- native gas and test USDC must reach the correct organization/Safe sender;
- OAuth/browser flow unsuitable for CI unless switched to an organization key;
- experimental chains may hang on broadcasts;
- faucet anti-abuse can block a new wallet;
- gas sponsorship wording is broader on the event page than standard wallet docs; do not assume every transaction is sponsored;
- marketplace write listings return unsigned calldata rather than KeeperHub broadcasting for the caller;
- x402 payments use mainnet-value USDC even if the internal workflow demonstrates a testnet action;
- finalist live-pitch timing is unknown;
- KYC/payout screening is not published.

### Coding risk

- LLM-generated amounts/addresses/calldata;
- token decimal mistakes;
- unsafe duplicate retry behavior;
- workflow schema/version drift;
- wrong sender during Safe simulation;
- event/RPC/indexer latency;
- logging secrets or personal data;
- conflating transaction submission with confirmation/outcome;
- building a wide agent platform instead of one demonstrable loop.

### Differentiation risk

The field is already saturated around reliability, guardians, payments, DeFi rebalancing, x402 monetization, and onboarding. Differentiation must be supported by field evidence, not branding.

---

## 9. Project-selection gates — do not select before these pass

A candidate should only advance if all are true:

1. **User/job evidence:** a specific user has a recurring onchain task with a costly current failure.
2. **KeeperHub causality:** KeeperHub performs the indispensable write, settlement, or workflow transition.
3. **Access spike:** account, key, wallet, gas, network, action, and transaction proof are reproducibly usable.
4. **Originality check:** not merely one of the crowded themes above.
5. **Bounded loop:** one trigger, one grounded decision, one policy, one KeeperHub action, one verifiable result.
6. **Safety:** deterministic limits and explicit approval where value or irreversible rights move.
7. **Judge reproducibility:** clean repository, setup script, testnet seed path, short demo, public proof, and truthful failure mode.
8. **Time fit:** core vertical slice in 1–2 days, reliability/UX next, submission assets before deadline.
9. **No fake economics:** no invented users, revenue, mainnet volume, or “autonomy” that is actually a hardcoded script.

---

## 10. Mandatory first spikes after project selection

1. Create a dedicated KeeperHub hackathon organization.
2. Verify email and organization wallet provisioning.
3. Create a least-exposed `kh_` key; store only in environment/secret manager.
4. Connect authenticated MCP and record the exact live tools.
5. Fund the org wallet with small testnet gas; verify balance independently.
6. Simulate a zero/near-zero-risk Sepolia or Base Sepolia write.
7. Broadcast with a unique idempotency key.
8. Poll to terminal state and independently verify receipt and state change.
9. Export a redacted evidence bundle.
10. Only then implement the product-specific loop.

---

## 11. Agent-ready operating context

```text
Mission: Build one original, useful AI-agent application for KeeperHub's virtual
Agents Onchain Hackathon. The product must execute a real transaction through
KeeperHub; KeeperHub must be foundational to the final state transition.

Participant: Adult resident of Nigeria. Remote-only. Appears eligible under the
official worldwide/18+/sanctions rule. Deadline: 13 Aug 2026, 11:00 WAT.

Required submission: public GitHub source, short demo video showing execution,
and a public transaction link. Ten finalists pitch live 17–19 Aug.

Truth order: DoraHacks event rules > live KeeperHub API/MCP > current KeeperHub
docs > pinned official source > public projects/search.

Default technical path: Ethereum Sepolia or Base Sepolia; Python/TypeScript agent
is acceptable; authenticated hosted MCP or Direct Execution API; strict simulate
-> policy/approval -> idempotent broadcast -> status poll -> independent receipt
and postcondition verification.

Never: mock the transaction; expose credentials; let the LLM invent raw transfer
parameters; treat submitted as confirmed; assume sponsorship; rely on an
experimental chain; retry an ambiguous write with a new idempotency key; claim
all public BUIDLs are formally submitted; select a generic guardian/payment/
Aave-rebalancer/onboarding-dashboard without a materially new mechanism.

Success evidence: KeeperHub execution ID, tx hash/explorer URL, confirmed receipt,
verified state transition, redacted audit trail, reproducible setup, and visible
safe-failure behavior.
```

---

## 12. Open questions for official support

Ask in KeeperHub Discord before architecture freeze:

1. Are Nigerian winners subject to any prize-provider/KYC restriction beyond the published sanctions clause?
2. Is there any team-size maximum or event-specific IP/licensing requirement?
3. Are finalist pitches fully remote, and what time range should finalists reserve on 17–19 August?
4. Which exact writes receive “mainnet Ethereum gas sponsorship,” and does that apply to hackathon accounts automatically?
5. Is a successful transaction produced via REST Direct Execution equally valid as one produced through the authenticated MCP tool wrapper?
6. Which live chains are officially accepted for judging when the quickstart and live `list_action_schemas` differ?
7. Is there any hackathon plan/credit, and what free-plan workflow and execution limits apply?
8. Does the onboarding bounty require a merged PR by the deadline, or can an open reviewed PR qualify?
9. Can a pre-existing codebase enter if the KeeperHub integration and submitted work are new during the build window?
10. What stablecoin network is used for prize payout?

---

## 13. Primary sources

- Event: https://dorahacks.io/hackathon/agents-onchain/detail
- Event field: https://dorahacks.io/hackathon/agents-onchain/buidl
- Docs index: https://docs.keeperhub.com/llms.txt
- MCP: https://docs.keeperhub.com/ai-tools/mcp-server
- Agent wallet: https://docs.keeperhub.com/ai-tools/agentic-wallet
- Direct execution: https://docs.keeperhub.com/api/direct-execution
- API keys: https://docs.keeperhub.com/api/api-keys
- Chains: https://docs.keeperhub.com/api/chains and https://app.keeperhub.com/api/chains
- Public schemas: https://app.keeperhub.com/api/mcp/schemas
- Public OpenAPI: https://app.keeperhub.com/api/openapi
- Marketplace catalog: https://app.keeperhub.com/api/mcp/workflows
- Marketplace model: https://docs.keeperhub.com/workflows/marketplace
- Core source: https://github.com/KeeperHub/keeperhub
- CLI: https://github.com/KeeperHub/cli
- Agentic wallet: https://github.com/KeeperHub/agentic-wallet
- Claude plugin: https://github.com/KeeperHub/claude-plugins
- TypeScript REST SDK: https://github.com/KeeperHub/sdk
- TypeScript/Python MCP client kernels: https://github.com/KeeperHub/mcp
- Official Hermes plugin: https://github.com/KeeperHub/hermes-plugin
- Previous hackathon postmortem: https://keeperhub.com/blog/010-openagents-hackathon-wrap
- Circle faucet: https://faucet.circle.com
