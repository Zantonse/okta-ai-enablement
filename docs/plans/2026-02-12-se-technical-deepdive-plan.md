# SE Technical Deep-Dive Page — Implementation Plan

> **For Claude:** REQUIRED SUB-SKILL: Use frontend-design to implement this as a single HTML file.

**Goal:** Build `technical.html` — an internal-only SE technical reference page for Okta AI products with protocol deep-dives, demo scripts, code snippets, competitive war room, and interactive navigation.

**Architecture:** Single HTML file with inline CSS and vanilla JS. Shares font stack and color system with `index.html`. Fixed sidebar nav, collapsible sections, copy-to-clipboard, text search. Cross-linked with main site.

**Tech Stack:** HTML5, inline CSS (custom properties), vanilla JavaScript, Google Fonts (Instrument Serif, DM Sans, JetBrains Mono)

---

## Task 1: Create technical.html with base structure, CSS, and sidebar nav

**Files:**
- Create: `/Users/craigverzosa/Documents/Work/Accounts/Enablement/technical.html`

Build the complete page shell:
- Same `<head>` as index.html (charset, viewport, Google Fonts)
- Title: "Okta AI — SE Technical Reference"
- CSS custom properties matching index.html color system
- `INTERNAL — SE USE ONLY` banner at top
- Fixed left sidebar nav (240px wide) with section jump links
- IntersectionObserver for active-section highlighting in sidebar
- Main content area with right offset for sidebar
- Responsive: sidebar collapses to hamburger on narrow screens
- Link back to main site in header

## Task 2: Platform Architecture Overview section

Build section 2 with:
- Two-column layout: Okta Platform (blue) vs Auth0 for AI Agents (purple)
- **Okta Platform (Enterprise IT):** Discover → Register → Protect → Govern lifecycle
  - Discover: ISPM, SAM Plugin for shadow agent detection
  - Register: Universal Directory for agent identities
  - Protect: XAA, Token Vault, FGA, CIBA, OPA
  - Govern: OIG, ITP, System Logs, Universal Logout
- **Auth0 for AI Agents (Builders):** User Auth, Token Vault, Async Auth, FGA
- "Two Motions" positioning callout: Auth0 for builders shipping AI apps, Okta for enterprises governing agents
- CSS diagram showing both products mapping to the agentic AI architecture

## Task 3: Cross-App Access (XAA) deep-dive

Following the consistent section template:

**What it is:** Enterprise-managed token exchange protocol. Okta sits as the identity broker between requesting apps/agents and resource apps. Uses ID-JAG (Identity Assertion JWT) to carry user context through agent-to-resource interactions.

**How it works — Sequence diagram (CSS):**
1. User authenticates to Agent App via Okta SSO
2. Agent App receives ID token from Okta
3. Agent needs to access Resource App on user's behalf
4. Agent App requests ID-JAG from Okta (presents ID token + agent identity)
5. Okta validates: Is this agent registered? Is this user assigned? Is policy met?
6. Okta issues ID-JAG (short-lived JWT with user context + agent context + scopes)
7. Agent presents ID-JAG + client credentials to Resource App's custom auth server
8. Custom auth server validates ID-JAG, issues scoped access token
9. Agent accesses Resource App API with scoped token

**Key technical details:**
- Based on OAuth 2.0 Token Exchange (RFC 8693)
- ID-JAG contains: sub (user), azp (agent app), aud (resource), scope, exp (short TTL)
- Requires: Agent registered in Okta Universal Directory, user-agent assignment, auth policy
- Token chain: ID Token → ID-JAG → Scoped Access Token (each step is validated)

**Demo script:**
- Step 1: Show agent in Universal Directory with identity type
- Step 2: Show user-to-agent assignment
- Step 3: Walk through token exchange in dev tools / Postman
- Step 4: Show System Log entry for the exchange (audit trail)
- Talking point: "Every hop is authenticated, authorized, and logged"

**Code snippet:** curl for token exchange request + example ID-JAG JWT payload (decoded)

