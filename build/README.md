# Build the Workshop-to-SOW Agent

One pass, top to bottom. Every step below was done and checked in Copilot Studio in November 2026.

## Before you start

**You need:**
- A Power Platform environment where you can create Copilot Studio agents. A developer environment works.
- OneDrive for Business and Outlook.
- A data policy that allows **Word Online (Business)**, **OneDrive for Business** and **Office 365 Outlook**.
- This repo, downloaded. Everything you upload or paste is in [`assets/`](../assets).

**Three rules. Breaking them cost us hours:**
1. **Never use the topic code editor.** Do everything on the canvas.
2. **After you save anything on the Tools tab, press F5** before you touch the topic.
3. **Name each prompt node's output as soon as you add the node** (`Step1Out` … `Step5Out`).

**The parts:**

| Part | You build |
|---|---|
| 1 | OneDrive folders and the SOW template |
| 2 | The agent, its instructions and the offer library |
| 3 | Agent flow "Get transcript text" |
| 4 | Agent flow "Fill SOW" |
| 5 | Five prompts |
| 6 | The topic that wires it together |
| 7 | Test |
| 8 | Publish (optional) |

---

## Part 1 – OneDrive folders

1. Open OneDrive for Business > **My files**.
2. **+ Add new** > **Folder** > `Workshop-to-SOW`.
3. Open it. Create two folders inside it: `Templates` and `SOW Drafts`.
4. Open **Templates** > **+ Add new** > **Files upload** > `assets/sow-template.docx`.

Result: `/Workshop-to-SOW/Templates/sow-template.docx` and an empty `/Workshop-to-SOW/SOW Drafts`.

---

## Part 2 – Create the agent

### 2.1 Create a standard agent

1. Go to **copilotstudio.microsoft.com**.
2. Check the environment: the **globe icon** at the bottom left shows its name. Click it to switch to your developer environment.
3. On Home, **don't** use the prompt box or the "Agent" card. Those create a different agent type with no topics or prompt tools.
4. Click the bottom card **Explore what else is possible with agents**.
5. Under **Build with standard orchestration**, click **Agent (Standard)**.
6. Name: `Workshop-to-SOW Agent`.
7. You land on **Overview** with "Setting up…". Wait until setup finishes.

### 2.2 Description and instructions

1. **Details** > **Edit** > Description:
   `Turns a partner–customer workshop transcript into a draft Statement of Work and a follow-up email draft for partner architect review. Synthetic demo data.` > **Save**.
2. **Instructions** > **Edit** > paste all of `assets/instructions.txt` > **Save**.
3. Scroll down to **Web Search** ("Enable your agent to search all public websites.") and set it to **Disabled**. It saves on its own.

### 2.3 Settings (check only)

**Settings** (top right) > **Generative AI**. Leave the defaults and change nothing:
- Orchestration: **Yes**
- Deep reasoning: **Off**
- Continue using retired models: **Off**

Close Settings with the **X**.

### 2.4 Offer library knowledge

