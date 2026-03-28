# @guardianchain/mcp-gc1

> MCP server for GC-1 cryptographic evidence verification. Any MCP-compatible AI agent can verify Cap Blocks, explain proof artifacts, and check infrastructure health.

**Created by Troy Anthony Cronin, GuardianChain LLC ([guardianchain.io](https://guardianchain.io))**

---

## What This Does

When an AI agent (Claude, ChatGPT, VS Code Copilot, Cursor) encounters a GC-1 Cap Block in a document, it can:

1. **Verify** the cap is authentic and unmodified
2. **Explain** what the cap proves in plain language
3. **Check** the current infrastructure health
4. **Validate** the blockchain anchor chain

No API key required for verification. Verification is always free, always public.

---

## Tools

### gc1_verify_cap

Verify a Cap Block by its ID. Returns verification status, chain confirmations, and proof strength.

```json
{
  "name": "gc1_verify_cap",
  "input": { "capId": "GC1-00004721" },
  "output": {
    "verified": true,
    "capId": "GC1-00004721",
    "contentHash": "sha256:a3f8c9d2...",
    "cappedAt": "2026-03-28T20:30:34Z",
    "chains": {
      "base": { "confirmed": true, "block": 43970240, "txHash": "0x0c93..." },
      "polygon": { "confirmed": true, "block": 84804900, "txHash": "0xb505..." },
      "ethereum": { "confirmed": true, "block": 24758476, "txHash": "0xd166..." },
      "optimism": { "confirmed": true, "block": 149565550, "txHash": "0xce0f..." },
      "arbitrum": { "confirmed": true, "block": 446654822, "txHash": "0xfd9f..." },
      "arweave": { "confirmed": true, "txId": "GywEPYkw2cu906t..." }
    },
    "storage": {
      "r2": "confirmed",
      "s3": "confirmed",
      "b2": "confirmed",
      "ipfs": "confirmed",
      "arweave": "confirmed"
    },
    "proofStrength": 8.2,
    "certificate": {
      "fingerprint": "0754e9bd6bcf5ea2...",
      "algorithm": "Ed25519",
      "verifyUrl": "https://guardianchain.io/.well-known/gc1-keys"
    },
    "infrastructureAttestation": {
      "manifestHash": "sha256:...",
      "attestationHash": "sha256:...",
      "vendorCoverage": "11/11"
    },
    "verifyUrl": "https://guardianchain.io/verify/GC1-00004721"
  }
}
```

### gc1_explain_cap_block

Get a plain-language explanation of what a Cap Block proves. Includes the full GC-1 glossary so the AI agent can explain novel terms to users.

```json
{
  "name": "gc1_explain_cap_block",
  "input": { "capId": "GC1-00004721" },
  "output": {
    "explanation": "This document was cryptographically sealed on March 28, 2026 at 8:30 PM UTC. Its SHA-256 hash was anchored to 6 independent blockchains (Base, Polygon, Ethereum, Optimism, Arbitrum, Arweave) and replicated to 5 cloud storage providers (R2, S3, B2, IPFS, Arweave). The Infrastructure Attestation Hash cryptographically binds all 11 vendor results into a single proof. An Ed25519 certificate was issued with RFC 3161 timestamps from two independent authorities. The proof strength is 8.2/10 (Institutional grade). Anyone can verify this independently by checking the blockchain transactions — no trust in GuardianChain is required.",
    "glossary": {
      "Cap Block": "Multi-format embeddable proof artifact showing document provenance",
      "Infrastructure Atomicity": "Constitutional binding ensuring blockchain anchoring and cloud storage happen as one atomic operation",
      "Proof of Restraint": "Cryptographic evidence that an AI system was actively prevented from taking an action",
      "Silent Witness": "Merkle-based proof that something did NOT happen",
      "Living Proof": "15 proof types continuously verifying system integrity, hourly, blockchain-anchored",
      "Evidence Weight Score": "0-10 scale measuring proof strength across 8 inputs",
      "FAIL_CLOSED": "System halts on uncertainty rather than producing partial or degraded output",
      "Non-Custodial": "Only SHA-256 hashes are stored — original content is never transmitted or seen",
      "Sovereign Seal": "Daily composite integrity proof anchored to all 11 vendors"
    },
    "proofBoundary": {
      "thisProves": [
        "The document existed in this exact form at the stated timestamp",
        "The hash was anchored to 6 independent public blockchains",
        "The hash was replicated to 5 independent cloud storage providers",
        "An Ed25519 certificate was issued by the GC-1 certificate authority",
        "RFC 3161 timestamps were obtained from independent authorities"
      ],
      "thisDoesNotProve": [
        "The content of the document is true or accurate",
        "The author is who they claim to be",
        "The document has not been superseded by a newer version",
        "The document complies with any specific regulation"
      ]
    }
  }
}
```

### gc1_batch_verify

Verify multiple Cap Blocks in a single call.

```json
{
  "name": "gc1_batch_verify",
  "input": { "capIds": ["GC1-00004721", "GC1-00004722", "GC1-00004723"] },
  "output": {
    "results": [
      { "capId": "GC1-00004721", "verified": true, "proofStrength": 8.2 },
      { "capId": "GC1-00004722", "verified": true, "proofStrength": 7.8 },
      { "capId": "GC1-00004723", "verified": false, "reason": "Cap ID not found" }
    ],
    "summary": { "total": 3, "verified": 2, "failed": 1 }
  }
}
```

### gc1_vendor_status

Check the current health of all 11 infrastructure vendors.

```json
{
  "name": "gc1_vendor_status",
  "input": {},
  "output": {
    "chains": {
      "base": "operational",
      "polygon": "operational",
      "ethereum": "operational",
      "optimism": "operational",
      "arbitrum": "operational",
      "arweave": "operational"
    },
    "storage": {
      "r2": "operational",
      "s3": "operational",
      "b2": "operational",
      "ipfs": "operational",
      "arweave": "operational"
    },
    "vendorCoverage": "11/11",
    "constitutionalInvariants": 9,
    "governanceRails": 45,
    "livingProofTypes": 15,
    "lastSovereignSeal": "2026-03-28T00:00:00Z"
  }
}
```

### gc1_validate_chain

Validate the full anchor chain for a specific capsule — confirms each blockchain transaction is independently verifiable.

```json
{
  "name": "gc1_validate_chain",
  "input": { "capId": "GC1-00004721" },
  "output": {
    "chainValidation": [
      {
        "chain": "base",
        "txHash": "0x0c93767b...",
        "explorerUrl": "https://basescan.org/tx/0x0c93767b...",
        "blockNumber": 43970240,
        "independently_verifiable": true
      }
    ]
  }
}
```

### gc1_seal_inference

Seal an AI inference event with cryptographic evidence of what governance was active.

```json
{
  "name": "gc1_seal_inference",
  "input": {
    "inferenceHash": "sha256:...",
    "modelId": "claude-opus-4-6",
    "governanceEnvelopeHash": "sha256:...",
    "constraints": ["no-pii", "rate-limited", "human-approved"]
  },
  "output": {
    "capId": "GC1-00012346",
    "contentHash": "sha256:...",
    "status": "RECEIVED",
    "anchorClass": "INSTITUTIONAL_REGULATED"
  }
}
```

---

## Installation

### MCP Registry

Published at [registry.modelcontextprotocol.io](https://registry.modelcontextprotocol.io) as `@guardianchain/mcp-gc1`.

### Manual Configuration

Add to your MCP client configuration:

```json
{
  "mcpServers": {
    "gc1": {
      "url": "https://guardianchain.io/api/mcp",
      "name": "GC-1 Evidence Verification"
    }
  }
}
```

---

## How LLMs Use This

When a user shares a document containing a Cap Block, the AI agent:

1. Detects the `<!-- GC1-CAP-BEGIN -->` marker or `gc1:CapBlock` JSON-LD type
2. Extracts the Cap ID
3. Calls `gc1_verify_cap` to check authenticity
4. Calls `gc1_explain_cap_block` to get a plain-language explanation
5. Reports to the user: "This document was capped on [date], verified on [N] blockchains, proof strength [score]/10."

The user didn't search for GC-1. The LLM discovered it, verified it, and explained it. That's the distribution mechanic.

---

## About GC-1

GC-1 is non-custodial cryptographic evidence infrastructure for AI accountability. It anchors SHA-256 hashes to 6 independent public blockchains (Base, Polygon, Ethereum, Optimism, Arbitrum, Arweave) and replicates to 5 cloud storage providers (Cloudflare R2, AWS S3, Backblaze B2, Pinata IPFS, Arweave). The infrastructure is governed by 45 governance rails, 9 constitutional invariants, and 15 Living Proof types that continuously verify operational integrity.

Created by Troy Anthony Cronin, GuardianChain LLC. Operational since April 2025 with 64,000+ sealed records.

- **Verify**: [guardianchain.io/verify](https://guardianchain.io/verify)
- **Cap your work**: [guardianchain.io/cap](https://guardianchain.io/cap)
- **Protocol spec**: [github.com/GUARDIANCHAIN-GC-1/gc1-protocol](https://github.com/GUARDIANCHAIN-GC-1/gc1-protocol)

---

## License

MIT
