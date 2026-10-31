# Psychiatric Note Master Prompts

Three reusable prompts that turn **your site's template + the patient's records** into a draft psychiatric note in any LLM (clinical or general-purpose).

| Prompt file | Use it for |
|---|---|
| `prompts/1_HP_Master_Prompt.txt` | H&P / initial evaluation: inpatient admissions, outpatient intakes, ED evaluations, initial consults |
| `prompts/2_Progress_FollowUp_Master_Prompt.txt` | Any follow-up: inpatient daily progress, outpatient med management, psychotherapy add-on, C-L follow-up, PHP/IOP, telepsych |
| `prompts/3_Discharge_Master_Prompt.txt` | Discharge summaries / discharge notes |

The prompts are `.txt` files on purpose. In a `.md` file, GitHub would display `***` as a line or bold text and the copied prompt would be wrong.

## What the prompts make the LLM do
- **Follow your template exactly.** Same headings, order, and wording. Nothing is added or removed.
- **Put `***` wherever the records don't have the information.** In Epic, press F2 to jump between them.
- **Read the date on every document** (date of service, then signed date, then upload/scan date) and sort the information:
  - **Enduring history** (suicide attempts, hospitalizations, medication trials, serious reactions, trauma, family history, etc.) is always kept, with its date.
  - **"State" information** (MSE, current SI/HI, vitals, labs, current meds, legal status) is used only if it is from the current encounter. It is never copied forward from older notes.
  - **Older episodes** are included only if they matter for the current presentation. If they don't, they are left out silently.
- **Leave EHR auto-fill codes alone** (`@VS@`, `@MEDS@`, etc.) so your EHR fills them with live data.
- **Never invent anything:** no new diagnoses, doses, plans, risk levels, or "denies" statements. The A/P is a synthesis of what is documented.
- **Add a Review List after the note** (not part of the note) covering conflicts, items taken from older records, missing safety items (e.g., firearm access), undated documents, and judgment calls. Read it, then delete it.

## How to use (every time)
1. Open the prompt for the note type. Select all and copy it.
2. Paste it into the LLM chat. Do not send yet.
3. At the bottom of the prompt, fill in:
   - `NOTE DATE:` today's encounter date (optional, but improves date reasoning)
   - `SITE:` (optional)
   - `EXTRA SITE INSTRUCTIONS:` (optional, see below)
   - `TEMPLATE:` paste the site's template for this note type
   - `DOCUMENTS:` paste the records. Separate documents with a line like `--- Document ---`. Raw EHR text is fine; the LLM finds the dates itself.
4. Send. Copy only the text between `===== BEGIN NOTE =====` and `===== END NOTE =====` into the chart, after reviewing it against the Review List.

**Tip:** if your LLM tool has a "custom instructions," "system prompt," or "project instructions" box, you can paste the prompt there once. After that, each chat only needs the TEMPLATE and DOCUMENTS sections.

## Site-specific tweaks (no need to edit the prompt)
Write one-line rules in `EXTRA SITE INSTRUCTIONS:`. These override the defaults, but they can never override the no-fabrication rule. Examples:
- `This site's EHR has no SmartLinks; fill @VS@ and @MEDS@ from the documents.`
- `Use full sentences in the HPI; no abbreviations.`
- `Mark checkboxes with (X).`
- `Hospital course should be organized by problem, not by date.`

You can keep a small text file per site with its templates and its extra instructions, ready to paste.

## Test before real use
`examples/sample_progress_test_case.txt` is a **fictional** patient. Paste it after the progress prompt, then check the answer against `examples/sample_progress_expected_behavior.txt`. It checks that:
- the template is followed exactly;
- the old irrelevant episode is left out;
- the old suicide attempt is kept with its date;
- the old MSE and old SI are not carried forward;
- `@VS@` is left alone;
- missing firearm access shows `***`;
- the medication-dose conflict is flagged.

Try it in each LLM you plan to use; some follow rules better than others.

## Important cautions
- **Privacy / HIPAA:** only paste real patient information into tools your institution has approved for PHI (with a Business Associate Agreement). For general consumer chatbots, de-identify first, or use fictional/test data only.
- **You remain the author.** Always review every line against the source records before signing, especially medications, doses, allergies, legal status, and risk assessment.
- **Long records:** if a tool cuts off long input, paste only the most relevant documents. Typical set: the current encounter documents, the most recent prior note of the same type, the most recent H&P/discharge summary, the current medication list, and recent labs.
