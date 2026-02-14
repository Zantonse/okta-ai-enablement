# Okta AI Products Enablement Site - Design Document

**Date:** 2026-02-12
**Status:** Approved

## Overview

A single-page HTML/CSS/JS enablement website for Okta AI products (Okta for AI Agents + Auth0 for AI Agents). Dual-purpose: customer-facing narrative content plus internal seller enablement behind a "Seller Mode" toggle.

## Audience

- **External:** Customers and prospects learning about Okta's agentic AI security
- **Internal:** Okta SEs, AEs, and solution architects prepping for customer engagements

## Tech Stack

- Single `index.html` file with inline CSS and vanilla JS
- No build tools or dependencies
- Open directly in browser

## Page Sections (scroll order)

### Public Sections (always visible)

1. **Hero** - "AI Security Starts with Identity" + animated stat counters (91% adopting, 580M access paths, etc.)
2. **The Problem** - Four risk cards (shadow AI, over-permissioned accounts, missing user context, no audit trail) + market stats
3. **Okta Platform Solution** - Four-phase lifecycle: Discover, Register, Protect, Govern with product mapping
4. **Auth0 for AI Agents** - Developer features: User Auth, Token Vault, Async Auth (CIBA), FGA for RAG + code snippet
5. **Architecture Patterns** - Five patterns visualized: XAA, Token Vault, Agent-to-Agent, CIBA, RAG+FGA
6. **Token Exchange Flow** - Visual walkthrough of Okta token exchange

### Seller Mode Sections (toggled)

7. **CIO Talk Track** - Jenna's four-layer framework (People, Process, Technology, Data), positioning tips
8. **Pricing Calculator** - Interactive: users x agents = connections x $5/mo. Adoption stage guidance table
9. **Discovery Questions** - 36 questions in collapsible accordion by category
10. **Competitive Positioning** - Okta vs Microsoft Entra, Ping, Astrix, ServiceNow
11. **Win Stories** - One Main Financial + NTT deal summaries
12. **Resources** - Links to docs, sample apps, SDKs, AI Takeoff Team intake

## Visual Design

- Okta navy (#191919) + blue (#0066CC) for Okta sections
- Auth0 purple (#6742D5) for Auth0 sections
- Warm sand background for internal sections with "INTERNAL" badges
- System font stack, rounded cards with hover effects, animated stat counters
- Responsive for desktop presentations and tablet reference

## Interactive Elements

- Seller Mode toggle with localStorage persistence
- Intersection Observer stat counter animations
- Real-time pricing calculator
- Accordion discovery questions
- Smooth scroll navigation
- Copy-to-clipboard on code snippets