**Objections:**
- "We can just use service accounts" → Service accounts have static creds, no user context, no audit trail linking action to user. XAA preserves user identity through the chain.
- "Microsoft Entra does token exchange too" → Entra's token exchange is intra-Microsoft only. XAA works across any SaaS ecosystem with custom auth servers.
- "This adds latency" → ID-JAG issuance is <50ms. Caching strategies available. Security-performance tradeoff is minimal.

## Task 4: Token Vault deep-dive

**What it is:** Auth0's secure storage for third-party OAuth tokens. Agents never touch raw tokens — Auth0 handles storage, refresh, and exchange. Eliminates token sprawl and the risk of long-lived tokens stored in agent code.

**How it works — Sequence diagram:**
1. User logs into GenAI App via Auth0 Universal Login
2. Auth0 authenticates user + connects to upstream IdP (Google, GitHub, Slack, etc.)
3. Upstream IdP returns access token + refresh token
4. Auth0 stores tokens encrypted in Token Vault
5. Agent needs to call third-party API (e.g., Google Calendar)
6. Agent calls `auth0.getAccessTokenForConnection({ connection: 'google-oauth2' })`
7. Auth0 checks: Is token valid? If expired → auto-refresh using stored refresh token
8. Auth0 returns fresh, scoped access token to agent
9. Agent calls third-party API with token

**Key technical details:**
- Federated Token Exchange: Auth0 mediates between agent and upstream IdPs
- Automatic token refresh — no custom OAuth logic needed
- Tokens scoped per-provider, per-user, per-connection
- CIAL (Client-Initiated Account Linking) for adding new provider connections post-login

**Demo script:**
- Step 1: Show the Auth0 dashboard connections page (Google, GitHub, Slack)
- Step 2: Show code: `getAccessTokenForConnection` — "This is the entire integration"
- Step 3: Show token flow in network tab
- Step 4: Show CIAL flow — user links additional identity from settings page
- Talking point: "Zero token handling code. No refresh logic. Tokens never touch your infra."

**Code snippets:**
1. Token Vault retrieval (TypeScript):
```typescript
import { google } from "googleapis";
import { auth0 } from "./auth0";

export async function getGoogleAuth() {
  const { token } = await auth0.getAccessTokenForConnection({
    connection: 'google-oauth2'
  });
  const googleOAuthClient = new google.auth.OAuth2();
  googleOAuthClient.setCredentials({ access_token: token });
  return googleOAuthClient;
}
```

2. CIAL flow initiation

**Objections:**
- "We already store tokens in our database" → Are they encrypted at rest? Auto-refreshed? Per-user scoped? Revocable? Token Vault handles all of this out of the box.
- "What about enterprise IdPs?" → Works with SAML, OIDC, social providers. CIAL supports linking enterprise + social identities to a single profile.

## Task 5: Async Authorization (CIBA) deep-dive

**What it is:** CIBA (Client-Initiated Backchannel Authentication) enables human-in-the-loop approval for sensitive agent actions. The agent requests authorization, the user gets a push notification, approves/denies, and the agent proceeds. No browser redirect needed.

**How it works — Sequence diagram:**
1. AI Agent determines it needs to perform a sensitive action (e.g., buy stock, transfer funds)
2. Agent sends CIBA request to Auth0: `POST /bc-authorize` with user_id, binding_message, scopes, RAR details
3. Auth0 sends push notification to user's device: "Your AI agent wants to buy 100 shares of ZEKO. Approve?"
4. User reviews context (binding_message + RAR data) and approves or denies
5. Auth0 notifies agent via CIBA ping mode (webhook callback) — no polling needed
6. If approved: Agent receives access token with requested scopes
7. Agent executes the action

**Key technical details:**
- Based on OpenID Connect CIBA (RFC 9126) + OAuth 2.0 RAR (RFC 9396)
- **Ping mode** (preferred): Auth0 calls agent's registered callback URL when user responds
- **Poll mode** (fallback): Agent polls token endpoint
- **Binding message**: Human-readable string displayed to user — "Do you want to buy ${qty} shares of ${ticker}?"
- **RAR**: Rich Authorization Requests carry structured data (action type, amount, resource) — not just scopes
- Timeout handling: Configurable expiry (default 300s), agent gets `expired_token` error
- Fallback: Secure link-based approval for users without push capability

