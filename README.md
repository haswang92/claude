# Psychiatric Note Master Prompts

Three reusable prompts that turn **your site's template + the patient's records** into a draft psychiatric note in any LLM (clinical or general-purpose). Each comes in a **full version** and a **SHORT VERSION**.

| Note type | Full version | Short version (~5,000 characters) | Use it for |
|---|---|---|---|
| H&P | `prompts/1_HP_Master_Prompt.txt` | `prompts/1_HP_Master_Prompt-SHORT VERSION.txt` | Inpatient admissions, outpatient intakes, ED evaluations, initial consults |
| Progress / follow-up | `prompts/2_Progress_FollowUp_Master_Prompt.txt` | `prompts/2_Progress_FollowUp_Master_Prompt-SHORT VERSION.txt` | Inpatient daily progress, outpatient med management, psychotherapy add-on, C-L follow-up, PHP/IOP, telepsych |
| Discharge | `prompts/3_Discharge_Master_Prompt.txt` | `prompts/3_Discharge_Master_Prompt-SHORT VERSION.txt` | Discharge summaries / discharge notes |

**Which version?**
- **Full version (~22,000–25,000 characters):** the most detailed and reliable. Use it in any tool that accepts it.
- **Short version (~5,000 characters):** same rules in condensed form. Use it when a tool rejects or cuts off the full prompt. Microsoft Copilot, for example, is reported to cap messages at about 8,000 characters.

The prompts are `.txt` files on purpose. In a `.md` file, GitHub would display `***` as a line or bold text and the copied prompt would be wrong. On GitHub, open a file and click **Raw** before copying.

## What the prompts make the LLM do
- **Follow your template exactly.** Same headings, order, and wording. Nothing is added or removed.
- **Put `***` wherever the records don't have the information.** In Epic, press F2 to jump between them.
- **Read the date on every document** (date of service, then signed date, then upload/scan date) and sort the information:
  - **Enduring history** (suicide attempts, hospitalizations, medication trials, serious reactions, trauma, family history, etc.) is always kept, with its date.
  - **"State" information** (MSE, current SI/HI, vitals, labs, current meds, legal status) is used only if it is from the current encounter. It is never copied forward from older notes.
  - **Older episodes** are included only if they matter for the current presentation. If they don't, they are left out silently.
- **Leave EHR auto-fill codes alone** (`@VS@`, `@MEDS@`, etc.) so your EHR fills them with live data.
- **Never invent anything:** no new diagnoses, doses, plans, risk levels, or "denies" statements. The A/P is a synthesis of what is documented.
- **Put the note in a "code box"**: a gray box with a **Copy** button. Without it, chat windows can turn `***` into a line or bold text, or `[ ]` into a checkbox, and your copied note would lose them. The box keeps the note exactly as written.
- **Add a Review List below the box** (not part of the note) covering conflicts, items taken from older records, missing safety items (e.g., firearm access), undated documents, and judgment calls. Read it; don't paste it into the chart.

## How to use (every time)
1. Open the prompt for the note type. Select all and copy it.
2. Paste it into the LLM chat. Do not send yet.
3. At the bottom of the prompt, fill in:
   - `NOTE DATE:` today's encounter date (optional, but improves date reasoning)
   - `SITE:` (optional)
   - `EXTRA SITE INSTRUCTIONS:` (optional, see below)
   - `TEMPLATE:` paste the site's template for this note type
   - `DOCUMENTS:` paste the records. Separate documents with a line like `--- Document ---`. Raw EHR text is fine; the LLM finds the dates itself.
