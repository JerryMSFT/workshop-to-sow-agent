# Troubleshooting

Problems we hit while building, and the fix that worked.

## The big one: don't use the topic code editor

**Symptom:** after editing the topic in **… > Open code editor** (or reverting such an edit), every run fails with:

```
The expression on the 'ManualTaskInput' node at '' failed with the following errors:
'Name isn't valid. 'Step1Out' isn't recognized.' (.Step1Out)
```

**Cause:** the code-editor save reset each prompt node's output back to one shared variable called `predictionOutput`. The `Step1Out` … `Step4Out` variables disappeared, so the later prompts lost their inputs.

**Fix:**
1. Check the bottom box of every prompt node. Each must read its own `Global.StepNOut`.
2. Give any node that's wrong its own variable. See [Two steps share one variable](#two-steps-share-one-variable).
3. **Save**, then press **F5**.
4. On the **Tools** tab, open Step2 to Step5. Re-pick every input through **… > Custom** and **Save** each one, even if it looks correct.
5. **F5**, then test again.

**Prevention:** make every change on the canvas.

## Two steps share one variable

**Symptom:** two prompt nodes show the same output name. For example, Step3 and Step4 both read `Global.Step4Out`, and renaming one renames the other.

**Cause:** both nodes write to the same variable. Renaming it in **Variable properties** renames it everywhere.

**Fix:** give one node a fresh variable.
1. Click the **>** arrow at the right end of the node's bottom box (not the name) > **Create a new variable**.
2. Click the new name (**Var1**) > rename it `StepNOut` > **Global** > **X** > **Save**.

If the two stay linked, delete that node (**… > Delete**). Re-add it with **+ > Add a tool > Tool** tab, then name its output.

## A node lost its Inputs or Outputs section

**Symptom:** a prompt node shows only "Step… / View tool", with no Outputs. This happens after you change the prompt on the Tools tab.

**Fix:** **Save** the topic and press **F5**. Don't delete the node; it's a redraw bug.

## A StepNOut variable isn't in the picker

- Its **Usage** is still **Topic**. Set it to **Global**, then **Save**.
- The tool page was opened before the variable existed. Press **F5** on the tool page.

## Picked the wrong row in the picker

The picker shows `StepNOut` (record), `StepNOut.finishReason` and `StepNOut.text` next to each other. Always pick **`.text`** (string). The value box must read `Global.StepNOut.text`.

## Text in a value box or message is sent literally

Plain text typed in a box is a fixed string. Formulas must show an **fx** chip:
- **Message node:** click in the text > toolbar **fx** > paste > **Insert**.
- **Tool input:** **… > Formula** tab.

## Prompt won't save

**"Empty dynamic value names are not allowed"**, with Save greyed out, means an extra unnamed input exists. The footer shows one input too many. Click the **N inputs** button and delete the unnamed one, along with its chip in the text.

**"Name already exists"** after a failed save means the failed attempt reserved the name. Use a slightly different name.

## Agent flow designer

| Problem | Fix |
|---|---|
| The lightning bolt doesn't open dynamic content (Respond to the agent, Create file) | Use **fx** > **Dynamic content** tab |
| A chip landed in the wrong box | **fx** inserts into the last box you clicked. Click the target box first. |
| Search can't find "Populate a Microsoft Word template" | Scroll down the Add an action panel |
| Can't find **Create file** | OneDrive for Business shows 8 actions; click **See more** |
| The second trigger input opens "Add an action" | You clicked the + on the line. Click the trigger card and use **+ Add an input** inside it. |
| Renaming a step renamed the action, not the flow | Rename the flow by clicking the title (top left) |
| The title says "Agent flows" | It becomes **Untitled** after the first **Save draft**; then rename it |

## Question node

| Problem | Fix |
|---|---|
| Renaming the node title didn't rename the variable | Rename the variable from the **Save user response as** chip |
| `.Content` / `.Name` fail on the file variable | A File answer is a Blob. Pass `{ contentBytes: Topic.TranscriptFile, name: "transcript.txt" }` |
| A typed "no" gives "Escalating to a representative is not currently configured…" | Click the **No, stop here** button. Typed answers aren't matched reliably, and `synonyms` can't be added in this version. |

## Other screens

| Problem | Fix |
|---|---|
| The Tools tab shows "Create your first tool" after F5 | It's still loading. Switch to Topics and back to Tools. |
| Publish is greyed out: "You need to configure at least one channel" | Add the **Teams and Microsoft 365 Copilot** channel |
| "At least one connector here has been blocked by your admin" | Your environment's data policy. Only the admin can change it. Use the test pane or another environment. |
| "Tools using author's authentication" | The flows use your connections. That's fine for a demo; change it before sharing the agent. |
