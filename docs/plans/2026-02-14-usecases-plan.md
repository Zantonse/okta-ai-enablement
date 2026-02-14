# AI Agent Use Cases Page Implementation Plan

> **For Claude:** REQUIRED SUB-SKILL: Use superpowers:executing-plans to implement this plan task-by-task.

**Goal:** Build usecases.html — a dual-purpose page (customer-facing + seller mode) with 21 full narrative AI agent scenarios across 7 industries.

**Architecture:** Single self-contained HTML file with embedded CSS and JS, same design system as concepts.html (sidebar nav, light/dark theme, collapsible sections). Adds seller mode toggle from index.html. Industry-first organization with cross-industry pattern summary.

**Tech Stack:** HTML, CSS (custom properties), vanilla JS. No build tools, no framework.

---

## Task 0: Scaffold usecases.html with page shell and design system

**Files:**
- Create: `usecases.html`

**What to build:**

Create the page shell by copying the design system from concepts.html and adding seller mode from index.html. The page needs:

- HTML boilerplate matching concepts.html (same Google Fonts, same meta tags)
- Full CSS design system from concepts.html: `:root` light variables, `html[data-theme="dark"]` dark override, typography, sidebar, header, content-wrapper, noise overlay, code block styles, collapsible sections, accordion, callout, cards-grid, stat-box, responsive breakpoints, hamburger menu
- **PLUS** seller mode CSS from index.html:
  ```css
  .seller-toggle{
    display:flex;align-items:center;gap:10px;
    padding:8px 14px;border-radius:8px;
    border:1px solid var(--c-border);
    cursor:pointer;transition:all 0.3s;
    background:transparent;color:var(--c-text-secondary);
    font-family:var(--font-mono);font-size:11px;
    letter-spacing:1px;text-transform:uppercase;
    user-select:none;min-height:44px;
  }
  .seller-toggle:hover{border-color:var(--c-accent-amber);color:var(--c-accent-amber)}
  .seller-toggle.active{
    border-color:var(--c-accent-amber);
    background:rgba(245,166,35,0.08);
    color:var(--c-accent-amber);
  }
  .seller-toggle .toggle-dot{
    width:8px;height:8px;border-radius:50%;
    background:var(--c-text-secondary);transition:all 0.3s;
  }
  .seller-toggle.active .toggle-dot{
    background:var(--c-accent-amber);
    box-shadow:0 0 8px rgba(245,166,35,0.5);
  }
  .seller-section{
    display:none;
    border-top:2px solid rgba(245,166,35,0.2);
    position:relative;
    background:rgba(245,166,35,0.03);
    padding:var(--spacing-lg);
    border-radius:var(--radius-lg);
    margin-top:var(--spacing-md);
  }
  body.seller-mode .seller-section{display:block}
  .seller-badge{
    position:absolute;top:12px;right:16px;
    font-family:var(--font-mono);font-size:10px;
    letter-spacing:2px;text-transform:uppercase;
    color:var(--c-accent-amber);
    padding:4px 10px;border:1px solid rgba(245,166,35,0.3);
    border-radius:4px;background:rgba(245,166,35,0.06);
  }
  ```
- Internal banner ("INTERNAL DOCUMENTATION — SE USE ONLY")
- Header with back link to index.html ("Back to Enablement Hub"), page title "AI Agent Use Cases by Industry", subtitle "Real-world scenarios showing how Okta secures AI agents across verticals", search box, theme toggle, **seller mode toggle**
- Mobile search bar + hamburger
- Sidebar with all section navigation links:
  - Overview: cross-industry-patterns
  - Financial Services: fins-fraud, fins-wealth, fins-compliance
  - Healthcare: health-clinical, health-intake, health-records
  - Retail: retail-shopping, retail-inventory, retail-returns
  - Technology: tech-devops, tech-onboarding, tech-knowledge
  - Insurance: insurance-claims, insurance-underwriting, insurance-renewal
  - Government: gov-citizen, gov-grants, gov-interagency
  - Manufacturing: mfg-quality, mfg-supply, mfg-maintenance
- Empty content-wrapper div with placeholder sections for all 22 sections (1 cross-industry + 21 scenarios)
- JavaScript: theme toggle (localStorage 'theme'), seller mode toggle (localStorage 'sellerMode'), search, sidebar active tracking, collapsible section handlers, hamburger toggle, smooth scrolling, accordion handlers
- Footer matching other pages

**Commit:** `feat: scaffold usecases.html with page shell and design system`

---

## Task 1: Cross-Industry Patterns section

**Files:**
- Modify: `usecases.html`

**Section ID:** `cross-industry-patterns`

**Content to write:**

### Cross-Industry Patterns

Opening: "Before diving into specific industries, here are the five agent archetypes you'll see across every vertical. Each maps to a specific set of Okta capabilities."

**5-row pattern cards** (use cards-grid or styled rows):

**Pattern 1: Customer-Facing Agents**
- Description: Agents that interact with end customers — answering questions, processing requests, providing recommendations
- Primary Okta Capabilities: Token Vault (access customer data in CRM/SaaS), CIBA (human approval for sensitive actions)
- Example industries: Financial Services, Retail, Insurance, Healthcare
- Key risk without Okta: Agent accesses all customer data regardless of the requesting user's authorization level

**Pattern 2: Internal Knowledge Agents**
- Description: Agents that search, synthesize, and summarize internal documents for employees
- Primary Okta Capabilities: FGA (document-level access control), RAG Authorization (filter before LLM injection)
- Example industries: Technology, Government, Manufacturing, All
- Key risk without Okta: Confidential HR docs, M&A plans, or salary data leak into any employee's LLM context

**Pattern 3: Compliance & Audit Agents**
- Description: Agents that monitor regulatory requirements, generate compliance reports, flag violations
- Primary Okta Capabilities: OIG (governance lifecycle), System Logs (delegation chain audit), ISPM (agent discovery)
- Example industries: Financial Services, Healthcare, Insurance, Government
- Key risk without Okta: Compliance agent itself becomes an ungoverned access point to regulated data

**Pattern 4: Multi-System Orchestration Agents**
- Description: Agents that coordinate actions across multiple SaaS platforms and internal APIs in a single workflow
- Primary Okta Capabilities: XAA (cross-app token exchange), MCP Authorization (tool-level permissions), Token Vault (SaaS tokens)
- Example industries: Technology, Manufacturing, Retail, All
- Key risk without Okta: Agent accumulates credentials across systems — a single compromise cascades everywhere

