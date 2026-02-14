# AI Agent Use Cases Page Design

**Date:** 2026-02-14
**Status:** Approved

## Overview

A new dedicated page (usecases.html) — "AI Agent Use Cases by Industry" — that gives SEs customer-ready scenarios to walk through during meetings, organized by industry vertical with full narrative stories.

**Audience:** Dual-purpose. Clean enough to share on-screen with customers. Seller mode toggle reveals SE-only talk tracks, competitive context, and pricing guidance.

**Design system:** Same as index.html, technical.html, and concepts.html — CSS variables, light/dark theme toggle, sidebar nav, same fonts and spacing. Seller mode toggle from index.html. Linked from all existing pages.

## Page Structure

Sidebar-navigated page with a cross-industry summary and 7 industry sections, each containing 2-3 full narrative scenarios (21 total).

### Page Header

- Internal banner ("INTERNAL DOCUMENTATION — SE USE ONLY")
- Title: "AI Agent Use Cases by Industry"
- Subtitle: "Real-world scenarios showing how Okta secures AI agents across verticals"
- Back link to Enablement Hub
- Search box, theme toggle, seller mode toggle

### Sidebar Navigation

- Cross-Industry Patterns
- Financial Services
- Healthcare & Life Sciences
- Retail & E-Commerce
- Technology & SaaS
- Insurance
- Government & Public Sector
- Manufacturing & Supply Chain

### Cross-Industry Patterns Section

Summary grid at the top mapping scenario archetypes to Okta capabilities:

| Pattern | Description | Primary Okta Capabilities |
|---------|-------------|--------------------------|
| Customer-Facing Agents | Agents that interact with end customers on behalf of the business | Token Vault + CIBA |
| Internal Knowledge Agents | Agents that search and synthesize internal documents for employees | FGA + RAG Authorization |
| Compliance & Audit Agents | Agents that monitor, report, and enforce regulatory requirements | OIG + System Logs + ISPM |
| Multi-System Orchestration | Agents that coordinate actions across multiple SaaS and internal APIs | XAA + MCP Authorization |
| Human-Approved Workflows | Agents that handle high-value or sensitive actions requiring approval | CIBA + Delegation Chain |

### Each Industry Section Structure

**Market Context Block:**
- 2-3 stat boxes (industry-specific AI adoption metrics)
- 1-2 sentence industry framing

**2-3 Full Narrative Scenarios, each containing:**
- Scenario title (e.g., "Fraud Detection Agent")
- **The Persona** — who initiates this, what's their role, what's their day like
- **The Problem** — what happens without proper agent identity (2-3 vivid sentences)
- **The Agent Workflow** — day-in-the-life narrative, 5-6 numbered steps showing the agent's actions, user interactions, data flows
- **Okta Capabilities Used** — tagged chips (XAA, Token Vault, FGA, CIBA, ISPM, OIG) with one sentence on how each applies
- **Before/After** — two-column comparison: without Okta governance vs. with Okta
- **Impact** — ROI stat or business outcome
- **[Seller Mode Only]** — SE talk track, competitive positioning for this scenario, objection pre-empts

### Industries and Scenarios

| Industry | Scenario 1 | Scenario 2 | Scenario 3 |
|----------|-----------|-----------|-----------|
| Financial Services | Fraud Detection Agent | Wealth Advisory Agent | Regulatory Compliance Agent |
| Healthcare & Life Sciences | Clinical Decision Support Agent | Patient Intake Agent | Medical Records RAG Agent |
| Retail & E-Commerce | Personalized Shopping Agent | Inventory Management Agent | Customer Service Returns Agent |
| Technology & SaaS | DevOps Incident Response Agent | Customer Onboarding Agent | Internal Knowledge Agent |
| Insurance | Claims Processing Agent | Underwriting Risk Agent | Policy Renewal Agent |
| Government & Public Sector | Citizen Services Agent | Grant Processing Agent | Inter-Agency Data Sharing Agent |
| Manufacturing & Supply Chain | Quality Inspection Agent | Supply Chain Optimization Agent | Maintenance Prediction Agent |

### Seller Mode Content

Hidden behind the seller toggle (same pattern as index.html):
- Per-scenario: SE talk track, competitive differentiation, pricing context
- Per-industry: Common objections in this vertical, design partner references, deal strategy

### Design System

- Same CSS variable system as other pages
- Light mode default with dark toggle (synced via localStorage)
- Fixed sidebar nav (same pattern as technical.html / concepts.html)
- Collapsible sections for each scenario
- Seller mode toggle (same pattern as index.html)
- Stat boxes, cards-grid, two-col layouts reused
- Code blocks stay dark in both themes
- Cross-linked from all 3 existing pages

## Files Affected

- NEW: usecases.html
- EDIT: index.html (add nav link to usecases page)
- EDIT: technical.html (add sidebar link to usecases page)
- EDIT: concepts.html (add sidebar link to usecases page)
