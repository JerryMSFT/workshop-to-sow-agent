# Formulas and expressions

Copy from here. In Copilot Studio, a formula must show an **fx** chip. If it shows as plain text, it's sent literally.

## Agent flow "Get transcript text"

| Where | Expression |
|---|---|
| Compose "Transcript text" > Inputs (fx) | `base64ToString(triggerBody()?['file']?['contentBytes'])` |
| Respond to the agent > TranscriptText | fx > Dynamic content > *Transcript text* **Outputs** |

## Agent flow "Fill SOW"

| Where | Expression |
|---|---|
| Parse JSON > Content | fx > Dynamic content > **SOWJson** |
| Parse JSON > Schema | `copilot-studio/flows/fill-sow-parse-json-schema.json` |
| Populate > PROPOSED_FEES | Body **TOTAL_FEES** |
| Populate > PARTNER_ARCHITECT_SIGNATURE | Body **PARTNER_ARCHITECT** |
| Create file > File Name (fx) | `concat(body('Parse_JSON')?['CLIENT_NAME'],' - SOW draft - ',formatDateTime(utcNow(),'yyyy-MM-dd'),'.docx')` |
| Create file > File Content | fx > Dynamic content > **Microsoft Word document** |
| Draft an email > Attachment Name (fx) | `body('Create_file')?['Name']` |
| Respond to the agent > SOWLink | **Web URL** from Create share link, or fx `body('Create_share_link')?['WebUrl']` |

## Topic "Run Workshop-to-SOW"

| Where | Value |
|---|---|
| Get transcript text > TranscriptFile (… > Formula) | `{ contentBytes: Topic.TranscriptFile, name: "transcript.txt" }` |
| Step5 Draft SOW > TodayDate (tool page, … > Formula) | `Text(Today(), "mmmm d, yyyy")` |
| Step5 Draft SOW > ArchitectName | `Susan Architect` (plain text, demo). Production: `System.User.DisplayName` (untested) |
| Fill SOW > SOWJson | `Global.Step5Out.text` (… > Custom) |
| Fill SOW > ArchitectEmail | `User.Email` (… > System) |

Summary message (toolbar **fx**):

```
"Total fees: " & Text(ParseJSON(Global.Step5Out.text).TOTAL_FEES) & Char(10) & "Budget check: " & Text(ParseJSON(Global.Step5Out.text).BUDGET_FLAG) & Char(10) & Char(10) & "Open gaps:" & Char(10) & Text(ParseJSON(Global.Step5Out.text).OPEN_GAPS)
```

Yes-branch message (toolbar **fx**):

```
"Your SOW draft is ready (DRAFT - review before sending): " & Topic.SOWLink & Char(10) & Char(10) & "An Outlook draft with the SOW attached is in your Drafts folder."
```

## Prompt inputs (tool page > Inputs > Fill using: Custom value)

| Prompt | Input | Value |
|---|---|---|
| Step1 Extract | TranscriptText | `Global.TranscriptText` |
| Step2 Gap check | ExtractJson | `Global.Step1Out.text` |
| Step3 Match offers | ExtractJson | `Global.Step1Out.text` |
| Step4 Design | ExtractJson | `Global.Step1Out.text` |
| Step4 Design | OffersJson | `Global.Step3Out.text` |
| Step5 Draft SOW | ExtractJson | `Global.Step1Out.text` |
| Step5 Draft SOW | GapJson | `Global.Step2Out.text` |
| Step5 Draft SOW | OffersJson | `Global.Step3Out.text` |
| Step5 Draft SOW | DesignJson | `Global.Step4Out.text` |
| Step5 Draft SOW | ArchitectName | `Susan Architect` |
| Step5 Draft SOW | TodayDate | `Text(Today(), "mmmm d, yyyy")` |