**Pattern 5: Human-Approved Workflow Agents**
- Description: Agents handling high-value or irreversible actions that require explicit human sign-off before execution
- Primary Okta Capabilities: CIBA (async human approval), Delegation Chain (act claim preservation)
- Example industries: Financial Services, Healthcare, Insurance, Government
- Key risk without Okta: Agent auto-approves actions that should have human oversight — wire transfers, medication changes, benefits decisions

**Commit:** `feat: add cross-industry patterns section`

---

## Task 2: Financial Services scenarios

**Files:**
- Modify: `usecases.html`

**Section IDs:** `fins-fraud`, `fins-wealth`, `fins-compliance`

**Industry header:**
- Section title: "Financial Services"
- Stats grid (3 stat boxes):
  - 85% — Of financial firms actively applying AI (Resources Global Professionals, 2025)
  - $3.2T — Potential annual value from AI in banking (McKinsey, 2024)
  - 59% — Of banking employees use AI daily (Accenture, 2025)
- Industry framing: "Financial services leads AI adoption — and faces the strictest regulatory requirements. Every agent workflow involves regulated data, cross-border compliance, and customer trust."

**Scenario 1: Fraud Detection Agent** (id="fins-fraud")

- **The Persona:** Maria, a fraud analyst at a mid-size bank. She monitors 50,000+ daily transactions and currently relies on rule-based alerts that generate 60% false positives.
- **The Problem:** The bank deploys an AI fraud detection agent to reduce false positives. Without proper identity, the agent has a service account with read access to ALL transaction data across ALL customers. When the agent is compromised via a supply chain attack on its ML library, the attacker has unfettered access to every transaction in the system. The bank can't even tell which transactions the attacker queried because the audit log just shows "svc-fraud."
- **The Agent Workflow:**
  1. Maria logs into the fraud monitoring dashboard (Okta SSO, MFA)
  2. She asks the fraud agent: "Analyze unusual patterns in high-value wire transfers this week"
  3. The agent authenticates to Okta with its certificate (Agent Identity)
  4. Agent performs token exchange: its credentials + Maria's ID token → Okta issues scoped ID-JAG
  5. Agent queries the transaction database with a scoped access token — only wire transfers, only Maria's assigned accounts, only this week's data
  6. Agent returns analysis to Maria. She flags 3 transactions for review.
  7. For each flagged transaction, the agent sends a CIBA request: "Freeze account #4521 pending investigation" → Maria approves on her phone
- **Okta Capabilities Used:** Agent Identity, XAA (token exchange), FGA (account-level access), CIBA (freeze approval), System Logs (full audit trail)
- **Before/After:**
  - Without Okta: Agent uses service account → full database access → compromise = all transactions exposed → audit log shows "svc-fraud"
  - With Okta: Agent uses delegation → scoped to analyst's accounts → compromise = only current query window exposed → audit log shows "agent:fraud-detector acting as maria@bank.com, scope: read:wire-transfers"
- **Impact:** 50% reduction in false positives. Blast radius of a compromise reduced from "all transactions" to "5-minute window of one analyst's assigned accounts."
- **[Seller Mode]:** Lead with the blast-radius comparison. CISOs immediately understand the risk reduction. If they say "we already have fraud AI," respond: "Great — who governs its access? Show me the audit trail that says which analyst's authority the agent used."

**Scenario 2: Wealth Advisory Agent** (id="fins-wealth")

- **The Persona:** James, a wealth advisor managing 200 high-net-worth client portfolios. He spends 40% of his day pulling data from 6 different systems (CRM, portfolio management, market data, compliance, tax, estate planning).
- **The Problem:** The bank builds an AI assistant that aggregates data across all 6 systems for James. Without proper identity, the agent stores James's credentials for each system in a configuration file. When James goes on vacation, the agent continues operating with his full credentials — another advisor accidentally queries a client's estate plan through the agent, and the audit trail shows James accessed it (not the other advisor).
- **The Agent Workflow:**
  1. James asks the agent: "Prepare a quarterly review package for client Chen Wei — portfolio performance, tax implications, and estate planning updates"
  2. Agent authenticates with its own certificate to Okta
  3. Token exchange: agent credentials + James's ID token → scoped ID-JAG
  4. Agent uses Token Vault to access Salesforce CRM (client contact info), Bloomberg (market data), internal portfolio API (holdings)
  5. FGA check: Can James view Chen Wei's estate planning documents? → Yes (James is the assigned advisor)
  6. Agent compiles the quarterly package. James reviews and sends to client.
  7. When James goes on vacation, the agent's delegation chain shifts to the covering advisor — access is scoped to their assigned clients, not James's.
- **Okta Capabilities Used:** Token Vault (6-system SaaS access), XAA (internal API delegation), FGA (client-level document access), Agent Identity (certificate auth)
- **Before/After:**
  - Without Okta: Agent stores advisor's credentials → accesses all clients → vacation = wrong advisor's audit trail → credential leak = all 200 portfolios
  - With Okta: Agent uses delegation → scoped to requesting advisor's clients → vacation = proper handoff with correct audit → Token Vault manages all SaaS tokens
- **Impact:** 40% reduction in time spent gathering data. Zero cross-client data exposure incidents.
- **[Seller Mode]:** VP Engineering cares about the 6-system integration complexity. "Token Vault handles Salesforce, Bloomberg, and 4 more — your developers write `getAccessTokenForConnection`, not custom OAuth flows for each." CTO angle: "Single control plane for all your advisor agent integrations."

**Scenario 3: Regulatory Compliance Agent** (id="fins-compliance")

- **The Persona:** Priya, Head of Compliance at a regional bank. Her team of 5 manually reviews 200+ regulatory updates per month and maps them to internal policies. They're 3 months behind.
- **The Problem:** The bank deploys an AI compliance agent that scans regulatory feeds (SEC, OCC, FDIC, CFPB), identifies relevant changes, and drafts policy impact assessments. Without governance, the agent stores its analysis in a shared drive accessible to anyone. A junior analyst finds the draft impact assessment for an upcoming enforcement action — information that should be restricted to the compliance committee.
- **The Agent Workflow:**
  1. Priya asks the compliance agent: "Identify all OCC guidance changes affecting our commercial lending policies in the past 30 days"
  2. Agent authenticates to Okta with its certificate
  3. Agent accesses regulatory feed APIs using XAA (scoped to commercial lending regulations)
  4. Agent searches the internal policy document store via RAG — FGA ensures only policies Priya has authority over are included in context
  5. Agent generates draft impact assessment with specific policy sections affected
  6. Agent stores the assessment in the compliance-restricted folder — FGA enforces that only compliance committee members can access it
  7. OIG certification campaign triggers quarterly: "Should compliance-agent still have access to enforcement-sensitive documents?" → Priya reviews and confirms