**Demo script:**
- Step 1: Show CIBA configuration in Auth0 dashboard
- Step 2: Show code: `auth0AI.withAsyncUserConfirmation()` wrapper
- Step 3: Trigger the flow — show push notification on phone
- Step 4: Approve → show agent receiving the callback and executing
- Talking point: "The agent pauses, the human decides, the agent proceeds. Standards-based, not custom-built."

**Code snippet:**
```typescript
const auth0AI = new Auth0AI();

export const buy = (context: Context) => {
  const withAsyncAuthorization = auth0AI.withAsyncUserConfirmation({
    userID: context.userID,
    bindingMessage: async ({ ticker, qty }) =>
      `Do you want to buy ${qty} shares of ${ticker}?`,
    scopes: ["openid", "stock:trade"],
    audience: process.env["AUDIENCE"]!,
    onAuthorizationRequest: "block",
    onUnauthorized: async (e: Error) => {
      if (e instanceof AccessDeniedInterrupt) {
        return "The user has denied the request";
      }
      return e.message;
    },
  });
};
```

**Objections:**
- "We can just use a confirmation dialog in the UI" → Agents run in the background. There may be no UI. CIBA works across channels (push, SMS, email link).
- "What about timeout?" → Configurable. Agent gets clean error response and can retry or escalate.
- "How is this different from regular MFA?" → MFA verifies identity. CIBA authorizes a specific action with rich context (RAR). The user sees exactly what the agent wants to do.

## Task 6: FGA for RAG deep-dive

**What it is:** Auth0 Fine-Grained Authorization (FGA) enforces relationship-based access control at retrieval time in RAG pipelines. Before an LLM sees a document, FGA checks: "Does this user have permission to view this document?" Uses the OpenFGA standard (ReBAC).

**How it works — RAG pipeline flow:**
1. User asks: "Show my last appointment record"
2. AI Agent sends query to vector store retriever (LangChain/LlamaIndex)
3. Vector store returns candidate documents based on semantic similarity
4. **FGA Retriever intercepts**: For each document, queries Auth0 FGA: `check(user:john_doe, doc:medical_record_333, viewer)`
5. FGA evaluates relationship graph: john_doe → patient → owns → medical_record_333 ✓
6. Unauthorized documents filtered out
7. Only permitted documents passed to LLM for response generation
8. LLM generates answer using only authorized context

**Key technical details:**
- **ReBAC model** (Relationship-Based Access Control):
```
model schema 1.1
type user
type patient
  relations
    define _self: [user]
type ai_agent
  relations
    define authorized_by: [patient]
type medical_record
  relations
    define can_update: owner
    define can_view: owner or delegated
    define delegated: [ai_agent#authorized_by]
    define owner: [patient]
```
- **Tuples** (permission data): `(user:john_doe, medical_record:mr_333, owner)`, `(user:john_doe, ai_agent:ai_111, authorized_by)`
- **FGARetriever**: Auth0 AI SDK component wrapping vector store retriever + FGA check
- Sub-millisecond authorization checks at scale
- Works with LangChain, Vercel AI, LlamaIndex, custom pipelines

**Demo script:**
- Step 1: Show FGA model in Auth0 FGA playground (relationship graph visualization)
- Step 2: Show tuples: which users own which documents
- Step 3: Show code: FGARetriever wrapping a LangChain vector store retriever
- Step 4: Query as User A → sees their docs. Query as User B → different docs.
- Talking point: "The LLM never sees documents the user shouldn't access. Authorization happens before generation."

**Code snippet:**
```typescript
const user = "user1";
const documents = await readDocuments();
const vectorStore = await MemoryVectorStore.fromDocuments(
  documents,
  new OpenAIEmbeddings({ model: "text-embedding-3-small" })
);
const retriever = FGARetriever.create({
  retriever: vectorStore.asRetriever(),
  buildQuery: (doc) => ({
    user: `user:${user}`,
    object: `doc:${doc.metadata.id}`,
    relation: "viewer",
  }),
});
const fgaTool = retriever.asJoinedStringTool();
const retrievalAgent = RetrievalAgent.create([fgaTool]);
const answer = await retrievalAgent.query("Show my last appointment record");
```

