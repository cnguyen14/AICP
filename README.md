# AI Inter-Agent Communication Protocol (AICP)

**Version 0.1 – Draft Specification**  
**Author**: Chien Nguyen
**Date**: April 7, 2025

---

## Abstract

As artificial intelligence (AI) agents become integral to enterprise operations, a need arises for a standardized communication protocol to facilitate secure, intelligent interaction between agents across organizational boundaries. This paper proposes the **AI Inter-Agent Communication Protocol (AICP)** — a secure, extensible framework that enables agents to exchange information, services, and intent-driven tasks in a structured and policy-aware manner.

---

## 1. Introduction

The proliferation of AI agents within businesses, logistics, e-commerce, and other sectors has increased the demand for automation in inter-company communication. Current solutions focus on APIs and manual integration pipelines, but these are not optimized for real-time, AI-native conversations. AICP addresses this gap by enabling agents to **discover, authenticate, and converse** with each other using structured JSON messages governed by organizational trust policies.

---

## 2. Objectives

- Define a **universal protocol** for agent-to-agent communication
- Ensure **data security**, **trust-based control**, and **scalability**
- Enable agents to **negotiate**, **query**, and **respond** autonomously
- Support **differentiated response levels** based on trust models

---

## 3. Related Work

Several standards exist for machine-to-machine (M2M) communication (e.g., MQTT, RESTful APIs), but few are optimized for **AI-driven, intent-based communication** between autonomous agents. Model Context Protocol (MCP) provides a foundation for agent-model interaction, but AICP extends this to **peer-level agent communication**, suitable for B2B or inter-service interactions.

---

## 4. Protocol Architecture

### 4.1 Agent Identity

Each agent is assigned a globally unique **Agent Identifier (AID)** in the format:

```
agent://companyDomain/agentName
```

Example:  
`agent://companyA/sales-bot`

---

### 4.2 Authentication & Token Exchange

Agents must establish trust before interaction. This involves:

- **Registration**: An agent must register with a remote system before requesting access.
- **Approval**: The receiving system approves and sets permission levels.
- **Token Issuance**: A secure token (e.g., JWT) is generated.
- **Token Use**: Tokens must be included in every message for verification.

---

### 4.3 Communication Plans & Trust Levels

Agents can assign **trust-based communication plans** to other agents:

#### Plan 1 – High-Level Enterprise (Trusted):
- Can ask any question
- Receives full response data
- Allowed to trigger sensitive operations

#### Plan 2 – Limited Partner:
- Can only send certain requests
- Receives filtered or summarized responses
- Restricted from sensitive queries

> For example, Company A treats Company B as a trusted enterprise (Plan 1), allowing full interaction. Company C is under Plan 2 and receives only limited replies to basic questions.

---

### 4.4 Transport Protocols

AICP supports multiple transport layers:

- **HTTPS** (default)
- **MQTT**, **WebSocket**, or **gRPC** for real-time or embedded applications

---

### 4.5 Message Format

All messages use JSON:

```json
{
  "protocol": "AICP",
  "version": "0.1",
  "from": "agent://companyA/sales-bot",
  "to": "agent://companyB/pricing-bot",
  "timestamp": "2025-04-07T14:45:00Z",
  "type": "REQUEST",
  "intent": "get_product_price",
  "payload": {
    "product_id": "SKU-1200X",
    "quantity": 100
  },
  "auth": {
    "token": "JWT_or_API_KEY"
  }
}
```

---

### 4.6 Message Types

- `REQUEST` – Data or service inquiry  
- `RESPONSE` – Reply to a request  
- `NEGOTIATION` – Multi-step interaction  
- `NOTIFY` – Asynchronous alerts or updates  
- `ERROR` – Error message with code and description

---

## 5. Use Case: B2B Quote Request

### Scenario:

Company A wants to get a quote from Company B.

#### Request:

```json
{
  "protocol": "AICP",
  "version": "0.1",
  "from": "agent://companyA/supply-bot",
  "to": "agent://companyB/pricing-bot",
  "type": "REQUEST",
  "intent": "get_product_price",
  "payload": {
    "product_id": "SKU-4387Y",
    "quantity": 500
  },
  "auth": {
    "token": "abc123token"
  }
}
```

#### Response:

```json
{
  "protocol": "AICP",
  "version": "0.1",
  "from": "agent://companyB/pricing-bot",
  "to": "agent://companyA/supply-bot",
  "type": "RESPONSE",
  "intent": "get_product_price",
  "payload": {
    "product_id": "SKU-4387Y",
    "unit_price": 13.50,
    "currency": "USD",
    "lead_time_days": 7
  }
}
```

---

## 6. Security Considerations

- All communication must be over **TLS/SSL**
- Tokens should expire and be refreshed regularly
- Agents should implement **intent whitelisting**
- Logging and auditing for sensitive message types is recommended

---

## 7. Future Work

- Federated agent directories with blockchain-based verification  
- Fine-grained policy language for dynamic trust rules  
- Built-in agent discovery and auto-handshake mechanisms  
- SDKs for Python, JavaScript, Go  
- Integration with autonomous negotiation engines

---

## 8. Conclusion

AICP offers a flexible, secure, and extensible communication protocol for AI agents operating in multi-organization ecosystems. By supporting trust-based interaction, structured message formats, and scalable transport layers, it paves the way for seamless B2B automation, AI-native APIs, and intelligent ecosystems.

---

## References

- Model Context Protocol (MCP), OpenAI, 2024  
- OAuth 2.0 Authorization Framework, IETF RFC 6749  
- JSON Web Tokens (JWT), RFC 7519  
- MQTT v5.0 Specification, OASIS Standard  
- gRPC Protocol, Google, 2023