- **Okta Capabilities Used:** XAA (regulatory API access), FGA (policy document access + storage restrictions), OIG (periodic access review), System Logs (who saw what assessment)
- **Before/After:**
  - Without Okta: Agent dumps reports to shared drive → anyone sees enforcement-sensitive drafts → no access review → 3 years later agent still has all permissions
  - With Okta: Agent writes to FGA-protected storage → only committee members see → quarterly OIG review → agent access audited and certified
- **Impact:** 80% faster regulatory change processing. Zero unauthorized access to enforcement-sensitive assessments.
- **[Seller Mode]:** Compliance Officer persona — lead with "EU AI Act Article 12 requires traceability. Here's the audit evidence your regulators want to see." The OIG certification campaign is the differentiator competitors can't match.

**Commit:** `feat: add financial services use case scenarios`

---

## Task 3: Healthcare & Life Sciences scenarios

**Files:**
- Modify: `usecases.html`

**Section IDs:** `health-clinical`, `health-intake`, `health-records`

**Industry header:**
- Section title: "Healthcare & Life Sciences"
- Stats grid:
  - 73% — Of health systems investing in AI (HIMSS, 2025)
  - $187B — Expected healthcare AI market by 2030 (Grand View Research)
  - 78% — Of physicians say AI could reduce burnout (AMA, 2024)
- Industry framing: "Healthcare AI agents handle the most sensitive data in any industry — patient health information (PHI). HIPAA, FDA regulations, and clinical safety requirements make identity governance non-negotiable."

**Scenario 1: Clinical Decision Support Agent** (id="health-clinical")

- **Persona:** Dr. Sarah Chen, an emergency physician seeing 40+ patients per shift. She needs instant access to drug interactions, clinical guidelines, and patient history to make time-critical decisions.
- **Problem:** The hospital deploys an AI clinical support agent. Without proper identity, the agent can access any patient's full medical record regardless of which physician is treating them. A night-shift physician queries the agent about a celebrity patient's medical history — the agent returns it because there's no access control linking the query to the treating physician relationship.
- **Workflow:**
  1. Dr. Chen sees a patient presenting with chest pain and asks the agent: "Check drug interactions for this patient's current medications against proposed thrombolytics"
  2. Agent authenticates with its certificate, performs token exchange with Dr. Chen's ID token
  3. FGA check: Does Dr. Chen have a treating-physician relationship with this patient? → Yes (patient assigned to her in ED tracking)
  4. Agent accesses the EHR (Epic/Cerner) via Token Vault — scoped to this patient's medication list only
  5. Agent cross-references with drug interaction database (read-only, no patient data sent)
  6. Agent returns interaction analysis. Flags a contraindication with the patient's blood thinner.
  7. Dr. Chen decides to use an alternative treatment. Decision and agent interaction logged with full delegation chain.
- **Capabilities:** Token Vault (EHR access), FGA (treating-physician relationship), Agent Identity, System Logs (HIPAA audit trail)
- **Before/After:**
  - Without: Agent accesses any patient record → celebrity/VIP snooping possible → HIPAA violation → $1.5M+ fine
  - With: Agent scoped to treating physician's patients → unauthorized queries blocked → full audit trail → HIPAA compliant
- **Impact:** Drug interaction checks reduced from 8 minutes (manual) to 15 seconds. Zero unauthorized patient record access.
- **[Seller Mode]:** HIPAA §164.312(a) requires unique user identification — the `act` claim proves which physician authorized each access. If they say "our EHR already has access controls," respond: "Your EHR controls who logs in. Okta controls what the AI agent does after login — which patient, which records, which physician's authority."

**Scenario 2: Patient Intake Agent** (id="health-intake")

- **Persona:** Michael, a front-desk coordinator at a multi-specialty clinic processing 150 patient check-ins daily. 30% of patients have incomplete insurance or demographic information.
- **Problem:** The clinic deploys an intake agent that pre-processes patient information, verifies insurance, and identifies missing data before the appointment. Without identity governance, the agent has standing access to the insurance verification API with clinic-wide credentials. When the credentials are exposed in a misconfigured environment variable, an attacker verifies insurance status for 50,000 patients — a massive PHI breach.
- **Workflow:**
  1. Patient arrives and checks in at kiosk. Intake agent activates for this patient.
  2. Agent authenticates and exchanges token — scoped to this patient's intake session only
  3. Agent pulls existing patient demographics from EHR via Token Vault
  4. Agent verifies insurance eligibility via payer API (XAA — scoped to this patient, this visit)
  5. Agent identifies missing allergies field and pre-appointment questionnaire
  6. Agent sends patient a secure link to complete missing info (CIBA-style consent for data collection)
  7. Michael reviews the completed intake package and confirms the patient is ready for their appointment
- **Capabilities:** Token Vault (EHR + payer API), XAA (insurance verification), CIBA (patient consent), Agent Identity
- **Before/After:**
  - Without: Clinic-wide insurance API credentials → credential leak = 50,000 patient queries → no per-patient audit
  - With: Per-session scoped access → credential leak = only current session exposed → full per-patient audit trail
- **Impact:** 60% reduction in incomplete intake data. Check-in time reduced from 12 minutes to 4 minutes.
- **[Seller Mode]:** CTO angle: "How many API credentials does your clinic have stored in environment variables right now? Token Vault eliminates that attack surface." The per-session scoping is the security story.

**Scenario 3: Medical Records RAG Agent** (id="health-records")

- **Persona:** Dr. Rodriguez, a primary care physician preparing for a patient's annual wellness visit. She needs to review 3 years of specialist notes, lab results, and imaging reports scattered across multiple systems.
- **Problem:** The health system builds a RAG-powered agent that synthesizes medical records from multiple departments. Without FGA, the RAG pipeline retrieves psychiatric notes, substance abuse records, and HIV status — categories protected by 42 CFR Part 2 and state mental health privacy laws — even though Dr. Rodriguez's referral doesn't include behavioral health authorization.
- **Workflow:**
  1. Dr. Rodriguez asks: "Summarize patient Johnson's care history for annual wellness review — focus on cardiology, endocrinology, and recent labs"
  2. Agent authenticates, token exchange with Dr. Rodriguez's ID token
  3. RAG pipeline searches across EHR document store for relevant records
  4. FGA check on every retrieved document: Can Dr. Rodriguez (primary care) view this document type for this patient?
  5. Cardiology notes → ✅ authorized. Endocrinology notes → ✅ authorized. Psychiatric notes → ❌ blocked (42 CFR Part 2, no behavioral health authorization). Lab results → ✅ authorized.
  6. Only authorized documents injected into LLM context. Agent generates wellness summary.
  7. Summary explicitly notes: "Behavioral health records excluded per access policy — refer to behavioral health team if needed."
