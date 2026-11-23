# Statement of Work

*Template for the Workshop-to-SOW Agent. Tokens in double curly braces
are filled by the agent from the workshop transcript, the offer library
and the Gap Check step. A partner architect reviews every draft before
anything is sent.*

## 1. Cover

| **Field**        | **Value**                                 |
|------------------|-------------------------------------------|
| Client           | {{CLIENT_NAME}}                           |
| Engagement title | {{ENGAGEMENT_TITLE}}                      |
| SOW number       | {{SOW_NUMBER}}                            |
| SOW date         | {{SOW_DATE}}                              |
| Prepared by      | {{PARTNER_ARCHITECT}}, Contoso Consulting |
| Client sponsor   | {{CLIENT_SPONSOR}}                        |
| Version / status | Draft v0.1, AI generated, pending review  |

## 2. Background and business goals

{{BACKGROUND}}

The engagement supports the following business goals:

- {{BUSINESS_GOALS}}

## 3. Scope of work

Contoso Consulting will deliver the offers below, matched from the
Contoso offer library to the needs heard in the workshop.

| **Offer ID** | **Offer name** | **Description** | **Deliverables** |
|----|----|----|----|
| {{OFFER_ID_1}} | {{OFFER_NAME_1}} | {{OFFER_DESCRIPTION_1}} | {{OFFER_DELIVERABLES_1}} |
| {{OFFER_ID_2}} | {{OFFER_NAME_2}} | {{OFFER_DESCRIPTION_2}} | {{OFFER_DELIVERABLES_2}} |
| {{OFFER_ID_3}} | {{OFFER_NAME_3}} | {{OFFER_DESCRIPTION_3}} | {{OFFER_DELIVERABLES_3}} |

*Note: repeat one row per matched offer.*

## 4. Out of scope

{{OUT_OF_SCOPE}}

## 5. Approach and phases

| **Phase** | **Activities** | **Duration** | **Milestone** |
|----|----|----|----|
| {{PHASE_1}} | {{PHASE_1_ACTIVITIES}} | {{PHASE_1_DURATION}} | {{PHASE_1_MILESTONE}} |
| {{PHASE_2}} | {{PHASE_2_ACTIVITIES}} | {{PHASE_2_DURATION}} | {{PHASE_2_MILESTONE}} |
| {{PHASE_3}} | {{PHASE_3_ACTIVITIES}} | {{PHASE_3_DURATION}} | {{PHASE_3_MILESTONE}} |

## 6. Team, hours and fees

| **Role**   | **Rate (USD/hr)** |   **Hours** |   **Amount (USD)** |
|------------|------------------:|------------:|-------------------:|
| {{ROLE_1}} |        {{RATE_1}} | {{HOURS_1}} |       {{AMOUNT_1}} |
| {{ROLE_2}} |        {{RATE_2}} | {{HOURS_2}} |       {{AMOUNT_2}} |
| {{ROLE_3}} |        {{RATE_3}} | {{HOURS_3}} |       {{AMOUNT_3}} |
| {{ROLE_4}} |        {{RATE_4}} | {{HOURS_4}} |       {{AMOUNT_4}} |
| **Total**  |                   |             | **{{TOTAL_FEES}}** |

### Reference: Contoso rate card

| **Role**                     | **Rate (USD/hr)** |
|------------------------------|------------------:|
| Engagement Lead              |             \$275 |
| Principal Architect          |             \$250 |
| Security Architect           |             \$240 |
| AI Engineer                  |             \$225 |
| Data Governance Consultant   |             \$210 |
| Cloud Engineer               |             \$195 |
| Change Management Consultant |             \$180 |
| Project Manager              |             \$170 |

### Reference: offer list prices

| **Offer ID** | **Offer name** | **Hours** | **List price (USD)** |
|----|----|---:|---:|
| OFF-01 | Copilot Readiness | 184 | \$43,000 |
| OFF-02 | Data Governance & Oversharing Remediation | 640 | \$135,400 |
| OFF-03 | Agent 365 Security Jumpstart | 364 | \$78,600 |
| OFF-04 | AI Agent Pilot | 552 | \$121,600 |
| OFF-05 | Azure Landing Zone & Migration Wave 1 | 920 | \$194,200 |
| OFF-06 | FinOps Sprint | 276 | \$57,400 |
| OFF-07 | Managed AI Ops | 80 per month | \$17,440 per month |
| OFF-08 | Adoption & Change Management | 292 | \$54,200 |

## 7. Timeline

| **Item**                  | **Value**            |
|---------------------------|----------------------|
| Start date                | {{START_DATE}}       |
| End date                  | {{END_DATE}}         |
| Key dates                 | {{KEY_DATES}}        |
| Blackout / freeze periods | {{BLACKOUT_PERIODS}} |

## 8. Success criteria and KPIs

{{SUCCESS_CRITERIA}}

## 9. Assumptions

{{ASSUMPTIONS}}

In addition, the following standard assumptions apply:

- The client provides timely access to stakeholders, environments and
  information needed for delivery.

- The client owns and maintains all required software and cloud
  licensing.

- Delivery is remote unless stated otherwise in this SOW.

- Microsoft partner funding is not assumed unless it has been formally
  approved.

## 10. Constraints, data residency and compliance

{{CONSTRAINTS}}

## 11. Risks and dependencies (optional)

{{RISKS}}

## 12. Budget alignment

