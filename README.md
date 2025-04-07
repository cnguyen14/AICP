# Agent Communication Protocol (ACP) 
**Version 0.1 – Draft Specification**  
**Author:** Chien Nguyen 
**Date:** April 07 2025

---

## Abstract

As artificial intelligence (AI) agents become integral to enterprise operations, a need arises for a standardized communication protocol to facilitate secure, intelligent interaction between agents across organizational boundaries. This paper proposes the **AI Inter-Agent Communication Protocol (AICP)** — a secure, extensible framework that enables agents to exchange information, services, and intent-driven tasks in a structured and policy-aware manner.

---

## 1. Introduction

The proliferation of AI agents within businesses, logistics, e-commerce, and other sectors has increased the demand for automation in inter-company communication. Current solutions focus on APIs and manual integration pipelines, but these are not optimized for real-time, AI-native conversations. AICP addresses this gap by enabling agents to **discover, authenticate, and converse** with each other using structured messages governed by organizational trust policies.

---

## 2. Objectives

- Define a **universal protocol** for agent-to-agent communication  
- Ensure **data security**, **trust-based control**, and **scalability**  
- Enable agents to **negotiate**, **query**, and **respond** autonomously  
- Support **differentiated response levels** based on trust models  
- Enable **service agents** with optional billing and credit enforcement  
- Include safety and escalation mechanisms for reliability at scale

---

## 3. Agent Identity & Authentication

### 3.1 Agent Identity

Each agent is assigned a globally unique **Agent Identifier (AID)** in the format:  
`agent://companyDomain/agentName`  
Example:  
`agent://companyA/sales-bot`

---

### 3.2 Authentication Mechanisms

AICP supports multiple authentication approaches, including:
- **API Keys** – For simple or internal use cases  
- **OAuth 2.0 / JWT Tokens** – For modern web-based agent authorization  
- **Certificates** – For secure, verified B2B communication (recommended)

---

### 3.3 Certificate-Based Agent Registration & Authentication

To initiate communication, **Company A must register its agent with Company B**. This process establishes trust and defines what kinds of requests Agent A is authorized to make to Agent B.

#### A. Registration Request Flow

1. **Company A initiates registration**, providing:
   - Agent Identifier (AID)
   - Contact info
   - Business name
   - Intended intents (e.g., `get_product_price`)
   - Billing preferences (if any)

2. **Company B reviews** and:
   - Approves or denies
   - Assigns allowed intents and trust level
   - Determines billing requirements (if applicable)

---

#### B. Certificate Issuance

Upon approval:
- **Company B generates a signed certificate** for Agent A  
- **The certificate is sent to Company A** to install on their agent  
- Company B stores the certificate and maps it to:
  - The AID  
  - The list of allowed intents  
  - Trust level and billing account (if used)

---

#### C. Certificate-Based Authentication

Agent A must present this certificate on every message sent to Agent B.  
Company B will:
- Verify the certificate's signature and validity  
- Lookup associated permissions and billing config  
- Approve or reject requests based on trust level, usage, and quota

---

#### D. Certificate Expiration & Renewal

- Certificates contain an expiration date  
- Company A must renew before expiry  
- Company B may revoke certificates at any time

---

#### E. Optional Billing Agent Integration

If payment is required:
- Billing is linked to certificate identity  
- Usage is tracked per intent  
- Credits, subscriptions, or postpaid plans are enforced accordingly

---

### 3.4 Communication Plans & Trust Levels

Agents can assign trust levels to others:

| Trust Level | Capabilities |
|-------------|--------------|
| Enterprise  | Full access, any question, negotiations |
| Standard    | Limited intents, filtered data |
| Basic       | Read-only, public info only |

This allows flexible control over what agents can do or see based on business relationship or intent.

---

### 3.5 Paid Access via Service Agents

**Service Agents** are agents that provide paid services such as premium data, automation, or negotiation.

#### Payment Enforcement

Agents may require:
- Valid payment token or session
- Credit balance or active subscription
- Linked billing identity (via certificate or token)

Error response for unpaid access:

```plaintext
type: ERROR
code: 402
message: "Payment required"
```

#### Role-Based Billing Logic

| Agent Role         | Billing Requirement     |
|--------------------|-------------------------|
| Information Agent  | Free                    |
| Internal Agent     | No external access      |
| Service Agent      | Paid                    |
| Hybrid Agent       | Mixed (free + paid)     |

---

## 4. Communication Protocol

### 4.1 Transport Layer

- **HTTPS** (default)  
- **WebSocket / gRPC / MQTT** (optional for real-time)

---

## 5. Agent Communication Structure

AICP enables **intent-based, semantic communication** between autonomous agents.

---

### 5.1 Intent-Based Message Format

Each message includes:
- `protocol`: `"AICP"`  
- `version`: `"0.1"`  
- `from`: Agent AID  
- `to`: Agent AID  
- `timestamp`: UTC  
- `type`: `REQUEST`, `RESPONSE`, `NOTIFY`, etc.  
- `intent`: Purpose (e.g., `get_product_price`)  
- `payload`: Request data  
- `auth`: Certificate or token reference

---

### 5.2 Shared Understanding of Intents

Agents can:
- Publish supported intents (via registry)
- Respond to `list_supported_intents` requests
- Use ontology or LLM-based mappings for flexibility

---

### 5.3 Trust-Aware Response Control

Responses vary by trust level and allowed intents.

---

### 5.4 Example Interaction

1. Agent A: `list_supported_intents`  
2. Agent B: Responds with a list  
3. Agent A: Sends `get_product_price`  
4. Agent B: Responds or rejects based on access config

---

### 5.5 Extensible Format

JSON is default; XML, gRPC, or Protobuf are also supported if both agents agree.

---

## 6. Use Case: B2B Quote Request

Company A’s agent requests pricing from Company B’s service agent. After certificate authentication and trust-level validation, the pricing is returned.

---

## 7. Security Considerations

- Encrypted channels (TLS)
- Certificate expiration and revocation
- Token fallback support (optional)
- Activity logging and rate limiting
- Role-based access controls per intent

---

## 8. Future Work

- Agent marketplaces  
- On-chain certificate verification  
- Public agent directories  
- SDKs for rapid integration  
- Integration with decentralized identity protocols

---

## 9. Conclusion

AICP defines a flexible, secure, and extensible framework for autonomous AI agents to communicate, transact, and collaborate across organizational boundaries. By leveraging trust models, billing integration, and certificate-based authentication, it empowers enterprises to automate responsibly and intelligently.

---

## 10. Fault Handling, Loops & Safety

### 10.1 Loop Prevention

- Message ID tracking  
- TTL / hop limits  
- Retry caps

---

### 10.2 Error Handling

Standard codes:
- `401 Unauthorized`
- `402 Payment Required`
- `403 Forbidden`
- `429 Too Many Requests`
- `500 Internal Error`

---

### 10.3 Health & Trust Signals

- Agent health status: `healthy`, `degraded`, `offline`  
- Auto-suspend high-failure agents  
- Trust reputation scoring

---

### 10.4 Billing Abuse Protection

- Daily usage caps  
- Rate limit per intent  
- Auto-lock for unpaid overuse

---

### 10.5 Manual Override

- Kill switch / pause toggle  
- Webhook logging of abuse  
- Admin tools for revocation

---

### 10.6 Human Escalation

If the system detects:
- Unresolvable errors  
- Repeating failure loops  
- Undefined or dangerous intent

Then the agent should:
- Escalate to human via alert or dashboard  
- Log the full message history  
- Pause further communication until reviewed

> Automation should never override safety. When in doubt — escalate to a human.

---