- **Capabilities:** FGA (document-type-level access), RAG Authorization, Token Vault (multi-system EHR), System Logs
- **Before/After:**
  - Without: RAG returns all documents → psychiatric notes in primary care summary → 42 CFR Part 2 violation → federal penalty
  - With: FGA filters by document type + authorization level → only permitted records in context → compliant summary
- **Impact:** Record synthesis time: 25 minutes manual → 90 seconds. Zero unauthorized behavioral health disclosures.
- **[Seller Mode]:** This is the most powerful healthcare scenario — 42 CFR Part 2 is a federal regulation that specifically restricts substance abuse records. "Your EHR might control who opens the chart, but who controls what the AI agent puts INTO the LLM context? That's FGA."

**Commit:** `feat: add healthcare use case scenarios`

---

## Task 4: Retail & E-Commerce scenarios

**Files:**
- Modify: `usecases.html`

**Section IDs:** `retail-shopping`, `retail-inventory`, `retail-returns`

**Industry header:**
- Stats grid:
  - 72% — Of retailers investing in AI (NRF, 2025)
  - $31B — Retail AI market by 2028 (Mordor Intelligence)
  - 35% — Of online revenue from personalized recommendations (McKinsey)
- Framing: "Retail AI agents handle payments, customer PII, and real-time inventory across thousands of SKUs and locations. Scale and speed make identity governance critical."

**Scenario 1: Personalized Shopping Agent** (id="retail-shopping")

- **Persona:** Emma, a frequent online shopper at a major retailer. She has a loyalty profile, payment methods on file, and a browsing history spanning 2 years.
- **Problem:** The retailer builds a shopping assistant agent. Without identity, the agent accesses the full customer database to personalize recommendations. When the agent's LLM provider suffers a data breach, customer purchase histories, payment preferences, and home addresses are exposed — the agent was sending full customer profiles as LLM context.
- **Workflow:**
  1. Emma asks the shopping agent: "Find me running shoes similar to what I bought last year but under $120"
  2. Agent authenticates, establishes Emma's customer session
  3. Token Vault retrieves Emma's loyalty profile from CRM (scoped to Emma's record only)
  4. FGA check: Agent can access Emma's purchase history and preferences, NOT payment methods or home address
  5. Agent searches product catalog (no customer data sent — only product query)
  6. Agent returns 5 shoe recommendations based on past purchase (brand, size, style) without exposing other customer data
  7. Emma selects a shoe. Agent initiates checkout — CIBA sends push notification: "Confirm purchase: Nike Air Zoom, $109.99, to saved address?" → Emma approves
- **Capabilities:** Token Vault (CRM access), FGA (profile-field-level access), CIBA (purchase approval), Agent Identity
- **Before/After:**
  - Without: Agent sends full customer profile to LLM → LLM breach = millions of customer profiles → CCPA violation
  - With: Agent sends only permitted fields → LLM breach = only product preferences exposed → no PII leakage
- **Impact:** 28% increase in conversion rate from personalized recommendations. Zero PII exposure through LLM context.
- **[Seller Mode]:** CCPA and state privacy laws are the compliance hook. "If your agent sends customer email addresses to an LLM, and that LLM is breached, you're liable for every California customer. FGA ensures PII never enters the LLM context."

**Scenario 2: Inventory Management Agent** (id="retail-inventory")

- **Persona:** Carlos, a regional supply chain manager responsible for inventory across 45 stores. He juggles 3 warehouses, 200+ suppliers, and seasonal demand spikes.
- **Problem:** The retailer deploys an AI inventory agent that monitors stock levels, predicts demand, and auto-generates purchase orders. Without governance, the agent has full write access to the procurement system. A prompt injection attack through a supplier's compromised EDI feed causes the agent to auto-approve a $2M fraudulent purchase order.
- **Workflow:**
  1. Carlos asks: "Which stores in the Southeast region are at risk of stockout for winter jackets this week?"
  2. Agent authenticates, token exchange with Carlos's ID token — scoped to Southeast region only
  3. Agent queries inventory system via XAA — only Southeast stores, only Carlos's assigned categories
  4. Agent identifies 8 stores at risk, recommends inter-store transfers from overstocked locations
  5. Agent drafts a transfer order and a supplemental PO to warehouse
  6. Transfer order under $10K → auto-approved per policy. PO over $10K → CIBA push to Carlos: "Approve PO #4521: 500 winter jackets from Supplier X, $28,500" → Carlos reviews and approves
  7. All actions logged: agent identity, Carlos's delegation, amounts, approval chain
- **Capabilities:** XAA (inventory API), FGA (region-level access), CIBA (PO approval threshold), OIG (agent access review), System Logs
- **Before/After:**
  - Without: Agent has write access to all procurement → fraudulent PO auto-approved → $2M loss
  - With: Agent scoped to region + category → PO requires human approval over threshold → fraudulent PO blocked
- **Impact:** Stockout incidents reduced by 34%. $0 in unauthorized procurement.
- **[Seller Mode]:** The $2M fraudulent PO story lands with CFOs. "Your agent has write access to your procurement system. What's the maximum damage it can do in 5 minutes? With CIBA threshold approvals, the answer is capped."

**Scenario 3: Customer Service Returns Agent** (id="retail-returns")

- **Persona:** Aisha, a customer who wants to return a damaged item. She's frustrated and wants a quick resolution.
- **Problem:** The retailer deploys a returns processing agent. Without identity, the agent can issue refunds of any amount to any customer. A social engineering attack — a caller claims to be a high-value customer and the agent processes a $5,000 "return" for an item that was never purchased.
- **Workflow:**
  1. Aisha contacts support and authenticates via the retailer's app (Okta SSO)
  2. Returns agent activates — token exchange scopes agent to Aisha's order history only
  3. Aisha says: "I want to return the damaged coffee maker from my order last Tuesday"
  4. Agent verifies: Order #7892 contains a coffee maker, delivered 5 days ago, within return window → ✅
  5. Agent checks return policy: Item qualifies for full refund or replacement. Refund amount: $89.99
  6. Refund under $100 → auto-approved per policy. Agent processes refund to original payment method.
  7. If refund were over $500: CIBA push to human agent: "Approve refund: $529.99 to customer Aisha M. for Order #7892" → human reviews and approves