**Objections:**
- "We use RBAC already" → RBAC is too coarse for document-level access. FGA operates at object-level with relationship graphs. A user's role doesn't determine which specific documents they can see — their relationship to those documents does.
- "This will slow down retrieval" → FGA checks are sub-millisecond. Batch checks available. The alternative (post-generation filtering) is worse — the LLM already saw the data.
- "We'll build our own" → FGA is an open standard (OpenFGA). Auth0's implementation is managed, scaled, and integrates natively with their auth stack.

## Task 7: Agent Discovery & Governance deep-dive

**What it is:** Okta's platform capabilities for discovering shadow AI agents, registering them as managed identities, enforcing access policies, and monitoring their behavior. Covers the full lifecycle: Discover → Register → Protect → Govern.

**How it works — Four phases:**

**Discover:**
- ISPM (Identity Security Posture Management): Scans for non-human identities including AI agents
- SAM Plugin: Detects SaaS-embedded agents (Salesforce Einstein, ServiceNow Virtual Agent, etc.)
- Shows: which agents exist, what they access, what credentials they use

**Register:**
- Universal Directory: Agent gets a first-class identity (not a service account)
- Identity type: "AI Agent" with metadata (owner, purpose, risk level)
- User-to-agent assignment: Which humans can this agent act on behalf of?

**Protect:**
- Auth policies: MFA step-up for high-risk agent actions
- XAA: Scoped token exchange (see section 3)
- OPA (Okta Privileged Access): JIT elevated access for agents needing admin rights

**Govern:**
- OIG (Okta Identity Governance): Certification campaigns for agent access
- System Logs: Every agent action logged with user context
- Universal Logout: Revoke agent sessions across all connected apps
- ITP (Identity Threat Protection): Anomaly detection for agent behavior

**Demo script:**
- Step 1: Show ISPM discovery view — "Here are 47 non-human identities we found"
- Step 2: Show agent in Universal Directory with AI Agent identity type
- Step 3: Show auth policy: "Agents from this group require step-up approval for financial data"
- Step 4: Show System Log filtered to agent events
- Talking point: "Today, most customers can't answer 'How many AI agents do we have?' Okta can."

**Objections:**
- "We track agents in a spreadsheet" → Manual tracking can't keep up with agent sprawl. ISPM auto-discovers agents you don't know about.
- "We use service accounts for agents" → Service accounts have no governance lifecycle, no certification, no user-context. Agent identities in Okta have full lifecycle management.
- "Astrix does discovery too" → Astrix only does discovery (ISPM). No runtime enforcement, no auth, no governance lifecycle. Okta covers the full chain.

## Task 8: Integration Matrix section

Build a responsive table:

| Framework | User Auth | Token Vault | Async Auth (CIBA) | FGA for RAG | SDK |
|-----------|-----------|-------------|--------------------|----|-----|
| LangChain | ✓ | ✓ | ✓ | ✓ | JS, Python |
| LlamaIndex | ✓ | ✓ | — | ✓ | Python |
| Vercel AI | ✓ | ✓ | ✓ | ✓ | JS |
| CrewAI | ✓ | ✓ | ✓ | — | Python |
| AutoGen | ✓ | — | — | — | Python |
| Custom/REST | ✓ | ✓ | ✓ | ✓ | JS, Python, Go |

Plus quickstart links column and sample apps column.

Available Quickstarts from auth0.com/ai:
- User Authentication (Next.js, FastAPI)
- Call Your APIs on User's Behalf (Next.js, Vercel AI)
- Call Other's APIs on User's Behalf (Next.js, Vercel AI, FastAPI)
- Asynchronous Authorization (Node.js, Vercel AI, LangChain)
- Authorization for RAG (Node.js, Vercel AI, LangChain)

## Task 9: Competitive War Room section

Detailed technical rebuttals per competitor:

**Microsoft Entra:**
- Has: Basic token exchange within Microsoft ecosystem, Entra ID Governance
- Lacks: Cross-ecosystem token exchange (XAA), agent-native identity types, CIBA for agents, FGA/ReBAC
- Key gap: Token exchange only works within Microsoft apps. No support for agent-to-resource across non-Microsoft SaaS.
- When they say: "We have Copilot Studio" → Respond: "Copilot Studio builds agents. Okta governs ALL agents, not just Microsoft's."
- When they say: "Entra does token exchange" → Respond: "Only within Microsoft's ecosystem. XAA works across any OAuth-compliant resource."

