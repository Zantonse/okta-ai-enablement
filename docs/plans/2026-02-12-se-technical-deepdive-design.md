# SE Technical Deep-Dive Page - Design Document

**Date:** 2026-02-12
**Status:** Approved

## Overview

An internal-only single-page HTML/CSS/JS technical reference for Okta Sales Engineers covering Okta AI products (Okta for AI Agents + Auth0 for AI Agents). Organized by feature/capability with protocol-level detail, demo scripts, code snippets, and technical objection handling.

## Audience

- **Okta SEs, Solution Architects, and technical pre-sales** preparing for and executing customer engagements
- Entire page is internal — no customer-facing sections or seller mode toggle needed

## Usage Scenarios

- **Live calls:** Quick-jump sidebar nav, collapsible sections, copy-to-clipboard code
- **Pre-call prep:** Dense technical content, sequence diagrams, demo scripts
- **Post-call reference:** Troubleshooting guides, integration matrix, edge cases

## Tech Stack

- Single `technical.html` file with inline CSS and vanilla JS
- Same font stack as main site (Instrument Serif, DM Sans, JetBrains Mono)
- No build tools or dependencies
- Cross-linked with main `index.html`

## Visual Direction

- Darker, denser layout than main site — dark background (#0a0a0f), tighter spacing
- Code-forward aesthetic — monospace accents, terminal-style diagrams, syntax highlighting
- Fixed left sidebar nav for fast section jumping during live calls
- Collapsible sections — feature deep-dives start collapsed for quick scanning
- Persistent "INTERNAL — SE USE ONLY" banner

## Page Sections

### 1. Header & Navigation
- "Okta AI — SE Technical Reference" title with INTERNAL badge
- Link back to main enablement site
- Fixed sidebar with jump links, active-section highlighting via IntersectionObserver

### 2. Platform Architecture Overview
- High-level diagram: Okta Platform (WIC/CIC) + Auth0 for AI Agents
- Two-motion positioning: Auth0 for builders, Okta platform for enterprise governance
- Product-to-use-case mapping

### 3-7. Feature Deep-Dives

Each section follows a consistent template:
- **What it is** — 2-3 sentence plain-English explanation
- **How it works** — CSS sequence diagram showing protocol flow, token formats, API endpoints
- **Key technical details** — standards (OAuth 2.0 extensions, IETF drafts), token lifetimes, scoping rules
- **Demo script** — step-by-step walkthrough with talking points
- **Code snippet** — working example (curl/SDK)
- **Common objections & responses** — feature-specific technical rebuttals

The five feature sections:

3. **Cross-App Access (XAA)** — Enterprise-managed token exchange, ID-JAG format, scoped agent access to enterprise resources on behalf of users
4. **Token Vault** — Auth0 token storage/management for third-party APIs, federation patterns, refresh token rotation, CIAL flows
5. **Async Authorization (CIBA)** — Client-Initiated Backchannel Auth for human-in-the-loop, polling vs push modes, timeout handling
6. **Fine-Grained Authorization (FGA for RAG)** — Relationship-based access control in RAG pipelines, FGARetriever integration, authorization at retrieval time
7. **Agent Discovery & Governance** — ISPM, SAM Plugin, agent registration in Universal Directory, OIG policies, System Log monitoring

### 8. Integration Matrix
- Framework × Feature support table (LangChain, LlamaIndex, Vercel AI, CrewAI, etc.)
- SDK availability by language (Python, TypeScript, Go)
- Links to relevant documentation

### 9. Competitive War Room
Detailed technical comparison per competitor (deeper than main site):
- **Microsoft Entra** — capabilities, gaps, technical differentiators
- **Ping Identity** — legacy IAM strengths, agent-native weaknesses
- **Astrix** — ISPM-only play, no runtime enforcement
- **ServiceNow** — IT workflow focus, not identity-native
- "When the customer says X, respond with Y" format for each
- Technical proof points and standards compliance

### 10. Troubleshooting & Edge Cases
- Common implementation gotchas (token expiration, FGA rule conflicts, CIBA timeouts)
- Debugging toolkit (System Log queries, Auth0 dashboard, CLI commands)
- Edge cases (multi-tenant, hybrid cloud, regulated industries)

### 11. Quick Reference Cards
- Condensed summary cards per feature for quick glances
- Key stats for conversation drops (580M access paths, breach costs, adoption %)

## Interactive Elements

- Fixed sidebar nav with active-section highlighting (IntersectionObserver)
- Collapsible sections — feature deep-dives start collapsed, expand on click
- Copy-to-clipboard on all code snippets and curl commands
- Simple text search/filter across all sections
- Step-through sequence diagram animations
