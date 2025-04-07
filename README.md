# AI Inter-Agent Communication Protocol (AICP)  
*Version 0.1 — Draft Specification*

---

## 1. Introduction

### 1.1 Purpose  
The **AI Inter-Agent Communication Protocol (AICP)** is a standardized protocol designed to enable **intelligent agents** (AI agents) to securely and reliably communicate across different organizations or systems. It facilitates automated **data exchange**, **requests**, **actions**, and **negotiations** between agents in enterprise or decentralized environments.

### 1.2 Use Cases
- Company A's AI agent requests real-time product pricing from Company B's AI agent.  
- AI agents from different departments or vendors collaborate to optimize a supply chain.  
- Distributed AI agents communicate status updates, service offers, or alerts.

---

## 2. Design Principles

- **Security First**: All communications are encrypted and authenticated.  
- **Interoperability**: Supports multiple platforms, languages, and AI frameworks.  
- **Extensibility**: Easy to expand with new message types and services.  
- **Human-Readable**: Uses JSON for message structure, allowing transparency and debugging.

---

## 3. Agent Identity & Authentication

### 3.1 Agent Identity  
Each agent has a **globally unique identifier (AID)**.  
Example: `agent://companyA/sales-bot`

---

### 3.2 Authentication Mechanisms  
To ensure secure inter-agent communication, AICP enforces token-based authentication. The following methods are supported:

- **API Keys** — For simple integrations  
- **OAuth 2.0 / JWT Tokens** — For scalable, modern applications  
- **Public Key Infrastructure (PKI)** — For enterprise-grade identity assurance

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

This mechanism allows an agent to control **who can ask what**, based on business relationship, reputation, or enterprise agreements.

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

This trust-based structure ensures agents can operate securely while still allowing flexible inter-agent collaboration based on predefined trust models.

---

## 4. Communication Protocol

### 4.1 Transport Layer  
- **HTTPS** (default)  
- **MQTT**, **WebSocket**, or **gRPC** (optional for real-time or embedded systems)

---

## 5. Message Format

All messages are in **JSON format**. Below is the general schema.

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

### 5.1 Message Types

- **REQUEST** – One agent asks another for data or action.  
- **RESPONSE** – Follows up a request with data or confirmation.  
- **NEGOTIATION** – Used for multi-step agreements or pricing.  
- **NOTIFY** – Used for alerts, updates, or broadcasts.  
- **ERROR** – Communicates any issues.

---

## 6. Handshake & Discovery

### 6.1 Agent Discovery  
Agents can optionally register with a **directory service** or be known via DNS-style naming (`agent://domain.com/agentname`).

### 6.2 Handshake Example  
Initial contact may involve exchanging:  
- Capabilities  
- Supported intents  
- Required credentials

---

## 7. Use Case Example

### Company A requests a quote from Company B:

**Request:**
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

**Response:**
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

## 8. Future Plans

- AI-to-AI negotiation using GPT-like reasoning  
- Plug-and-play SDKs in Python, Node.js, Go  
- Blockchain-based identity and verification  
- Federation support for multi-network agent discovery

---

## 9. Conclusion

AICP lays the groundwork for **machine-level B2B communication**. By providing a unified protocol for AI agents to interact, businesses can automate processes, reduce friction, and build intelligent ecosystems that communicate with minimal human input.