**Ping Identity:**
- Has: Strong legacy IAM, API security (PingGateway)
- Lacks: Agent-native discovery (no ISPM), no token vault, no FGA, limited async auth
- Key gap: Built for human users, not AI agents. No first-class agent identity concept.
- When they say: "PingOne is cloud-native" → Respond: "Cloud-native IAM ≠ agent-native IAM. Where's agent discovery? Where's the token vault?"

**Astrix:**
- Has: Good ISPM/NHI discovery
- Lacks: Everything after discovery — no auth, no token management, no governance, no developer SDKs
- Key gap: Detection without enforcement. They tell you the problem; Okta solves it.
- When they say: "We found 10,000 non-human identities" → Respond: "Great. Now what? Okta discovers AND manages the full lifecycle."

**ServiceNow:**
- Has: IT workflow automation, ITSM integration
- Lacks: Identity-native controls, no token exchange, no FGA, no CIBA
- Key gap: IT workflow tool, not an identity platform. Can't enforce least-privilege at the protocol level.
- When they say: "We manage AI agent workflows" → Respond: "Workflow ≠ identity. Who authenticates the agent? Who governs its access? Who revokes it?"

## Task 10: Troubleshooting & Edge Cases section

Collapsible items:

**Common Gotchas:**
- Token expiration during long-running agent tasks: Use refresh tokens + Token Vault auto-refresh. For XAA, ID-JAG has short TTL — agent must re-request.
- FGA rule conflicts: Use FGA playground to visualize relationship graph. Check tuple ordering. Test with `check` API before deploying.
- CIBA timeout tuning: Default 300s. Financial services may need longer. Configure per-action.
- Multi-tenant agent isolation: Use Auth0 Organizations for tenant separation. Each org gets its own connection + FGA model.

**Debugging Toolkit:**
- Okta System Log queries for agent events: `actor.type eq "AI_AGENT"`, filter by agent ID
- Auth0 Dashboard > Logs: Filter by client_id for agent app, look for `fcx` (federated token exchange) events
- FGA Playground: Visual relationship graph, test `check` and `list-objects` queries
- CLI tools: `okta-cli`, `auth0-cli`, `fga-cli` for scripted testing

**Edge Cases:**
- Hybrid cloud: Agent in AWS, resources in Azure. XAA works across clouds — Okta is the broker.
- Regulated industries (HIPAA, SOX): FGA provides document-level audit trail. CIBA provides explicit consent for regulated actions. System Logs provide compliance evidence.
- Agent-to-agent chaining: Agent A calls Agent B. Each hop gets its own ID-JAG with narrowing scopes. Full chain in System Log.

## Task 11: Quick Reference Cards and search functionality

Build:
- Condensed summary cards for each of the 5 features (compact design, key info only)
- Key stats for conversation drops:
  - 580M access paths (1,000 agents × 58 tools × 10,000 users)
  - 5.8B policy decisions (×10 conversations per user)
  - 80% of breaches involve compromised credentials (Verizon DBIR)
  - 51% of companies already use AI agents (Pager Duty)
  - 44% have no governance in place (The Times)
  - 82% of orgs plan to integrate agents by 2026 (Capgemini)
  - $100B+ GenAI market by 2032, 30%+ CAGR (Fortune Business Insights)
- Simple text search: JS that highlights matching text across all sections
- Copy-to-clipboard on all code blocks

## Task 12: Add cross-links between index.html and technical.html

**Files:**
- Modify: `/Users/craigverzosa/Documents/Work/Accounts/Enablement/index.html`
- In the nav section, add a "Technical Deep-Dive →" link pointing to `technical.html`

## Task 13: Test in browser

- Start local HTTP server
- Verify all sections render
- Test sidebar navigation (active highlighting)
- Test collapsible sections (expand/collapse)
- Test search functionality
- Test copy-to-clipboard
- Test responsive behavior
- Verify cross-links between pages work