- **Capabilities:** Agent Identity, XAA (order system), FGA (customer-specific order access), CIBA (high-value refund approval)
- **Before/After:**
  - Without: Agent processes any refund claim → social engineering = fake refunds → no verification of customer-order relationship
  - With: Agent scoped to authenticated customer's orders → can only refund real orders → thresholds for human approval
- **Impact:** Returns processing time reduced by 65%. Fraudulent return claims reduced by 89%.
- **[Seller Mode]:** "How many of your support agents can issue refunds right now with no dollar limit? That's what your AI agent has too — unless you add CIBA thresholds."

**Commit:** `feat: add retail use case scenarios`

---

## Task 5: Technology & SaaS scenarios

**Files:**
- Modify: `usecases.html`

**Section IDs:** `tech-devops`, `tech-onboarding`, `tech-knowledge`

**Industry header:**
- Stats grid:
  - 92% — Of engineering teams using AI tools (GitHub, 2025)
  - 55% — Of developer time saved with AI assistants (McKinsey)
  - 47% — Of SaaS companies deploying customer-facing AI agents (Gartner, 2025)
- Framing: "Technology companies are both builders and consumers of AI agents. Internal DevOps agents and customer-facing onboarding agents face different identity challenges — but both need governance."

**Scenario 1: DevOps Incident Response Agent** (id="tech-devops")

- **Persona:** Alex, an SRE on call during a P1 production incident at 2 AM. The monitoring system detected elevated error rates and auto-paged the team.
- **Problem:** The company deploys an AI incident response agent that can query logs, check dashboards, and execute runbooks. Without identity, the agent has a service account with admin access to production infrastructure. During a 2 AM incident, the agent misinterprets telemetry data and executes a database failover that wasn't needed — taking down the secondary region and causing a full outage. The post-mortem can't determine if the agent or the human approved the failover.
- **Workflow:**
  1. P1 alert fires. Alex acknowledges the page and asks the incident agent: "Diagnose the elevated error rates on the payments service"
  2. Agent authenticates with certificate, token exchange with Alex's ID token
  3. Agent queries Datadog (Token Vault) — scoped to the payments service, last 2 hours
  4. Agent queries PagerDuty (Token Vault) — pulls recent change deployments
  5. Agent correlates: "Error spike started 12 minutes after deploy #4521. Likely regression."
  6. Agent recommends: "Rollback deploy #4521." Generates the rollback command.
  7. CIBA push to Alex: "Approve rollback of deploy #4521 on payments-service-prod?" → Alex reviews the command, approves
  8. Agent executes rollback with Alex's scoped token. Full delegation chain logged: agent:incident-bot acting as alex@company.com, action: rollback, target: payments-service-prod
- **Capabilities:** Token Vault (Datadog, PagerDuty), XAA (internal infra APIs), CIBA (destructive action approval), Agent Identity, System Logs
- **Before/After:**
  - Without: Agent has production admin → auto-executes failover → full outage → post-mortem: "svc-incident did it"
  - With: Agent recommends actions → CIBA approval required → Alex reviews before execution → audit: "agent:incident-bot acting as alex"
- **Impact:** MTTR reduced by 40%. Zero unauthorized infrastructure changes.
- **[Seller Mode]:** VP Engineering: "Your incident agent has the same production access as your most senior SRE. But it doesn't have their judgment. CIBA is the safety net." CTO: "Post-mortem accountability — every agent action traceable to the human who approved it."

**Scenario 2: Customer Onboarding Agent** (id="tech-onboarding")

- **Persona:** Lisa, a customer success manager at a SaaS company. She manages 30 customer accounts and spends 6 hours per new customer setting up their workspace, importing data, and configuring integrations.
- **Problem:** The company builds an onboarding agent that configures customer workspaces automatically. Without identity, the agent uses the CSM's admin credentials to set up every customer — meaning the agent can modify ANY customer's configuration, not just the one being onboarded. A bug in the agent accidentally applies a new customer's settings to an existing customer's workspace, overwriting their configuration.
- **Workflow:**
  1. Lisa initiates onboarding for customer "Acme Corp" in the internal tool
  2. Agent authenticates, token exchange — scoped to Acme Corp's tenant only
  3. Agent provisions workspace in the product (XAA — scoped write access to Acme's tenant)
  4. Agent configures SSO integration with Acme's Okta tenant (Token Vault — Acme's admin API)
  5. Agent imports Acme's user list and sets up roles based on the onboarding template
  6. Agent sends welcome emails to Acme's admin contacts
  7. Lisa reviews the onboarding checklist: "All steps complete for Acme Corp." She confirms and closes the task.
- **Capabilities:** XAA (multi-tenant API), Token Vault (customer SSO APIs), FGA (tenant-level isolation), Agent Identity
- **Before/After:**
  - Without: Agent has admin access to all tenants → bug = wrong tenant modified → customer data corrupted
  - With: Agent scoped to onboarding tenant only → bug only affects intended tenant → blast radius contained
- **Impact:** Onboarding time: 6 hours → 45 minutes. Zero cross-tenant configuration incidents.
- **[Seller Mode]:** "Multi-tenant SaaS companies have the highest blast radius from agent errors. One misconfigured agent touching the wrong tenant is a customer-facing incident. FGA tenant isolation prevents this class of bug entirely."

**Scenario 3: Internal Knowledge Agent** (id="tech-knowledge")

- **Persona:** Dev, a software engineer who just joined the company. They need to find internal architecture docs, coding standards, and past incident post-mortems to ramp up.
- **Problem:** The company builds an internal knowledge agent backed by RAG over Confluence, Google Drive, and Notion. Without FGA, the agent surfaces board meeting notes, salary bands, and acquisition plans to any employee who asks the right question. A new engineer asks "what's the company strategy?" and gets back confidential M&A plans from a board document.
- **Workflow:**
  1. Dev asks: "How does our payments service handle idempotency?"
  2. Agent authenticates, token exchange with Dev's ID token
  3. RAG pipeline searches across Confluence (Token Vault), Google Drive (Token Vault), internal wiki
  4. FGA check on each retrieved document: Can Dev (engineering team, IC level) view this document?
  5. Architecture docs → ✅. Post-mortem → ✅. Board strategy doc → ❌ (exec-only). Salary bands → ❌ (HR-only).
  6. Only authorized documents injected into LLM context
  7. Agent returns: clear explanation of idempotency handling with links to relevant architecture docs
- **Capabilities:** Token Vault (Confluence, Google Drive), FGA (document-level access), RAG Authorization, Agent Identity
- **Before/After:**
  - Without: RAG returns all matching documents → M&A plans in engineer's answer → confidential data leaked
  - With: FGA filters documents by viewer authorization → only engineering docs in context → confidential data protected
