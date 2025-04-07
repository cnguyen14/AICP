# AI Inter-Agent Communication Protocol (AICP)  
**Version 0.1 – Draft Specification**  
**Author:** Chien Nguyen 
**Date:** April 7, 2025

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
- Enable monetized agent services through pay-per-interaction design

---

## 3. Agent Identity & Authentication

### 3.1 Agent Identity

Each agent is assigned a globally unique **Agent Identifier (AID)** in the format:  
`agent://companyDomain/agentName`  
Example:  
`agent://companyA/sales-bot`

---

### 3.2 Authentication Mechanisms

Agents must establish trust before interaction. This involves:

- **API Keys** – Simple integration  
- **OAuth 2.0 / JWT Tokens** – Scalable, secure access control  
- **Public Key Infrastructure (PKI)** – Enterprise-level identity assurance

---

### 3.3 Token Exchange & Registration

Before communication is allowed, **Company A must register with Company B’s system**. This step ensures secure and verified interaction.

**Workflow:**
1. **Registration**: Company A sends a request to register with Company B. It includes the Agent ID, intended use, and metadata.
2. **Approval**: Company B reviews and approves the request.
3. **Token Issuance**: Company B generates a signed token (e.g., JWT) with permissions defined by the agreement.
4. **Verification**: All future communications from Company A to Company B must include this token in the request. Company B verifies the token before processing.

> This registration-based approach protects both sides and ensures only authorized agents can exchange information.

---

### 3.4 Communication Plans & Trust Levels

In AICP, an agent (e.g., Company A) can define **different trust levels** for incoming communication from other agents. This determines **how much the agent is willing to respond**, what types of questions are acceptable, and how deep the interaction can go.

#### Example:

- **Plan 1 (High-Level Enterprise - Trusted Partner):**
  - Can initiate any question or request  
  - Can receive full and detailed responses  
  - May trigger advanced functions (e.g., negotiations, business decisions)  

- **Plan 2 (Standard Partner - Limited Access):**
  - Can only ask predefined question types  
  - May receive summarized or filtered answers  
  - No access to sensitive or strategic-level queries  

> For example, Company A may treat Company B as a high-level enterprise under **Plan 1**, granting them full conversational freedom. Meanwhile, Company C may fall under **Plan 2**, only receiving responses for basic, approved intents.

---

### 3.5 Paid Access via Service Agents

AICP supports the concept of **Service Agents** — agents that provide paid services or premium responses only upon successful payment or subscription validation.

This model supports scenarios such as:
- Access to premium data or analytics
- Pay-per-use transaction processing
- Tiered service models with limited free access

#### Payment Enforcement

A service agent may require:
- A valid **payment token** (e.g., Stripe session ID, invoice ID, credit balance)
- A verified **subscription or entitlement token**
- A reference to a completed transaction (handled off-protocol)

If no valid token is provided, the agent responds with an error:

```plaintext
type: ERROR
intent: get_advanced_specs
message: "Payment required or token invalid"
code: 402
```

#### Supported Models

- **Pay-per-Intent**: Each interaction is metered and charged individually  
- **Access Tiers**: Tokens unlock different levels of response or detail  
- **Subscriptions**: Recurring access is granted for specified durations

> Service Agents allow AI developers and businesses to monetize agent-based systems while maintaining security and protocol alignment.

---

## 4. Communication Protocol

### 4.1 Transport Layer

AICP supports multiple transport layers:

- **HTTPS** (default)  
- **MQTT**, **WebSocket**, or **gRPC** for real-time or embedded applications

---

## 5. Agent Communication Structure

AICP enables intelligent agents to communicate through **intent-driven, semantically meaningful messages**. These messages are not static API calls or queries. Instead, they are part of an ongoing dialog between two intelligent agents that negotiate, request, and respond based on shared understanding and trust.

To achieve this, agents need more than just structured data — they need **mutual comprehension** of each other’s **intents** and **capabilities**.

---

### 5.1 Intent-Based Communication

Each message includes the following components:

- **Protocol Version**  
- **Sender and Receiver AIDs**  
- **Timestamp**  
- **Type** (`REQUEST`, `RESPONSE`, `NEGOTIATION`, `NOTIFY`, `ERROR`)  
- **Intent** (e.g., `get_product_price`, `check_inventory`)  
- **Payload**  
- **Authentication token**

---

### 5.2 Shared Understanding of Intents

AICP supports shared understanding through:

#### A. Shared Intent Registry

Agents can expose supported intents and payload formats publicly or on request using:

```plaintext
intent: list_supported_intents
```

#### B. Intent Discovery

Agents may dynamically explore other agents' capabilities through discovery messages.

#### C. Semantic Flexibility

Agents may use embeddings, ontologies, or LLMs to interpret synonyms and similar intents even without hardcoding.

---

### 5.3 Trust-Aware Response Control

Each agent has the right to selectively respond based on the sender's trust level.

---

### 5.4 Example Interaction (Without Predefined Query)

1. Agent A connects and authenticates with Agent B  
2. Agent A sends: `intent: list_supported_intents`  
3. Agent B responds with a list  
4. Agent A chooses one and sends a `REQUEST` with the proper payload

---

### 5.5 Extensible Message Format

JSON is default, but other formats (XML, Protobuf, GraphQL) are allowed as long as **intent and trust logic** remain.

---

## 6. Use Case: B2B Quote Request

Agent A asks Agent B for pricing information. After discovering supported intents and trust policies, it sends a request with `get_product_price`. Agent B responds with cost and availability.

---

## 7. Security Considerations

- All communication over TLS  
- Expiring/refreshable tokens  
- Trust-based filtering and intent whitelisting  
- Logging of sensitive exchanges  

---

## 8. Future Work

- Blockchain-based agent ID verification  
- Decentralized intent registries  
- Agent marketplaces  
- Built-in billing, metering, and microtransactions  
- SDKs for popular platforms  

---

## 9. Conclusion

AICP provides the foundation for **secure, scalable, and intelligent communication between AI agents**. With support for **trust models**, **paid services**, and **dynamic understanding**, AICP enables the next generation of **autonomous, monetized, and collaborative AI systems** for the real world.

---

## References

- Model Context Protocol (MCP), OpenAI, 2024  
- OAuth 2.0 Authorization Framework, IETF RFC 6749  
- JSON Web Tokens (JWT), RFC 7519  
- MQTT v5.0 Specification, OASIS Standard  
- gRPC Protocol, Google, 2023