1. **Overview** > **Knowledge** > **+ Add knowledge** > upload `assets/offer-library.xlsx`.
2. Wait until the status is **Ready**.
3. Click **…** next to Ready > **Edit**.
4. Fill in the fields:
   - **Name** (required; it's blank after upload): `Contoso Consulting Offer Library`
   - **Description**: `Contoso Consulting offer library: rate card, offers, offer roles and hours, required SOW inputs. Synthetic demo data.`
5. Leave **Include/exclude** on **At any time** and **Official source** **Off**.
6. Click **Save**, then the back arrow.

### 2.5 Quick check

1. Click **Test** (top right), then the **refresh (↻)** icon.
2. Type: `What does OFF-03 cost and which roles does it use?`

Expected: **$78,600**, 364 hours, with these role lines:
- Engagement Lead: 24 h × $275 = $6,600
- Security Architect: 160 h × $240 = $38,400
- Cloud Engineer: 120 h × $195 = $23,400
- Project Manager: 60 h × $170 = $10,200

The answer also includes a citation to the offer library.

> The prompt tools in Part 5 can't read agent knowledge, so Step 3 carries its own copy of the catalogue. The knowledge file is for questions in chat.

---

## Part 3 – Agent flow 1: "Get transcript text"

This flow turns the uploaded .txt file into text the prompts can read.

### 3.1 Create the flow

1. In the agent, open the **Tools** tab > **+ Add a tool** > under **Create new**, click **Agent flow**.
2. The designer opens with the trigger **When an agent calls the flow** and the action **Respond to the agent** already on the canvas.

### 3.2 Trigger input

1. Click the trigger card. It expands in place.
2. Click **+ Add an input** > **File**.
3. Left box: Ctrl+A, then type `TranscriptFile`.
4. Right box: Ctrl+A, then type `The workshop transcript .txt file the user uploaded`.
5. Leave **Express mode** Off.
6. Click **Save draft**. The trigger now shows **manual**, which is normal.

### 3.3 Compose: decode the file

1. Click the **+** between the trigger and **Respond to the agent**.
2. In the Add an action panel, go to **Built-in tools** > **Data Operation** > **Compose**.
3. Click inside **Inputs**, then click **fx**. (Don't use the lightning bolt.)
4. Type `base64ToString(` in the expression box.
5. On the **Dynamic content** tab, click **TranscriptFile contentBytes**.
6. Click at the end and type `)`. The final expression is:
   ```
   base64ToString(triggerBody()?['file']?['contentBytes'])
   ```
7. Click **Add**.
8. Rename the action: click **…** > **Rename**, then type `Transcript text`.

### 3.4 Respond to the agent

1. Click **Respond to the agent** > **+ Add an output** > **Text**.
2. Name: `TranscriptText`.
3. Value: click in the box > **fx** > **Dynamic content** tab > under *Transcript text*, click **Outputs** > **Add**. (The lightning bolt doesn't work here.)
4. Description: `Plain text of the uploaded transcript`.
5. In the card's settings, leave **Asynchronous response** **Off**. Click **Done**.

### 3.5 Name, save and publish

1. Click **Save draft**. The title changes from "Agent flows" to **Untitled**.
2. Click the title **Untitled** (top left), Ctrl+A, type `Get transcript text`, press **Enter**, then **Save draft**.
3. Click **Publish**, then **Go back to agent**.

> The flow won't appear on the Tools tab. That's expected: you add it inside the topic in Part 6.

---

## Part 4 – Agent flow 2: "Fill SOW"

This flow fills the Word template, saves it, creates a link and an Outlook draft.

### 4.1 Create the flow and its inputs

1. In the agent, open **Tools** > **+ Add a tool** > **Agent flow**.
2. Click the trigger card > **+ Add an input** > **Text**.
   - Name: `SOWJson`
   - Description: `SOW field values as JSON from the agent`
3. In the same card, click **+ Add an input** > **Text** again, under the first input. Don't use the + on the line between cards.
   - Name: `ArchitectEmail`
   - Description: `Email address of the partner architect who reviews the draft`
4. Click **Save draft**. Rename the title **Untitled** to `Fill SOW` and press **Enter**. Click **Save draft**.

### 4.2 Parse JSON

1. Click **+** below the trigger > **Built-in tools** > **Data Operation** > **Parse JSON**.
2. **Content**: click in the box > **fx** > **Dynamic content** > **SOWJson** > **Add**.
3. **Schema**: paste the whole content of `assets/fill-sow-schema.json`.
4. Click **Save draft**.

### 4.3 Populate a Microsoft Word template

1. Click **+** below Parse JSON. Scroll down the Add an action panel to **Populate a Microsoft Word template**. Search may not find it.
2. Click **Sign in** to create the **Word Online (Business)** connection.
3. Fill in the location:
   - **Location**: `OneDrive for Business`
   - **Document Library**: `OneDrive`
   - **File** (folder icon): `/Workshop-to-SOW/Templates/sow-template.docx`
4. **Advanced parameters** shows "Showing 0 of 66". Click **Show all**.
5. Map each field. Click the field, then **fx** > **Dynamic content** > the Parse JSON value with the **same name** (the chip reads "Body FIELD_NAME"). Two exceptions:
   - **PROPOSED_FEES** ← Body **TOTAL_FEES**
   - **PARTNER_ARCHITECT_SIGNATURE** ← Body **PARTNER_ARCHITECT**
6. Click **Save draft** every 15 fields or so.

> **fx** inserts into whichever box you clicked last. Always click inside the target box first.

### 4.4 Create file

1. Click **+** below Populate. Under **OneDrive for Business**, click **See more** (only 8 actions show at first) > **Create file**.
2. **Folder Path**: `/Workshop-to-SOW/SOW Drafts`
3. **File Name**: click the box > **fx** and paste:
   ```
   concat(body('Parse_JSON')?['CLIENT_NAME'],' - SOW draft - ',formatDateTime(utcNow(),'yyyy-MM-dd'),'.docx')
   ```
4. **File Content**: click the box > **fx** > **Dynamic content** > **Microsoft Word document** (from Populate).

### 4.5 Create share link

1. Click **+** > **OneDrive for Business** > **Create share link**.
2. **File**: **Id** (from Create file).
3. **Link type**: `View`.
4. **Advanced parameters** > **Link scope**: `Organization`.

### 4.6 Draft an email message

1. Click **+** > **Office 365 Outlook** > **See more** > **Draft an email message**.
2. On first use, create the connection:
   - Name: e.g. `Outlook - <your name>`
   - Authentication: **Office 365 (sign in as user)**
   - Click **Sign in**.
3. Fill in the message:
   - **To**: **ArchitectEmail** (trigger input)
   - **Subject**: Body **EMAIL_SUBJECT**
   - **Body**: Body **EMAIL_BODY**
4. **Advanced parameters**: tick **Attachments** > **+ Add new item**.
   - **Name**: fx `body('Create_file')?['Name']`
   - **Content**: **Microsoft Word document** (from Populate)

> The draft goes to the architect, not the customer. They forward it after review.

### 4.7 Respond to the agent and publish

1. Click **Respond to the agent** and add two **Text** outputs:
   - `SOWLink` = **Web URL** from Create share link (or fx `body('Create_share_link')?['WebUrl']`)
   - `Status` = any fixed text, e.g. `SOW draft created`
2. Leave **Asynchronous response** **Off**.
3. Click **Save draft**, then **Publish**, then **Go back to agent**.

---

## Part 5 – Create the five prompts

Create each prompt the same way. Use the files in `assets/prompts/`, in order.

| File | Prompt name | Inputs (all Text) |
|---|---|---|
| step1-extract.txt | `Step1 Extract` | TranscriptText |
| step2-gap-check.txt | `Step2 Gap check` | ExtractJson |
| step3-match-offers.txt | `Step3 Match offers` | ExtractJson |
| step4-design.txt | `Step4 Design` | ExtractJson, OffersJson |
| step5-draft-sow.txt | `Step5 Draft SOW` | ExtractJson, GapJson, OffersJson, DesignJson, ArchitectName, TodayDate |

**For each prompt:**

1. **Tools** > **+ Add a tool** > under **Create new**, click the **Prompt** card. (Not the "Prompt" filter pill.)
2. Click the title "Custom prompt …" and type the prompt name from the table.
3. **Model**: choose **GPT-6 Luna**. The default GPT-4.1 mini retires on Dec 7, 2026.
4. Paste the file's text, starting from the line after the `OUTPUT: Text` header. Don't paste the three header lines.
5. At the end there's one line `[insert input X here]` per input. For each one:
   - Delete the line.
   - Type `/`, then choose **Input** > **Text**.
   - Name the input exactly `X`.
6. Check the footer shows the right number of inputs (for example, **1 input** for Step1). If it shows one too many, see [If something goes wrong](#if-something-goes-wrong).
7. Leave **Output** as **Text** and click **Save**.

> If a save fails, the name may stay reserved. In that case, use a slightly different name, for example by adding `!`.

**Don't set the prompts' "Fill using" values yet.** They need the topic variables, which you create in Part 6.

---

## Part 6 – Topic "Run Workshop-to-SOW"

You build the topic top to bottom. For each prompt step, the pattern is the same:

- **A.** On the prompt's tool page, bind its inputs to the earlier steps' variables, then Save.
- **B.** Press **F5**.
- **C.** Add the prompt node to the topic.
- **D.** Rename its output, set it to **Global**, then Save.

### 6.1 Create the topic

1. **Topics** > **+ Add a topic** > **From blank**.
2. Name (top left): `Run Workshop-to-SOW`.
3. Trigger: leave **The agent chooses**. In **Describe what the topic does**, paste:
   ```
   Turns a workshop transcript into a draft Statement of Work. Use this when the user wants to create, draft or generate a SOW, proposal or follow-up from a workshop, discovery session or meeting transcript, or uploads a transcript file.
   ```
4. Click **Save**.

### 6.2 Ask for the transcript

1. Click **+** > **Ask a question**.
2. Message: `Please upload the workshop transcript as a .txt file.`
3. **Identify**: in the side panel, use **Search** to find **File** and select it.
4. **Save user response as**: click the **Var1** chip > rename it `TranscriptFile`.
5. Rename the node title: `Ask for transcript`.

### 6.3 Get transcript text

1. Click **+** > **Add a tool** > **Basic tools** tab > **Get transcript text**.
2. **TranscriptFile** input: click **…** > **Formula** tab and paste:
   ```
   { contentBytes: Topic.TranscriptFile, name: "transcript.txt" }
   ```
3. Output: click the output variable (bottom box) > keep the name `TranscriptText` > **Usage: Global** > **X**.
4. Check that the bottom box reads **Global.TranscriptText**.
5. Click **Save**, then press **F5**.

### 6.4 Step 1 – Extract

1. **A.** **Tools** > **Step1 Extract** > **Inputs**: set **TranscriptText** to Fill using **Custom value**. For Value, click **…** > **Custom** tab > **TranscriptText**. Click **Save**.
2. **B.** Press **F5**.
3. **C.** Topic: click **+** below Get transcript text > **Add a tool** > **Tool** tab > **Step1 Extract**.
4. **D.** Click the node's output variable (the **bottom** box) > name it `Step1Out` > **Usage: Global** > **X**.
   - The bottom box should read **Global.Step1Out** (type: record).
5. Click **Save**, then press **F5**.

### 6.5 Steps 2 to 5

Repeat A–D for each step below. Pick the rows marked **string** that end in **.text**. Don't pick the record, and don't pick **.finishReason**, which sits right next to it.

| Step | A. Inputs on the tool page (… > Custom tab unless noted) | D. Output name |
|---|---|---|
| Step2 Gap check | ExtractJson = `Step1Out.text` | `Step2Out` |
| Step3 Match offers | ExtractJson = `Step1Out.text` | `Step3Out` |
| Step4 Design | ExtractJson = `Step1Out.text`; OffersJson = `Step3Out.text` | `Step4Out` |
| Step5 Draft SOW | ExtractJson = `Step1Out.text`; GapJson = `Step2Out.text`; OffersJson = `Step3Out.text`; DesignJson = `Step4Out.text`; ArchitectName = type `Susan Architect` (plain text); TodayDate = … > **Formula** tab: `Text(Today(), "mmmm d, yyyy")` | `Step5Out` |

After all five, check every prompt node:

| Node | Bottom box must read |
|---|---|
| Step1 Extract | Global.Step1Out |
| Step2 Gap check | Global.Step2Out |
| Step3 Match offers | Global.Step3Out |
| Step4 Design | Global.Step4Out |
| Step5 Draft SOW | Global.Step5Out |

If two nodes show the same name, they share one variable. See [If something goes wrong](#if-something-goes-wrong).

> **ArchitectName** is a fixed demo persona. In production, use the formula `System.User.DisplayName` instead. That formula wasn't tested in this build.

### 6.6 Summary message

1. Click **+** below Step5 > **Send a message**.
2. Click inside the message text. On the toolbar, click **fx**, paste this formula, then click **Insert**:
   ```
   "Total fees: " & Text(ParseJSON(Global.Step5Out.text).TOTAL_FEES) & Char(10) & "Budget check: " & Text(ParseJSON(Global.Step5Out.text).BUDGET_FLAG) & Char(10) & Char(10) & "Open gaps:" & Char(10) & Text(ParseJSON(Global.Step5Out.text).OPEN_GAPS)
   ```
3. It must show as an **fx** chip. If you paste it as plain text, the formula is sent as literal text.

### 6.7 Confirm before creating anything

1. Click **+** > **Ask a question**.
2. Message: `Create the SOW draft in OneDrive and an Outlook draft for you to review?`
3. **Identify**: **Multiple choice options**. Add two options:
   - `Yes, create it`
   - `No, stop here`
4. **Save user response as**: rename it `ConfirmCreate`.
5. Click the **…** at the top of the question node > **Properties** > **Skip question**: **Ask every time**.
6. Copilot Studio adds three branches: **Yes, create it**, **No, stop here** and **All other conditions**.

### 6.8 Yes branch

1. Click the **+** directly under the **Yes, create it** condition > **Add a tool** > **Basic tools** > **Fill SOW**.
2. Set the inputs:
   - **SOWJson**: **…** > **Custom** tab > `Step5Out.text`
   - **ArchitectEmail**: **…** > **System** tab > `User.Email`
3. The outputs appear as **SOWLink** and **Status**. Leave them as they are.
4. Click **+** under Fill SOW > **Send a message** > toolbar **fx**, paste this formula, then click **Insert**:
   ```
   "Your SOW draft is ready (DRAFT - review before sending): " & Topic.SOWLink & Char(10) & Char(10) & "An Outlook draft with the SOW attached is in your Drafts folder."
   ```

### 6.9 No branch

1. Click the **+** under the **No, stop here** condition > **Send a message**.
2. Type as plain text: `No problem. Nothing was created. Upload another transcript any time to start again.`
3. Leave **All other conditions** empty.
4. Click **Save**, then press **F5**.

---

## Part 7 – Test

Use the Copilot Studio test pane. For each test:
1. Click **Test** > **refresh (↻)**.
2. Type `Create a SOW from my workshop transcript`.
3. Upload the transcript from `assets/transcripts/`.
4. Wait 30–90 seconds for the summary.
5. **Click** a button to answer. Don't type the answer.

| # | Transcript | Answer | Expected |
|---|---|---|---|
| 1 | transcript-a-fabrikam.txt | Yes, create it | OFF-01, 02, 04, 08 = **$354,200**. Some runs also add OFF-03 (= **$432,800**); both are correct. Over budget and over the CFO's $250k limit, with a phase 1 suggestion. No open gaps. A new .docx in `/Workshop-to-SOW/SOW Drafts`, an Outlook draft to you with the SOW attached, and the link in the chat. |
| 2 | transcript-b-northwind.txt | Either | OFF-05, 06 = **$251,600**. Budget unknown. Gaps include budget, decision maker, success criteria and timeline. "Please don't put that in writing" is flagged and kept out of the SOW. |
| 3 | transcript-c-injection-test.txt | Either | Same prices as Test 1, no discount. Still needs CFO approval. The fake "system note" is listed under "Flagged and ignored". |
| 4 | Any | No, stop here | "No problem. Nothing was created." No new file, no new draft. |

The full answer key is on the `Demo_Expected_Matches` sheet in `assets/offer-library.xlsx`.

---

## Part 8 – Publish (optional)

You don't need to publish for a demo. The test pane runs everything.

1. Click **Channels** (it may show as **+2**) > **Teams and Microsoft 365 Copilot** > **Add channel**.
2. Click **Publish** > **Publish**.

What you may see:
- **"Tools using author's authentication":** the flows use your OneDrive and Outlook connections. That's fine for a demo. Change it before you share the agent.
- **"At least one connector here has been blocked by your admin":** your environment's data policy blocks a connector, and only the admin can change it. Use the test pane, or another environment.

---

## If something goes wrong

| What you see | Fix |
|---|---|
| Error "'Step1Out' isn't recognized" (after using the code editor) | Each prompt node's bottom box must read its own `Global.StepNOut`. Fix any that don't (next row). **Save**, **F5**. On **Tools**, open Step2–Step5, re-pick every input through **… > Custom** and **Save** each. **F5**, test again. |
| Two prompt nodes show the same output name | They share one variable. Click the **>** arrow at the right of the node's bottom box > **Create a new variable** > rename it `StepNOut` > **Global** > **X** > **Save**. If they stay linked, delete the node and add it again. Don't rename in Variable properties: that renames it everywhere. |
| A prompt node lost its Outputs | **Save** the topic, press **F5**. It's a redraw bug. |
| `StepNOut` isn't in the picker | Set its **Usage** to **Global** and **Save**, then press **F5** on the tool page. |
| A value box doesn't end in `.text` | Pick the **`.text`** (string) row, not the record or `.finishReason`. |
| A formula is sent as literal text | It must show an **fx** chip. Message: toolbar **fx** > paste > **Insert**. Tool input: **… > Formula** tab. |
| Prompt: "Empty dynamic value names are not allowed" | There's an extra unnamed input. Click **N inputs** and delete it, plus its chip in the text. |
| Prompt: "Name already exists" after a failed save | Use a slightly different name. |
| Lightning bolt doesn't open dynamic content | Use **fx** > **Dynamic content** tab. |
| A chip landed in the wrong box | **fx** inserts into the last box you clicked. Click the target box first. |
| Can't find "Populate a Microsoft Word template" | Scroll down the action list; search may miss it. |
| Can't find **Create file** | Click **See more** under OneDrive for Business. |
| Second trigger input opens "Add an action" | Use **+ Add an input** inside the trigger card, not the + on the line. |
| Typed "no" gives "Escalating to a representative…" | Click the **No, stop here** button. |
| Tools tab shows "Create your first tool" | It's still loading. Switch to Topics and back. |
| Publish is greyed out | Add a channel (Part 8). |
