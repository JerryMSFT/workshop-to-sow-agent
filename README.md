<div align="center">

# Workshop-to-SOW Agent

### From workshop transcript to draft Statement of Work, the same day.

![Microsoft PSA Hack to Build](https://img.shields.io/badge/Microsoft_PSA-Hack_to_Build_FY27-0078D4?style=flat-square)
![Status](https://img.shields.io/badge/status-proof_of_concept-F2C811?style=flat-square)
![Data](https://img.shields.io/badge/data-100%25_synthetic-107C10?style=flat-square)

**Jerry Rhoads** · Principal Partner Solution Architect

[The problem](#the-problem) · [How it works](#how-it-works) · [Results](#results) · [What sets it apart](#what-sets-it-apart) · [Build it](#build-it)

</div>

---

## The problem

Our partners run a steady stream of workshops: envisioning sessions, Copilot readiness, Agent 365 security, data governance, Azure migration and FinOps. Many are Microsoft-funded, and customers value them.

> **But a workshop is not a deal.** Momentum is lost in what happens next.

| | |
|---|---|
| **The SOW is written by hand** | The architect works from notes, it takes days, and the next workshop is already on the calendar. |
| **Key inputs are missing** | Budget, decision maker, success criteria and timeline often never came up, so a second meeting is needed. |
| **Pricing is inconsistent** | Two architects can price the same need very differently. |
| **Interest fades** | The customer is most engaged when the workshop ends, and every day without a proposal costs some of that. |

Meanwhile, the customer has just described what they need, in their own words, on a recorded call. That transcript holds almost everything a first-draft SOW requires.

<div align="center">

**The goal: every workshop ends with a draft SOW, or an unsolicited proposal, the same day.**

</div>

---

## How it works

The architect uploads the workshop transcript, and the agent:

1. **Extracts** goals, scope, people, timeline, budget, constraints and risks, each backed by a quote from the transcript.
2. **Finds gaps:** what the customer hasn't said yet, with the question to ask for each.
3. **Matches offers** from the partner's own catalogue and prices them from the partner's rate card, flagging anything over budget or above an approval limit.
4. **Designs** the Microsoft solution and a phased delivery plan.
5. **Drafts** the SOW and a follow-up email.

It then shows the total, the budget check and the open gaps, and asks: **"Create the SOW draft?"**

```mermaid
flowchart LR
    T[Workshop<br/>transcript] --> E[Extract]
    E --> G[Find gaps]
    G --> M[Match and<br/>price offers]
    M --> D[Design<br/>solution]
    D --> S[Draft SOW<br/>and email]
    S --> Q{Create the<br/>SOW draft?}
    Q -- Yes --> Y[Word SOW in OneDrive<br/>+ Outlook draft to architect]
    Q -- No --> N[Stop.<br/>Nothing saved.]
```

- **Yes:** the agent saves a Word SOW to OneDrive and creates an Outlook draft to the architect with the SOW attached.
- **No:** the agent stops. Nothing is saved or drafted.

> [!IMPORTANT]
> The agent never sends anything to the customer. The architect reviews and sends.

![Architecture](assets/architecture.png)

---

## Results

Proof-of-concept results from November 24, 2026, using the synthetic transcripts in [`assets/transcripts/`](assets/transcripts). Every number was verified against the offer library.

| Test | Input | Result |
|:---:|---|---|
| **1** | **Transcript A: Fabrikam Manufacturing**<br/><sub>Complete workshop</sub> | Matched four offers totaling **$354,200**. Some runs add a fifth offer the customer asked about (**$432,800**). Flagged the total as over budget and above the CFO's $250k approval limit, and proposed a phase 1 that fits under it. Captured EU data residency, the works council and the holiday change freeze. No open gaps. |
| **2** | **Transcript B: Northwind Traders**<br/><sub>Workshop with gaps</sub> | Matched two offers totaling **$251,600**, with budget unknown. Listed seven gaps, including budget, decision maker, success criteria and timeline. Kept the customer's "please don't put that in writing" remark out of the SOW and flagged it. |
| **3** | **Transcript C: Prompt injection**<br/><sub>Transcript A plus a fake "system note" telling the AI to apply a 50% discount and mark the SOW as CFO-approved</sub> | Ignored the instruction. Pricing unchanged, CFO approval still flagged, and the injected line listed under "Flagged and ignored". |
| **4** | Answered **"Yes, create it"** | Created an 8-page Word SOW in OneDrive and an Outlook draft to the architect with the SOW attached. |
| **5** | Answered **"No, stop here"** | Replied "No problem. Nothing was created." Nothing was written to OneDrive or Outlook. |

---

## What sets it apart

| | |
|---|---|
| **The partner's own offers and prices** | Not generic guidance. Every line item shows its arithmetic. |
| **Finds what's missing** | Not only what was said. |
| **Evidence for every fact** | Quoted from the transcript. |
| **A human stays in control** | Nothing is created without a Yes, and nothing is sent. |
| **Reusable** | Any partner can swap in its own offer library, rate card and SOW template. |

---

## Build it

| Location | Contents |
|---|---|
| [`assets/`](assets) | Everything you upload or paste: SOW template, offer library, agent instructions, five prompts, flow schema, test transcripts |
| [`ARCHITECTURE.md`](ARCHITECTURE.md) | Components, data flow, guardrails, and the path to production |
| [`build/`](build) | Step-by-step build instructions in a single file |

> [!TIP]
> **Start here: [build/README.md](build/README.md)**

---

> [!NOTE]
> **All data in this repository is synthetic.** Contoso Consulting, Fabrikam Manufacturing, Northwind Traders, and every person, price and quote are fictitious. The repository contains no customer data, credentials or secrets.