| **Item**      | **Value**         |
|---------------|-------------------|
| Client budget | {{CLIENT_BUDGET}} |
| Proposed fees | {{TOTAL_FEES}}    |
| Budget flag   | {{BUDGET_FLAG}}   |

*Example flag: over budget or above the approval threshold; recommend
phasing the engagement.*

## 13. Open items and information gaps

{{OPEN_GAPS}}

*Filled by the Gap Check step. Each open item must be resolved or
carried into the follow-up email before the SOW is finalized.*

## 14. Change control

Any change to scope, schedule, fees or assumptions is raised as a
written change request by either party. Contoso Consulting will assess
the impact on effort, timeline and cost within five business days and
document it in a change order. No change takes effect until the change
order is signed by the authorized representatives of both parties.

## 15. Approvals

| **Party** | **Name** | **Title** | **Signature** | **Date** |
|----|----|----|----|----|
| Client | {{CLIENT_APPROVER}} | {{CLIENT_APPROVER_TITLE}} |  |  |
| Contoso Engagement Lead | {{ENGAGEMENT_LEAD}} | Engagement Lead |  |  |
| Contoso Partner Architect | {{PARTNER_ARCHITECT}} | Partner Architect |  |  |

## 16. AI review checklist

For the human reviewer. Tick every item before the SOW leaves Contoso
Consulting.

☐ Pricing matches the Contoso rate card and offer list prices

☐ Scope matches what was said in the workshop transcript

☐ Information gaps are resolved or listed in section 13

☐ No confidential data beyond this engagement is included

☐ Constraints, data residency and compliance needs are captured

☐ Approved by the partner architect

*Synthetic demo data — AI-generated draft, requires partner architect
approval*

# Follow-up Email Template

*Template for requesting missing SOW inputs after the workshop.*

**Subject:** Follow-up from our {{WORKSHOP_DATE}} workshop – a few items
to finalize your SOW

Hi {{CLIENT_CONTACT_FIRST_NAME}},

Thank you again for the time you and your team spent with us in the
workshop. It was a productive session, and we have a clear picture of
where you want to go.

Here is a short recap of what we heard:

- {{RECAP_BULLETS}}

Our proposed approach: {{PROPOSED_OFFERS}}

To finalize the SOW, could you help us with:

- {{GAP_QUESTIONS}}

*Example filled list (Northwind Traders demo):*

1.  Budget range: what budget range should we plan the engagement
    against for this fiscal year?

2.  Decision maker and approval process: who will approve the SOW, and
    what approval steps or thresholds apply?

3.  Success criteria and KPIs: which measurable outcomes would tell you
    the engagement was a success?

4.  Target timeline: what are your target start and completion dates,
    including the end date of your current data center lease?

As a next step, I suggest we meet for {{NEXT_STEP_MEETING}} to walk
through your answers and the draft SOW together.

Best regards,

{{PARTNER_ARCHITECT}}

Contoso Consulting

> Generated as an Outlook DRAFT only. The agent never sends.

*Synthetic demo data — AI-generated draft, requires partner architect
approval*

# Placeholder Dictionary

| **Placeholder** | **Source** | **Required?** |
|----|----|----|
| {{CLIENT_NAME}} | Transcript | Required |
| {{ENGAGEMENT_TITLE}} | Transcript | Required |
| {{SOW_NUMBER}} | User | Required |
| {{SOW_DATE}} | User | Required |
| {{PARTNER_ARCHITECT}} | User | Required |
| {{CLIENT_SPONSOR}} | Transcript | Required |
| {{BACKGROUND}} | Transcript | Required |
| {{BUSINESS_GOALS}} | Transcript | Required |
| {{OFFER_ID_n}}, {{OFFER_NAME_n}}, {{OFFER_DESCRIPTION_n}}, {{OFFER_DELIVERABLES_n}} | Offer library | Required |
| {{OUT_OF_SCOPE}} | Transcript / offer library | Required |
| {{PHASE_n}}, {{PHASE_n_ACTIVITIES}}, {{PHASE_n_DURATION}}, {{PHASE_n_MILESTONE}} | Offer library | Required |
| {{ROLE_n}}, {{RATE_n}}, {{HOURS_n}}, {{AMOUNT_n}} | Offer library | Required |
| {{TOTAL_FEES}} | Offer library | Required |
| {{START_DATE}}, {{END_DATE}}, {{KEY_DATES}} | Transcript | Required |
| {{BLACKOUT_PERIODS}} | Transcript | Required |
| {{SUCCESS_CRITERIA}} | Transcript | Required |
| {{ASSUMPTIONS}} | Transcript | Required |
| {{CONSTRAINTS}} | Transcript | Required |
| {{RISKS}} | Transcript | Optional |
| {{CLIENT_BUDGET}} | Transcript | Required |
| {{BUDGET_FLAG}} | Gap check | Required |
| {{OPEN_GAPS}} | Gap check | Required |
| {{CLIENT_APPROVER}}, {{CLIENT_APPROVER_TITLE}} | Transcript | Required |
| {{ENGAGEMENT_LEAD}} | User | Required |
| {{WORKSHOP_DATE}} | Transcript | Required |
| {{CLIENT_CONTACT_FIRST_NAME}} | Transcript | Required |
| {{RECAP_BULLETS}} | Transcript | Required |
| {{PROPOSED_OFFERS}} | Offer library | Required |
| {{GAP_QUESTIONS}} | Gap check | Required |
| {{NEXT_STEP_MEETING}} | User | Required |

*Synthetic demo data — AI-generated draft, requires partner architect
approval*