4. Send.
5. Click the **Copy** button on the code box to copy the note. Review it against the Review List, then paste it into the chart. Delete the `===== BEGIN NOTE =====` / `===== END NOTE =====` lines if they come along.
   - If your tool shows no code box (you see the ```` ``` ```` marks instead), select only the text between the BEGIN/END lines and copy that.

**Tip:** if your LLM tool has a "custom instructions," "system prompt," "project instructions," or custom-agent instructions box, paste the prompt there once. After that, each chat only needs the TEMPLATE and DOCUMENTS sections.

## When the input is too long: send it in parts
If the tool rejects or cuts off your message, split it across several messages and label each one:
1. **Message 1:** `PART 1 of 3`, then the prompt.
2. **Message 2:** `PART 2 of 3`, then the TEMPLATE and the first documents.
3. **Message 3:** `PART 3 of 3`, then the remaining documents.

The LLM replies only "Received part X of 3." until the last part arrives, then writes the note. Every prompt (full and short) includes this rule.

## Uploading files vs. pasting text
- **Paste text when you can.** It is the most reliable, because the LLM sees every word and every date.
- **Scanned PDFs, faxes, and photos** are images of text. The tool may misread them or skip them. Double-check anything taken from them.
- **Dates in headers and footers often get lost** when a PDF is read. When you upload a file, type its date and type next to it in the DOCUMENTS section, e.g. `--- Document: Outside discharge summary, 03/28/2023 (attached file) ---`.
- **Long or multiple files may be only partly read.** Some tools search uploaded files for the pieces that seem relevant instead of reading every page. Keep the record set focused (see below).
- **Paste the prompt itself as the message, don't upload it.** Tools may treat an uploaded prompt as reference material instead of instructions.

## Notes on specific tools (test each one first)
None of these publish exact input limits, and features change often. Run the fictional test case below in each tool before using it with patients.
- **Doximity GPT:** described by Doximity as HIPAA-compliant and supports file attachments. Try the full version first.
- **Microsoft Copilot:** use only your institution's **enterprise Microsoft 365 Copilot**. The free consumer Copilot is not approved for patient information. The message limit is reported to be about 8,000 characters, so you'll likely need the **SHORT VERSION**, sending in parts, or pasting the full prompt into a custom agent's instructions (if your institution allows custom agents).
- **OpenEvidence:** reported to be HIPAA-compliant with a BAA. Its main chat is built to answer clinical questions with literature citations, so it may add references or summarize instead of filling the template exactly. Check the result closely; the test case will show quickly how well it follows the rules.

## Site-specific tweaks (no need to edit the prompt)
Write one-line rules in `EXTRA SITE INSTRUCTIONS:`. These override the defaults, but they can never override the no-fabrication rule. Examples:
- `This site's EHR has no SmartLinks; fill @VS@ and @MEDS@ from the documents.`
- `Use full sentences in the HPI; no abbreviations.`
- `Mark checkboxes with (X).`
- `Hospital course should be organized by problem, not by date.`

You can keep a small text file per site with its templates and its extra instructions, ready to paste.

## Test before real use
`examples/sample_progress_test_case.txt` is a **fictional** patient. It works with either progress prompt (full or short). Paste it after the prompt, then check the answer against `examples/sample_progress_expected_behavior.txt`. It checks that:
- the note is in a code box and the template is followed exactly;
- the old irrelevant episode is left out;
- the old suicide attempt is kept with its date;
- the old MSE and old SI are not carried forward;
- `@VS@` is left alone;
- missing firearm access shows `***`;
- the medication-dose conflict is flagged.

The short prompt plus the test case is about 9,000 characters, so in Copilot you may need to send it in 2 parts (see above). Try it in each LLM you plan to use; some follow rules better than others.

## Important cautions
- **Privacy / HIPAA:** only paste real patient information into tools your institution has approved for PHI (with a Business Associate Agreement). For general consumer chatbots, de-identify first, or use fictional/test data only.
- **You remain the author.** Always review every line against the source records before signing, especially medications, doses, allergies, legal status, and risk assessment.
- **Long records:** paste only the most relevant documents. A typical set:
  - the current encounter documents;
  - the most recent prior note of the same type;
  - the most recent H&P or discharge summary;
  - the current medication list;
  - recent labs.
