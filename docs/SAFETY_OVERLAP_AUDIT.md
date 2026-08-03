# KeeperHub Safety-Control Overlap Audit

**Checked:** 2026-08-03
**Question:** Would a generic “Agent Harness” based on spending limits, address/contract allowlists, approvals, and transaction safety add a missing capability?

## Conclusion

**Mostly no.** KeeperHub already has substantial versions of those controls across several layers. A generic harness whose main proposition is “limit what the agent can spend and where it can send” would duplicate the platform and likely look shallow in this hackathon.

The controls are fragmented and have important boundaries, but the correct opportunity is not to recreate them. A stronger application should use them and add a specific decision, outcome-assurance, or recovery loop KeeperHub does not already provide.

## Existing controls

| Surface | Existing control | Enforcement | Important boundary |
|---|---|---|---|
| Agentic payment wallet | Client-side auto / ask / block thresholds | `PreToolUse` hook | Applies to payment-shaped wallet calls, not every organization-wallet transaction |
| Agentic payment wallet | Contract allowlist | Client hook plus Turnkey policy | Hard default is Base USDC and Tempo USDC.e; users can narrow but not self-serve widen it |
| Agentic payment wallet | Per-transfer and approval caps | Turnkey server-side policy | 100 USDC maximum |
| Agentic payment wallet | Daily aggregate cap | Turnkey/server-side ledger | 200 USDC per UTC day by default |
| Agentic payment wallet | Chain allowlist | Turnkey policy | Base, Tempo mainnet, and Tempo testnet for the documented signing path |
| Direct Execution and workflows | Organization daily cap | KeeperHub database reservation/ledger | Current source caps **native value** in wei/lamports; token-only calls reserve zero native value |
| Direct Execution | Simulation | RPC estimate/call without signing | Safe-routed simulation uses the EOA as `from`, so `msg.sender` behavior may differ |
| Direct Execution/webhooks | Idempotency | Organization/workflow-scoped records | Replay window is 24 hours; ambiguous writes still require reconciliation |
| Safe + Zodiac Roles | Protocol, contract, function and argument scoping | Onchain Zodiac Roles Modifier | Per-parameter presets are tighter than contract-only allowlists |
| Safe + Zodiac Roles | Per-token allowance buckets | Onchain allowances | Buckets can be separated by protocol, recipient, spender and token |
| Safe + Zodiac Roles | Direct recipient/approval rules | Onchain role scope | A direct EOA sender override can bypass Safe policy |
| Safe ownership | Safe transaction authorization | Safe smart account | Default threshold-1 owner can bypass the Roles modifier; policy is not an absolute security boundary |
| API authentication | Session-only wallet-control endpoints | Hosted authorization | Organization API keys cannot provision/delete/withdraw/export/switch wallets or approve human-boundary actions |
| Official Hermes plugin | Writes absent by default | Tool registration | Writes appear only when explicitly enabled; application-specific policy is still required |

## Source-level correction: organization spending caps

KeeperHub documentation says organizations can configure daily caps in wei. The audited source confirms that the shared direct/workflow ledger sums native value moved:

- EVM cap: `dailyValueCapWei`;
- Solana cap: `dailySolanaValueCapLamports`;
- token-only or zero-native-value calls are recorded with zero native notional;
- no configured cap means unlimited for that chain family.

Therefore, the organization cap should not be described as a universal USD or ERC-20 spending limit. Safe/Zodiac token allowances or product-specific token accounting are needed for token-value policy.

## What remains genuinely useful

Potentially useful layers must go beyond generic safety controls:

1. **Outcome assurance:** prove the transaction achieved the intended business result, not merely that a receipt succeeded.
2. **Evidence-to-action judgment:** interpret difficult external/onchain evidence for one specific user job, while existing KeeperHub controls bound execution.
3. **Ambiguous-state reconciliation:** determine what happened after timeout, partial workflow failure, duplicate trigger, reorg, or uncertain postcondition before any retry.
4. **Effective-policy analysis:** explain the real protection after accounting for EOA overrides, Safe owner bypass, contract-only presets, active Sender choice, and token-cap scope.
5. **Domain-specific approval:** show a human exactly the consequence and evidence for one high-impact action rather than presenting a generic wallet confirmation.

These are opportunity territories, not final project selections. Each still needs user-demand evidence, current-field subtraction, an access spike, and a narrow testable product loop.

## Product decision

Do **not** pursue this generic proposition:

> “A safety harness that gives an onchain AI agent spending limits, whitelisted addresses, simulation, and approvals.”

KeeperHub already covers most of it.

A surviving proposition must instead look like:

> “For a specific user facing a specific consequential decision, the application interprets evidence, uses KeeperHub's existing policies and execution controls, then independently proves or reconciles the intended outcome.”

## Primary evidence

- Agentic wallet controls: https://docs.keeperhub.com/ai-tools/agentic-wallet
- Direct execution, caps, simulation and idempotency: https://docs.keeperhub.com/api/direct-execution
- Safe and Zodiac Roles: https://docs.keeperhub.com/wallet-management/safe
- API/session security boundaries: https://docs.keeperhub.com/api/authentication
- MCP tools and write surfaces: https://docs.keeperhub.com/ai-tools/mcp-server
- Pinned core implementation is recorded in `CONTEXT_DOSSIER.md`; relevant source files include `lib/execute/value-ledger.ts` and `app/api/execute/_lib/spending-cap.ts`.
