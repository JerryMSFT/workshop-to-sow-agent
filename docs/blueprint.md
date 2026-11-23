# Workshop-to-SOW Agent
Concept, business value, technical design, and future roadmap
*Prepared on October 19, 2026 by Jerry Rhoads, Principal Partner Solution Architect*

**Executive summary**
Microsoft partners run discovery workshops to scope AI, security, data and Azure projects, but turning a workshop into a scoped, priced Statement of Work (SOW) is slow, manual and inconsistent. The Workshop-to-SOW Agent takes over after the workshop: it reads the Microsoft Teams transcript and notes, extracts goals, scope and constraints, flags missing information and drafts a customer follow-up, matches needs to the partner's own offer library, proposes a reference architecture, and generates a draft SOW in Word for the partner architect to review and approve. The target outcome is moving from workshop to a reviewable proposal in hours instead of days, with consistent scope and pricing, so more workshops convert into pipeline and co-sell. Because each partner loads its own offers and rates once, the agent is reusable IP that partners can adopt as a product.

# 1. Overview
The solution has two parts:
- Before / during the workshop – a structured Workshop Template with discovery questions per solution area (AI and agents, security, data, Azure migration) so the conversation captures what a SOW needs: business goals, current state, scope, constraints, timeline, budget range, decision makers and success criteria. The workshop runs normally in Microsoft Teams with transcription on.
- After the workshop – an AI agent built on Azure AI Foundry Agent Service (or Copilot Studio) with Azure OpenAI runs five steps: (1) Extract, (2) Gap Check, (3) Match Offers, (4) Design, (5) Draft SOW. A partner architect reviews and approves every output; the agent never sends anything to a customer.

# 2. Problem Clarity and Customer Relevance
*Is the problem real, specific and worth solving for a named customer/partner scenario?*
*Clear problem statement, named persona/segment, evidence of pain — not a generic idea.*
**Problem statement:** After a discovery workshop, partner presales architects must manually turn notes and recollections into a scoped and priced SOW. This typically takes days, key inputs (budget, success criteria, constraints, decision makers) are often missed and require extra follow-up meetings, and scope and pricing vary from architect to architect. Momentum is lost between the workshop and the proposal.
**Persona / segment:** Presales and solution architects at Microsoft partners — global and regional system integrators (SIs), managed service providers (MSPs) and consulting firms — who run workshop-led motions (for example AI readiness, Agent 365 security, data governance, Azure migration and FinOps).
**Evidence of pain:** As a Partner Solution Architect, I see workshop-led activation as a core FY27 motion with partners; the gap between a successful workshop and a signed SOW is a recurring point where opportunities stall and co-sell pipeline is delayed.

# 3. Business Impact and Value Potential
*Size and credibility of the business or customer outcome (ACR, efficiency, revenue, adoption)*
*Quantified or credibly estimated outcome, clear beneficiary, line of sight to pipeline/co-sell.*
## Business impact
- Faster time-to-proposal: target a reviewable draft SOW within hours of the workshop instead of days.
- Higher workshop-to-pipeline conversion: proposals arrive while customer interest is high, and gap-check follow-ups close missing information quickly.
- Consistent scoping and pricing across a partner's architects, reducing under-scoped or over-priced proposals.
- Line of sight to Microsoft outcomes: faster partner SOWs accelerate co-sell opportunities, Azure consumption (ACR) and Copilot / agent adoption.
## Value proposition
- For partner architects: less manual document work, more time with customers.
- For partner leadership: repeatable, governed proposals that reflect the firm's standard offers and rates.
- For customers: a faster, clearer proposal with explicit scope, assumptions and success criteria.
- For Microsoft: workshop investments convert to pipeline faster across the partner ecosystem.

# 4. Innovation and Differentiation
*Originality vs. what already exists in-market or in Microsoft's portfolio.*
*Genuinely innovative approach or meaningful improvement over existing tooling*
General-purpose tools such as Microsoft 365 Copilot can summarize a meeting or help write a document, but they do not know a partner's offer catalog, delivery phases, roles or rate card, and they do not check a conversation against what a SOW requires. The Workshop-to-SOW Agent is differentiated by: (1) a structured workshop-to-proposal workflow rather than a single prompt; (2) a gap check that identifies missing SOW inputs and drafts the customer follow-up; (3) retrieval over the partner's own offer library so scope, phases, effort and pricing reflect the partner's real offers; and (4) a built-in human-in-the-loop approval step. To my knowledge, there is no Microsoft offering today that turns a partner–customer workshop into a priced SOW grounded in the partner's own offers.

