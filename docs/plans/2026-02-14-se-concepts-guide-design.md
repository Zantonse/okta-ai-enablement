# SE Concepts Guide Design

**Date:** 2026-02-14
**Status:** Approved

## Overview

A new dedicated page (concepts.html) — "AI Agent Identity: Concepts & Fundamentals" — that fills the foundational knowledge gaps an SE needs to confidently discuss Okta's AI agent identity platform in customer meetings.

**Target persona:** Mid-level SE who knows IAM basics (SSO, MFA, SAML) but needs OAuth token mechanics, agent identity model, and AI-specific patterns explained.

**Design system:** Same as index.html and technical.html — CSS variables, light/dark theme toggle, sidebar nav, same fonts and spacing. Linked from both existing pages.

## Page Structure

Sidebar-navigated page with 8 progressive sections:

### 1. Foundations (OAuth 2.0, JWTs, Token Types)

**OAuth 2.0 for Agent Security** — Focused on agent-relevant grants only:
- Authorization Code + PKCE: How users authenticate before agents act on their behalf. Visual flow. Why PKCE prevents interception.
- Client Credentials: How agents authenticate as themselves. When to use vs. token exchange.
- Token Exchange (RFC 8693): The grant that makes delegation work. Side-by-side: "without" (agent uses user's token = dangerous) vs. "with" (scoped, auditable, short-lived).
- Scopes: What they are, how they enforce least-privilege. Example: `read:documents` vs. `admin:all`.

**JWT Anatomy** — Decoded JWT walkthrough:
- Header (alg, typ, kid) — signing algorithms, why RS256 matters
- Payload claims (sub, aud, iss, exp, iat, scope, act) — each explained with "what happens if wrong"
- Signature — verification, key rotation basics
- The `act` claim — delegation chain claim unique to agent identity

**Token Types Compared** — Table: ID Token vs. Access Token vs. Refresh Token vs. ID-JAG with purpose, lifetime, user context, example use.

### 2. Agent Identity Model

**What IS an Agent Identity?** — Core concept:
- Comparison table: Human Identity vs. Service Account vs. Workload Identity vs. Agent Identity
- Key differentiator: Agent identities are bound to human owners, operate on behalf of users
- Visual: Agent identity card (name, type, owner, certificate, permissions, dates)

**Agent Lifecycle** — Timeline diagram:
- Create → Assign → Activate → Monitor → Review → Revoke

**Why Not Just Use Service Accounts?** — The killer objection response:
- Service accounts: long-lived, no user binding, no delegation chain
- Agent identities: short-lived, user-bound, full delegation chain
- Analogy: house key vs. bonded contractor with time-limited access badge

### 3. Delegation & Token Exchange

**Why Delegation Matters** — The "aha moment":
- Problem: Agent with user's full token = agent IS the user
- Solution: Token exchange = agent gets scoped token with `act` claim

**RFC 8693 Step-by-Step** — The WHY behind each step (not just the sequence):
- Why agent authenticates first (proves identity)
- Why user's ID token is subject_token (preserves context)
- Why Okta issues ID-JAG not direct access token (separation of concerns)
- Why resource server validates signature (trust chain)

**ID-JAG Deep Dive** — Decoded payload with annotations for every claim.

### 4. Authorization Patterns

**FGA/ReBAC Explained:**
- RBAC limitations → ReBAC model with relationship graph
- Tuple format explained like a sentence
- Inheritance logic
- Why it matters for RAG

**CIBA:**
- Decision tree: When CIBA vs. regular OAuth
- Binding messages and phishing prevention
- Delivery modes: Push vs. Poll vs. Ping
- Timeout implications

**MCP Authorization:**
- What IS MCP ("USB port for AI agents")
- Why MCP needs authorization
- How Okta secures MCP (OAuth scopes → tool permissions)
- Visual: Agent → MCP Gateway → MCP Server → Resource

### 5. AI-Specific Security

**RAG Security Model:**
- What is RAG (retrieve → inject → answer)
- The security problem (unauthorized documents in LLM context)
- Context injection attacks
- How FGA solves it (check before injection)
- Visual: Pipeline with and without FGA

**Agent Threat Model:**
- Token theft, privilege escalation, shadow agents, prompt injection via tools
- How Okta mitigates each

### 6. Compliance & Governance

**EU AI Act:**
- Article 14 (human oversight → CIBA)
- Article 12 (logging → System Logs)
- Risk classification → OIG
- Enforcement timeline

**SOC2 / HIPAA / GDPR Mapping:**
- Table: Requirement → Okta capability → How to demonstrate

**Audit Logging Architecture:**
- What gets logged, log format, SIEM integration, retention requirements

### 7. Competitive Positioning

**Microsoft Entra Agent ID:**
- Has: Agent registration, managed identities
- Lacks: Cross-platform token exchange, FGA, CIBA
- SE line: "Entra secures inside Azure. Your agents talk to Salesforce, ServiceNow, custom APIs."

**Ping Identity:**
- Has: Emerging concepts, DaVinci
- Lacks: Token vault, FGA, CIBA, governance
- SE line: "Ping is building toward this. Okta is shipping it."

**DIY (Build-Your-Own):**
- What customers attempt, why it fails
- SE line: "You can build a subset. But who maintains it at 2 AM?"

### 8. SE Conversation Guide

**Persona-Based Messaging:**
- CISO: governance, audit, compliance, risk
- CTO: architecture, developer experience, integration
- VP Engineering: SDK simplicity, framework support, PoC timeline
- Compliance Officer: EU AI Act, SOC2, audit evidence

**Objection Response Framework:**
- 10 common objections with: What they're really asking → Technical response → Business response → Proof point

## Design System

- Same CSS variable system as index.html and technical.html
- Light mode default with dark toggle (synced via localStorage)
- Fixed sidebar nav (same pattern as technical.html)
- Collapsible sections for dense content
- Code blocks stay dark in both themes
- Cross-linked from both existing pages

## Files Affected

- NEW: concepts.html
- EDIT: index.html (add nav link to concepts page)
- EDIT: technical.html (add nav link to concepts page)
