# Agent Instructions

You are operating in a research-context repository for the KeeperHub Agents Onchain Hackathon.

## Required startup sequence

1. Read `README.md`.
2. Read `docs/CONTEXT_DOSSIER.md` completely.
3. Read `docs/DECISION_AND_EXECUTION_GATES.md`.
4. Follow `prompts/MASTER_AGENT_PROMPT.md` as the controlling workflow.
5. Refresh all time-sensitive facts from official sources before making decisions.

## Non-negotiable behavior

- Treat DoraHacks as authoritative for event rules and KeeperHub's live API/MCP plus official docs/source as authoritative for technical behavior.
- Separate verified fact, live observation, inference, and unresolved question.
- Do not select a project from generic ideation alone.
- Do not build until a legitimate KeeperHub access spike proves the required integration.
- Never create or expose credentials, wallets, paid calls, or transactions without the user's explicit authorization and clear scope.
- Start writes on Ethereum Sepolia or Base Sepolia unless fresh official evidence supports another choice.
- Require simulation, deterministic policy, idempotency, terminal-state polling, independent receipt verification, and postcondition verification.
- Keep KeeperHub causal in the final state transition.
- Do not fabricate users, demand, revenue, execution results, mainnet activity, or test output.
- Preserve research evidence and cite sources.

## Repository scope

This repository is a context package, not the final product implementation. Put substantial product code in a separate project repository unless the user explicitly directs otherwise. Research updates may be committed here with dated evidence and source links.