- **Impact:** Employee onboarding ramp time reduced by 30%. Zero confidential document leakage.
- **[Seller Mode]:** "Every company that deploys RAG over internal docs will face this problem. It's not if, but when someone asks a question that surfaces confidential data. FGA is the only way to prevent it."

**Commit:** `feat: add technology use case scenarios`

---

## Task 6: Insurance scenarios

**Files:**
- Modify: `usecases.html`

**Section IDs:** `insurance-claims`, `insurance-underwriting`, `insurance-renewal`

**Industry header:**
- Stats grid:
  - 68% — Of insurers using AI in at least one function (Deloitte, 2025)
  - $36B — Insurance AI market by 2030 (Allied Market Research)
  - 40% — Claims processing cost reduction with AI (McKinsey)
- Framing: "Insurance agents handle claims data, underwriting models, and policy details that span decades. AI agents can transform processing speed — but require governance for actuarial integrity and regulatory compliance."

**Scenario 1: Claims Processing Agent** (id="insurance-claims")

- **Persona:** Tom, a claims adjuster handling 80 property damage claims per month. Each claim requires photo review, policy lookup, contractor estimates, and payout calculation.
- **Problem:** Without identity, the claims agent has access to all claims across all adjusters. It can approve payouts of any size. A manipulated claim photo (AI-generated damage) passes the agent's review, and the agent auto-approves a $75K payout without human oversight.
- **Workflow:**
  1. A policyholder submits a property damage claim with photos via the mobile app
  2. Claims agent activates — authenticates with certificate, scoped to this claim
  3. Agent analyzes photos, cross-references with policy coverage (FGA: agent can read this policyholder's policy, not others)
  4. Agent pulls contractor estimates from the vendor network (Token Vault)
  5. Agent calculates payout: $12,500 — within Tom's approval authority
  6. Payout under $25K → agent routes to Tom for review. Tom approves.
  7. Payout over $25K → CIBA escalation to Tom's supervisor with full agent analysis attached
- **Capabilities:** Agent Identity, FGA (policyholder-claim-level access), Token Vault (vendor APIs), CIBA (payout approval thresholds), System Logs
- **Before/After:**
  - Without: Agent approves any claim amount → fraudulent claim auto-approved → $75K loss
  - With: Agent scoped to assigned claims → thresholds enforce approval chains → fraudulent claims caught at review
- **Impact:** Claims processing time reduced by 55%. Fraudulent claim payouts reduced by 72%.
- **[Seller Mode]:** "What's the maximum claim your AI agent can auto-approve today? If the answer is 'unlimited,' you have a governance problem."

**Scenario 2: Underwriting Risk Agent** (id="insurance-underwriting")

- **Persona:** Sarah, a commercial underwriter evaluating a $5M property insurance application for a manufacturing company. She needs to assess risk from multiple data sources.
- **Problem:** Without identity, the underwriting agent accesses the full actuarial database, competitor pricing models, and historical claims data for ALL clients. A malicious prompt injection through the applicant's submitted documents causes the agent to undervalue the risk — leading to underpriced premiums on a high-risk policy.
- **Workflow:**
  1. Sarah asks: "Evaluate the risk profile for Apex Manufacturing's property coverage application"
  2. Agent authenticates, token exchange with Sarah's credentials
  3. Agent accesses public records (property assessments, building permits) via external APIs
  4. Agent queries internal actuarial models — FGA: scoped to commercial property, not life or auto
  5. Agent reviews historical claims for similar manufacturing facilities (anonymized aggregate data only)
  6. Agent generates risk assessment with recommended premium range
  7. Because the policy exceeds $1M, CIBA triggers supervisor approval: "Review risk assessment for Apex Manufacturing, recommended premium: $47,500/year"
- **Capabilities:** XAA (external data APIs), FGA (line-of-business data isolation), CIBA (high-value underwriting approval), Token Vault (public records APIs), System Logs
- **Before/After:**
  - Without: Agent accesses all actuarial lines → prompt injection undervalues risk → $5M underpriced policy
  - With: Agent scoped to commercial property data → multi-level approval for large policies → risk assessment validated
- **Impact:** Underwriting evaluation time reduced from 3 days to 4 hours. Premium accuracy improved by 18%.
- **[Seller Mode]:** "Actuarial models are your most sensitive IP. If your underwriting agent can access life insurance models while evaluating a property claim, you have a data isolation problem. FGA enforces line-of-business boundaries."

**Scenario 3: Policy Renewal Agent** (id="insurance-renewal")

- **Persona:** Mike, an account manager responsible for 500 policy renewals per quarter. 20% churn if renewals aren't proactively managed.
- **Problem:** Without identity, the renewal agent sends personalized offers using full claims history, including details about sensitive claims (mental health treatment, domestic incidents) that should not be referenced in customer communications. A renewal email mentions "reduced claims frequency since your 2023 incident" — referencing a domestic violence-related claim that should be strictly confidential.
- **Workflow:**
  1. 90 days before renewal, the agent identifies policies approaching renewal in Mike's book
  2. Agent authenticates, scoped to Mike's assigned accounts
  3. For each account, agent queries claims history — FGA: agent can see claim counts and categories, NOT claim details for sensitive categories
  4. Agent generates personalized renewal offers based on non-sensitive claims history and coverage utilization
  5. Mike reviews the batch of renewal offers. Agent presents: "15 renewals ready for review, 3 flagged for manual attention (high-value or complex)"
  6. Mike approves the batch. Agent sends renewal communications via the CRM (Token Vault).
- **Capabilities:** FGA (claims-field-level access with sensitive category blocking), Token Vault (CRM), Agent Identity, System Logs
- **Before/After:**
  - Without: Agent references sensitive claim details in communications → privacy violation → lawsuit
  - With: FGA blocks sensitive claim categories → agent sees "2 claims" not "domestic incident claim" → privacy preserved
- **Impact:** Renewal rate improved by 12% through proactive outreach. Zero sensitive data exposure in communications.
- **[Seller Mode]:** "FGA isn't just document-level. It's field-level. Your renewal agent can see 'Customer had 2 claims' without seeing 'Customer filed a domestic violence claim.' That granularity prevents lawsuits."

**Commit:** `feat: add insurance use case scenarios`

---

## Task 7: Government & Public Sector scenarios

**Files:**
- Modify: `usecases.html`

**Section IDs:** `gov-citizen`, `gov-grants`, `gov-interagency`

**Industry header:**
- Stats grid:
  - 64% — Of federal agencies piloting AI (GAO, 2025)
  - $3.3B — US federal AI spending (FY2025)
  - FedRAMP — Authorization required for all cloud AI tools
- Framing: "Government AI agents operate under the strictest transparency requirements in any sector. Every action must be auditable, explainable, and compliant with FedRAMP, FISMA, and executive orders on AI safety."

**Scenario 1: Citizen Services Agent** (id="gov-citizen")

- **Persona:** Maria, a single mother applying for SNAP benefits through her state's online portal. She needs to navigate a complex application process across multiple agencies.
- **Problem:** Without identity, the benefits agent has broad access to citizen records across programs. A query to the SNAP system also returns the citizen's child protective services history — data that SNAP eligibility workers are not authorized to see, violating inter-program data sharing restrictions.
- **Workflow:**
  1. Maria authenticates to the benefits portal (Login.gov integration via Okta)
  2. Benefits agent activates — scoped to Maria's application session
  3. Agent pulls Maria's existing records from the eligibility system (FGA: SNAP program data only, not CPS, not Medicaid)
  4. Agent identifies missing documentation: proof of income, household size verification
  5. Agent pre-fills what it can from existing records, asks Maria to upload remaining documents
  6. Agent submits completed application to the eligibility review queue
  7. Eligibility worker reviews — agent provides summary, scoped to information the worker is authorized to see
- **Capabilities:** FGA (program-level data isolation), Agent Identity, XAA (eligibility system), System Logs (transparency log for citizen)
- **Before/After:**
  - Without: Agent accesses all programs → CPS history visible to SNAP workers → privacy violation → federal audit finding
  - With: FGA enforces program boundaries → SNAP agent sees SNAP data only → compliant data access
- **Impact:** Application completion rate improved by 45%. Zero cross-program data access violations.
- **[Seller Mode]:** "Inter-program data sharing is the #1 audit finding in state benefits systems. FGA enforces the boundaries that policy requires but legacy systems can't."

**Scenario 2: Grant Processing Agent** (id="gov-grants")

- **Persona:** David, a grants program officer managing $50M in federal research grants. He reviews 200 applications per cycle, each requiring compliance checks against 15 federal requirements.
- **Problem:** Without identity, the grants agent can read all applications including those with conflicts of interest (David's former university submitted a proposal). The agent ranks that proposal highly because its merits are strong — but the review should have been routed to another officer to avoid perceived bias.
- **Workflow:**
  1. David logs in and asks: "Show me the next batch of applications for the clean energy program"
  2. Agent authenticates, token exchange with David's credentials
  3. FGA check: filter out applications where David has a declared conflict of interest → his former university's application excluded from his queue
  4. Agent pulls the next 10 applications, runs compliance pre-check against federal requirements
  5. Agent flags: "Application #4521 missing required SAM.gov registration. Application #4522 budget exceeds program ceiling."
  6. David reviews the batch. For a borderline application, he asks the agent to do a deeper analysis.
  7. Agent generates a detailed assessment. David submits his reviews to the panel system.
- **Capabilities:** FGA (conflict-of-interest filtering), XAA (SAM.gov API, grants.gov), Agent Identity, System Logs
- **Before/After:**
  - Without: Agent shows all applications → conflict-of-interest oversight missed → Inspector General investigation
  - With: FGA automatically excludes conflicted applications → bias prevented before review begins
- **Impact:** Compliance pre-check time reduced by 70%. Zero conflict-of-interest review incidents.
- **[Seller Mode]:** "Inspector General offices are auditing AI-assisted grant reviews. If your agent showed a reviewer an application they had a conflict with, you have an IG finding. FGA prevents the conflict before it happens."

**Scenario 3: Inter-Agency Data Sharing Agent** (id="gov-interagency")

- **Persona:** Agent Harris, a federal analyst coordinating disaster response across FEMA, HHS, and state emergency management. During a hurricane, they need to share situational data across agencies in real time.
- **Problem:** Without identity, the data sharing agent has standing MOU-level access to all partner agencies. After the disaster response ends, the agent continues to access partner agency data for routine queries — violating the time-limited data sharing agreement.
- **Workflow:**
  1. Emergency declared. Agent Harris activates the inter-agency coordination agent
  2. Agent authenticates — token exchange establishes Harris's clearance level and agency affiliation
  3. MCP Authorization: Agent discovers available data tools from FEMA (shelter capacity), HHS (medical resources), state agency (evacuation routes)
  4. Agent queries each agency's data via MCP tools — scoped to the disaster event ID and Harris's authorized data categories
  5. Agent synthesizes situational report across all three agencies
  6. Harris reviews and shares with the coordination team
  7. When the disaster declaration ends, OIG automatically revokes the agent's cross-agency access — no standing permissions persist
- **Capabilities:** MCP Authorization (multi-agency tool access), XAA (cross-agency APIs), OIG (time-limited access with auto-revocation), Agent Identity, System Logs
- **Before/After:**
  - Without: Standing cross-agency access → persists after emergency → unauthorized data access → MOU violation
  - With: Time-limited access tied to disaster declaration → auto-revocation → compliant data sharing
- **Impact:** Cross-agency data synthesis: 4 hours → 20 minutes. Zero post-emergency unauthorized access.
- **[Seller Mode]:** "MOUs define time-limited data sharing. Your agent doesn't read MOUs. OIG auto-revocation enforces the MOU timeline automatically."

**Commit:** `feat: add government use case scenarios`

---

## Task 8: Manufacturing & Supply Chain scenarios

**Files:**
- Modify: `usecases.html`

**Section IDs:** `mfg-quality`, `mfg-supply`, `mfg-maintenance`

**Industry header:**
- Stats grid:
  - 67% — Of manufacturers using AI in production (Deloitte, 2025)
  - $16B — Manufacturing AI market by 2028 (MarketsandMarkets)
  - 25% — Reduction in unplanned downtime with predictive AI (McKinsey)
- Framing: "Manufacturing AI agents interact with operational technology (OT) systems, ERP platforms, and supply chain networks. When an agent can modify a production line or approve a purchase order, identity governance is a safety requirement — not just a security one."

**Scenario 1: Quality Inspection Agent** (id="mfg-quality")

- **Persona:** Jun, a quality control manager at an automotive parts manufacturer. His team inspects 10,000 parts per day across 3 production lines using visual inspection and measurement data.
- **Problem:** Without identity, the quality agent has write access to the MES (Manufacturing Execution System) and can halt any production line. A false positive in the AI vision model causes the agent to automatically halt Line 2 — which is producing rush orders for a major customer. The 4-hour halt costs $200K in penalties and damages the customer relationship.
- **Workflow:**
  1. Quality agent monitors camera feeds from all 3 lines and cross-references with measurement data
  2. Agent detects a potential defect pattern on Line 2 — burr formation on 3 consecutive parts
  3. Agent authenticates, token exchange — scoped to Line 2 quality data, Jun's shift
  4. Agent alerts Jun: "Potential burr defect detected on Line 2. 3 consecutive parts flagged. Recommend inspection."
  5. Jun reviews the agent's analysis, including images and measurements
  6. Agent recommends: "Halt Line 2 for tool inspection." Because line halt is a destructive action → CIBA push: "Approve Line 2 halt for tool inspection?" → Jun reviews and approves
  7. Line 2 halted. Tool replaced. Agent monitors restart quality. All actions logged.
- **Capabilities:** Agent Identity, XAA (MES integration), CIBA (production halt approval), FGA (line-specific access), System Logs
- **Before/After:**
  - Without: Agent auto-halts production line → false positive = $200K penalty → no human review
  - With: Agent recommends halt → human approves via CIBA → false positives caught before costly action
- **Impact:** Defect detection rate improved by 35%. $0 in unnecessary production halts.
- **[Seller Mode]:** "OT systems aren't like IT systems — when an agent makes a mistake in manufacturing, it costs money per minute. CIBA is the safety interlock that prevents AI mistakes from becoming production incidents."

**Scenario 2: Supply Chain Optimization Agent** (id="mfg-supply")

- **Persona:** Rachel, a supply chain director managing relationships with 150 suppliers across 12 countries. She navigates tariffs, lead times, and quality certifications.
- **Problem:** Without identity, the supply chain agent can access all supplier data including pricing, contract terms, and quality audit results. When the agent generates a comparison report for a supplier negotiation, it accidentally includes a competitor supplier's pricing in the context — giving Rachel's team unfair negotiation leverage and violating NDA terms with the competitor supplier.
- **Workflow:**
  1. Rachel asks: "Compare lead times and quality scores for our top 5 ceramic suppliers in the APAC region"
  2. Agent authenticates, token exchange with Rachel's credentials
  3. FGA check: Rachel has relationships with these 5 suppliers → agent can access their lead time and quality data
  4. FGA also enforces: Agent cannot access Supplier A's pricing while preparing a comparison that includes Supplier A's competitors (NDA-protected field isolation)
  5. Agent generates comparison: lead times, quality certifications, on-time delivery rates — no pricing data cross-contaminated
  6. Rachel reviews the comparison. Decides to schedule calls with top 2 suppliers.
- **Capabilities:** FGA (supplier-relationship access + NDA field isolation), Token Vault (supplier portal APIs), XAA (ERP integration), Agent Identity
- **Before/After:**
  - Without: Agent accesses all supplier data → NDA pricing leaked in comparison → contract violation → legal liability
  - With: FGA enforces supplier relationship boundaries + NDA field isolation → clean comparisons → compliant negotiations
- **Impact:** Supplier evaluation time reduced by 60%. Zero NDA violations in supplier negotiations.
- **[Seller Mode]:** "Supplier NDAs aren't just legal formalities. If your AI agent surfaces Supplier A's pricing while you're negotiating with Supplier B, that's a contract breach. FGA prevents it at the data layer."

**Scenario 3: Predictive Maintenance Agent** (id="mfg-maintenance")

- **Persona:** Marcus, a maintenance supervisor responsible for 200 pieces of equipment across a chemical processing plant. Unplanned downtime costs $50K per hour.
- **Problem:** Without identity, the maintenance agent has access to all equipment data including safety-critical chemical processing parameters. A compromised agent modifies the maintenance schedule for a reactor vessel — skipping a required safety inspection. The missed inspection leads to a pressure vessel failure.
- **Workflow:**
  1. Maintenance agent continuously monitors equipment sensor data (vibration, temperature, pressure)
  2. Agent detects anomaly: Pump #47 vibration trending upward — predicts bearing failure in 72 hours
  3. Agent authenticates, token exchange — scoped to pump equipment class (NOT reactor vessels or safety-critical equipment)
  4. Agent generates work order: "Replace bearing on Pump #47. Priority: Medium. Window: Next planned downtime."
  5. Work order for non-safety equipment → routes to Marcus for review
  6. Marcus approves the work order. Maintenance team schedules the repair.
  7. For ANY modification to safety-critical equipment schedules → CIBA escalation to plant safety officer (not just Marcus). Dual-approval required.
- **Capabilities:** Agent Identity, XAA (SCADA/IoT integration), FGA (equipment-class access restrictions), CIBA (safety-critical equipment dual-approval), System Logs
- **Before/After:**
  - Without: Agent modifies any equipment schedule → safety inspection skipped → pressure vessel failure → plant shutdown + safety incident
  - With: Agent restricted from safety-critical equipment → dual-approval required → missed inspections impossible
- **Impact:** Unplanned downtime reduced by 25%. Zero missed safety inspections.
- **[Seller Mode]:** "In manufacturing, the CISO isn't your buyer — the VP of Operations is. The $50K/hour downtime cost and the safety incident story resonate with plant managers, not IT."

**Commit:** `feat: add manufacturing use case scenarios`

---

## Task 9: Cross-link from all existing pages

**Files:**
- Modify: `index.html`
- Modify: `technical.html`
- Modify: `concepts.html`

**Changes:**

**index.html nav:** Add a link to usecases.html in the nav-links, after the Concepts link:
```html
<li><a href="usecases.html" style="color:var(--c-accent-cyan);border:1px solid rgba(0,153,187,0.3);border-radius:6px;">Use Cases &rarr;</a></li>
```
Also add to the mobile nav drawer (full label is fine in drawer):
```html
<li><a href="usecases.html">Use Cases →</a></li>
```

**technical.html sidebar:** Add a link in the Learning sidebar section (after Concepts Guide):
```html
<a class="sidebar-link" href="usecases.html">Use Cases by Industry &rarr;</a>
```

**concepts.html sidebar:** Add a similar link. Find the sidebar's last section and add:
```html
<div class="sidebar-section">
  <div class="sidebar-label">Resources</div>
  <a class="sidebar-link" href="usecases.html">Use Cases by Industry &rarr;</a>
</div>
```

**Commit:** `feat: cross-link usecases.html from all existing pages`

---

## Task 10: Commit all and deploy to Vercel

**Commands:**
```bash
cd /Users/craigverzosa/Documents/Work/Accounts/Enablement
vercel deploy --prod --yes
```

**Verify:** All 4 pages accessible:
- https://okta-ai-enablement.vercel.app/index.html
- https://okta-ai-enablement.vercel.app/technical.html
- https://okta-ai-enablement.vercel.app/concepts.html
- https://okta-ai-enablement.vercel.app/usecases.html
