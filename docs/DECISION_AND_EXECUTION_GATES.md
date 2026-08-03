# Decision and Execution Gates

Use these as hard stage gates. A later phase cannot begin until the preceding gate has evidence.

## Gate 1 — Event eligibility and mechanics

Confirm from the official DoraHacks page:

- event remains virtual;
- deadline and source timezone;
- participant remains eligible;
- required source, video, and transaction artifacts;
- finalist-pitch obligations;
- current prize and bounty mechanics.

Record unresolved KYC, IP, payout, team-size, and licensing questions without guessing.

## Gate 2 — Live platform truth

Refresh:

- KeeperHub MCP tool lists;
- `/api/chains` and chain statuses;
- action schemas;
- marketplace listings;
- public OpenAPI surfaces;
- docs/source drift relevant to the proposed integration.

Do not use an experimental or deprecated chain for a write unless the user explicitly accepts the risk.

## Gate 3 — User/job evidence

A candidate needs:

- a specific user;
- a recurring task or failure;
- evidence the problem exists;
- why an agent is useful rather than a fixed automation rule;
- why onchain execution is required;
- why KeeperHub is indispensable.

Generic “AI makes blockchain easier” claims do not pass.

## Gate 4 — Competitive differentiation

Compare the candidate against:

- current public DoraHacks signals;
- KeeperHub's marketplace;
- KeeperHub's previous winners;
- direct substitutes such as AgentKit, Chainlink Automation, Safe modules, relayers, and fixed workflows.

Describe collisions separately at the user-job, proposition, mechanism, primitive, integration, and presentation levels.

## Gate 5 — Buildability

Prove:

- required KeeperHub action exists;
- required network is stable;
- organization/account/key/wallet access is available legitimately;
- gas and test assets can be obtained;
- external APIs/contracts are public or licensed for the use;
- no critical allowlist, KYC, paid-plan, or deployment dependency blocks the demo;
- the core loop can be built in 1–2 days.

Separate coding risk from integration/access risk.

## Gate 6 — Project selection

Select exactly one candidate only after Gates 1–5. Document:

- user and painful job;
- trigger → reasoning → policy → KeeperHub action → verified outcome;
- differentiation;
- truth boundaries;
- irreversible actions and approvals;
- smallest judge-testable vertical slice;
- explicit reasons rejected alternatives lost.

## Gate 7 — First KeeperHub write

Before product expansion:

1. Use a dedicated hackathon organization.
2. Keep credentials outside source control.
3. Prefer Ethereum Sepolia or Base Sepolia.
4. Fund the correct organization EOA/Safe with minimal test assets.
5. Simulate the exact intended request.
6. Require non-reverting simulation.
7. Broadcast once with a unique idempotency key.
8. Persist the KeeperHub execution ID.
9. Poll according to the server's interval hint.
10. Independently verify receipt and intended state change.
11. Produce a redacted evidence record.

## Gate 8 — Product quality

Require:

- deterministic transaction policy;
- typed boundaries around all LLM output;
- address, chain, token, contract, function, and amount constraints;
- approval gates for meaningful value or irreversible rights;
- timeout and retry policy;
- safe ambiguous-state handling;
- observable submitted/confirmed/verified states;
- reproducible tests and setup;
- no fake production claims.

## Gate 9 — Submission readiness

The submission is not complete until it has:

- public GitHub repository and setup instructions;
- architecture and KeeperHub integration explanation;
- automated tests and actual test output;
- short demo video showing the real loop;
- public KeeperHub-routed transaction link;
- receipt and postcondition proof;
- limitations and safe-failure behavior;
- DoraHacks BUIDL submitted before the WAT deadline;
- a concise finalist pitch and backup demo evidence.
