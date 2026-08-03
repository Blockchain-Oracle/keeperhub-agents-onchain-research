# KeeperHub Agents Onchain Research

A source-grounded research and execution context package for the **KeeperHub Agents Onchain Hackathon** on DoraHacks.

This repository is deliberately a **research handoff, not a preselected project**. It gives a capable agent enough verified context, decision gates, safety rules, and source links to refresh the facts, investigate opportunities, select a defensible build, validate KeeperHub access, and carry the selected product through implementation and submission.

## Current event snapshot

- **Format:** Virtual
- **Eligibility:** Worldwide, solo or teams, age 18+, sanctions restrictions apply
- **Participant context used in the research:** Adult resident of Nigeria; remote-only
- **Submission deadline:** **13 August 2026, 11:00 WAT**
- **Required proof:** Public GitHub source, short demo video, and a transaction executed through KeeperHub
- **Prize pool:** USD 5,000, paid in stablecoins
- **Core requirement:** KeeperHub must be the application's onchain execution layer

Event facts can change. Always refresh the official sources before acting.

## Repository map

| File | Purpose |
|---|---|
| [`docs/CONTEXT_DOSSIER.md`](docs/CONTEXT_DOSSIER.md) | Complete event, technical, source, market, competitor, risk, and eligibility audit |
| [`prompts/MASTER_AGENT_PROMPT.md`](prompts/MASTER_AGENT_PROMPT.md) | Copy-paste prompt that tells an autonomous agent how to work through the hackathon end to end |
| [`docs/DECISION_AND_EXECUTION_GATES.md`](docs/DECISION_AND_EXECUTION_GATES.md) | Compact stage gates that prevent premature ideation and unsafe writes |
| [`docs/OPEN_QUESTIONS.md`](docs/OPEN_QUESTIONS.md) | Unresolved organizer and access questions |
| [`AGENTS.md`](AGENTS.md) | Repository-level operating instructions for coding/research agents |

## Recommended reading order

1. Read this README.
2. Read the entire context dossier.
3. Read the decision and execution gates.
4. Use the master agent prompt.
5. Refresh all time-sensitive facts against official sources.
6. Do not select a project until user/job evidence, field differentiation, and KeeperHub access feasibility have been demonstrated.

## Verified technical baseline

The research safely exercised KeeperHub's public interfaces and found:

- 442 action schemas;
- 35 action categories;
- 6 trigger types;
- 22 live chain records;
- 35 authenticated MCP tools in source;
- 9 anonymous public MCP tools;
- 95 marketplace workflows at the audit snapshot;
- an official Hermes plugin that is read-only by default and whose audited suite passed 11/11 tests.

No authenticated wallet, key, workflow, or transaction was created during the research. The eventual build must perform its own legitimate testnet access spike.

## Safety baseline

Never:

- commit API keys, OAuth tokens, wallet HMAC secrets, webhook URLs, or private keys;
- let an LLM freely invent a chain, contract, function selector, recipient, token, or amount;
- broadcast before simulation and deterministic policy validation;
- retry an ambiguous write with a fresh idempotency key;
- call a submitted transaction “successful” before receipt and postcondition verification;
- describe a mock, manual transaction, or direct signer as KeeperHub execution;
- assume every indexed DoraHacks page is formally submitted to this event;
- assume gas sponsorship or prize/KYC rules beyond what the official sources state.

## Research timestamp and limitations

The dossier was assembled on **3 August 2026**. Hosted capabilities, marketplace counts, repository commits, docs, event entries, and deadlines are time-sensitive.

This repository intentionally contains no license grant. Source repositories referenced in the dossier retain their own licenses; inspect each before copying code.