# 5. Technical Feasibility and Architecture Approach
*Originality vs. what already exists in-market or in Microsoft's portfolio.*
***Final Solution Requirement** - Technical decisions, appropriate technologies, scalability, path beyond the hackathon*
*Genuinely innovative approach or meaningful improvement over existing tooling*
***Final Solution Requirement** Deliberate, justified design; scales; realistic route to a real product.*
The solution uses managed Microsoft services so it can be built as a working proof of concept within the November build phase and scaled into a partner product. The agent is orchestrated in Azure AI Foundry Agent Service (Copilot Studio is an alternative for low-code partners), grounded with retrieval over the partner offer library, and outputs documents through Power Automate and Microsoft Graph. Each partner deploys in its own tenant, which keeps data isolated and lets the solution scale partner by partner.
## Core architectural components.
- Workshop Template – discovery question bank per solution area.
- Microsoft Teams + Microsoft Graph – meeting transcript and notes as input.
- Azure AI Foundry Agent Service / Copilot Studio with Azure OpenAI – extraction, gap analysis, offer matching, architecture recommendation and SOW drafting.
- Offer Library (SharePoint list / Excel) – offers, phases, roles, hours, rate card, prerequisites, assumptions and exclusions.
- Azure AI Search – indexes the offer library for retrieval-augmented generation (RAG).
- Power Automate + Microsoft Graph – generates the Word SOW from a template, saves it to SharePoint, creates the Outlook follow-up draft and posts a Teams notification.
[FIGURE: workshop-to-sow-architecture.png — caption "Figure 1 – Workshop-to-SOW Agent architecture"]
## Reference flow
1. Partner runs the workshop in Teams using the Workshop Template, with transcription on.
2. The agent retrieves the transcript and notes through Microsoft Graph.
3. Extract: goals, pain points, current state, scope, constraints, timeline, stakeholders.
4. Gap Check: compares extracted facts to required SOW inputs; flags gaps and drafts a follow-up email.
5. Match Offers: retrieves matching offers from the offer library via Azure AI Search.
6. Design: proposes a high-level reference architecture for the matched offers.
7. Draft SOW: Power Automate generates the Word SOW (phases, roles, hours, pricing, assumptions) in SharePoint.
8. Human review: the partner architect edits and approves the SOW and follow-up before anything is sent.

# 6. Security, Governance, and Reliability Considerations
*Data handling, grounding, identity/access, privacy, RAI considerations*
*Sensitive data protected, access controlled, no secrets exposed, RAI risks acknowledged.*
- Identity and access: Microsoft Entra ID authentication; least-privilege Graph permissions limited to the meetings and sites the agent needs.
- Data handling: single-tenant deployment per partner; customer workshop data stays in the partner's Microsoft 365 / Azure tenant; no secrets in code (Azure Key Vault and managed identities).
- Grounding: SOW content is grounded in the transcript and the partner's offer library; the agent cites the offer used and marks assumptions and gaps rather than inventing them.
- Protection: Microsoft Purview sensitivity labels applied to generated SOWs; Azure AI Content Safety on inputs and outputs.
- Responsible AI: human-in-the-loop approval — the agent drafts, a person approves, and nothing is sent automatically; customers are informed that the workshop is transcribed.
- Auditability and reliability: actions logged for audit; failed steps retry and notify the architect.
- Demo data: the hackathon demo uses only synthetic data (fictional partner "Contoso Consulting" and fictional customer "Fabrikam") — no real customer information.

# 7. Microsoft Technology Alignment
*Fit with Azure AI, Copilot Studio, Foundry, agentic patterns and FY27 priorities.*
*Right Microsoft stack for the job, used purposefully — not bolted on.*
The solution uses the Microsoft stack for what each service does best: Azure AI Foundry Agent Service and Azure OpenAI for agentic reasoning; Azure AI Search for retrieval over partner IP; Microsoft Teams and Microsoft Graph for workshop input; SharePoint, Word and Outlook for outputs; Power Automate for document workflow; and Entra ID, Purview and Azure AI Content Safety for governance. It follows an agentic pattern (a multi-step agent with tools and human approval) and supports FY27 priorities around partner-led AI transformation, workshop-led activation and co-sell pipeline growth.

# 7. Reusability and GTM Potential
*Can this become reusable IP, a packaged offer or a marketplace solution?*
*Credible path to repeatable IP / GTM beyond a one-off demo*
- Reusable IP: the agent, workshop templates and SOW template are generic; each partner loads its own offer library and rate card once.
- Packaged offer: partners can use it internally to speed up presales, or package "workshop + proposal in 24 hours" as a differentiated offer.
- Marketplace path: can be published as a deployable solution template (Copilot Studio solution or Azure deployment template) on Microsoft Marketplace.
- Scale: starter workshop templates for Microsoft solution plays (AI readiness, security, data, migration, FinOps) let new partners start quickly.

# 8. Suggested Pilot
- Phase 1 (Build phase, Nov 2–20): working proof of concept with a synthetic partner offer library (Contoso Consulting, 6–8 offers) and two scripted workshop transcripts — one complete and one with gaps — to demonstrate end-to-end SOW generation and the gap check.
- Phase 2: pilot with one or two partners from my portfolio running real workshop-led motions, using their own offer library, with partner architects reviewing every output.
- Success measures: time from workshop to draft SOW, architect edit effort, number of follow-up gaps caught, and workshop-to-opportunity conversion.

# 9. Conclusion
The Workshop-to-SOW Agent closes the gap between a good workshop and a signed proposal. It is simple to demonstrate, grounded in each partner's own offers, governed by design with a human approving every output, and reusable across the partner ecosystem — turning workshops into pipeline faster for partners, customers and Microsoft.
