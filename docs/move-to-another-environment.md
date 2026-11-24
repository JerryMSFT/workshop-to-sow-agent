# Move the agent to another environment

> **Status:** not yet tested in this build. The steps follow Microsoft Learn: [Export and import agents using solutions](https://learn.microsoft.com/en-us/microsoft-copilot-studio/authoring-solutions-import-export). If you'd rather rebuild from scratch, follow [build-guide.md](build-guide.md); it gives the same result.

You need at least the **System Customizer** role in both environments. The target environment's data policy must allow Word Online (Business), OneDrive for Business and Office 365 Outlook.

## Export (source environment)

1. Go to **make.powerapps.com** and pick the source environment (top right).
2. **Solutions** > **+ New solution**.
   - Name: `Workshop-to-SOW`
   - Publisher: any
   - Click **Create**.
3. **Add existing** > **Agent** > **Agent** > **Workshop-to-SOW Agent** > **Add**.
4. **Add existing** > **Automation** > **Cloud flow** > **Get transcript text** and **Fill SOW** > **Add**.
5. On the agent row and on each flow row, click **⋮** > **Advanced** > **Add required objects**.
6. **Export solution** > **Unmanaged** > **Export** > **Download** the zip.

## Import (target environment)

1. On **make.powerapps.com**, pick the target environment.
2. **Solutions** > **Import solution** > browse to the zip > **Next**.
3. When asked, create connections for **Word Online (Business)**, **OneDrive for Business** and **Office 365 Outlook**.
4. Click **Import**.

## After the import

These depend on the account and environment, so redo them:

1. **OneDrive:** do Part 1 of the build guide again (folders and template upload) in the new account.
2. **Fill SOW flow:** re-pick the template file in **Populate a Microsoft Word template** and the folder in **Create file**. Save, then publish.
3. **Agent knowledge:** re-upload `offer-library.xlsx` if it's missing.
4. **Prompts:** check that each one uses **GPT-6 Luna**.
5. **Topic:** check that every prompt node shows its own `Global.StepNOut` (build guide 6.5).
6. Run Test 1 and Test 4 from [testing.md](testing.md).
