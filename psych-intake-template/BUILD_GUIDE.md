# Psychiatric Intake Template: Epic Build Guide

Companion to `INITIAL_PSYCH_EVAL_SmartPhrase.txt`.

## 1. Installing it in Epic

1. In Epic, open **SmartPhrase Manager** (Epic button > Tools > SmartPhrase Manager, or search "SmartPhrase").
2. Click **New**, name it `PSYINTAKE`, and paste the contents of the `.txt` file.
3. Share it with yourself (and any colleagues) from the Sharing tab.
4. In a note, type `.PSYINTAKE` then Enter. Press **F2** to jump to each `***`. Press **F3** to open a SmartList.
5. Optional: save it as a **SmartText** or attach it to a Note Template so it appears automatically for new-patient visits.

Tip: do the first build in a copy (`PSYINTAKETEST`) and try it on a test or your own dummy patient before using it live.

## 2. What the symbols mean

| Symbol | Meaning |
|---|---|
| `@NAME@`, `@AGE@` | **SmartLinks.** Epic pulls these from the chart automatically. |
| `{List:ID}` | **SmartList.** A pick-list (F3). Choose one or several options. |
| `***` | **Wildcard.** A blank you must fill in. Epic blocks signing while any remain (a safety feature). |
| `@ME@`, `@TD@` | Your signature block and today's date. |

## 3. SmartLinks used: confirm with your Epic analyst

Names differ by organization. Everything you already used in your example works as-is (`@NAME@ @AGE@ @SEX@ @PMHP@ @SOCHX@ @ALLERGY@ @CMEDS@ @DIAG@ @ME@`). The following are commonly available but **need verification**. Delete any that return an error or stay as literal text:

| In template | Pulls forward | Fallback if unavailable |
|---|---|---|
| `@TD@` | Today's date | Type the date |
| `@DOB@`, `@MRN@` | DOB, MRN | `@AGE@` only |
| `@PCP@` | PCP of record | Type PCP name |
| `@PMH@` | Medical history list | Use `@PMHP@` as in your example |
| `@PSH@` | Surgical history | `@PSURGHX@` at some sites |
| `@FH@` | Family history | `@FAMHX@` at some sites |
| `@VS@` | Recent vitals | Enter vitals manually |
| `@LABRCNT@` | Recent labs | `@LABRSLT@` or a lab SmartLink |
| `@LASTPHQ9@`, `@LASTGAD7@` | Prior scores | Requires a custom SmartLink pointing at the flowsheet rows. Ask the analyst, or delete. |

Useful additions to request: a **last-visit/last-note** SmartLink, **active problem list** (`@PROB@`), and **prior psychiatric medications from medication history**.

## 4. SmartLists to build

Every `{Options/Separated/By/Slashes:***}` in the template is an inline pick-list. Epic can usually convert these on the fly. If your site prefers named SmartLists, ask for:

- **DCPsychSideEffects (28211):** already exists at your site (from your example). Keep it.
- **Visit participants / Reliability / Visit type**
- **MSE descriptors** (affect, thought process, insight, judgment): each should default to the normal option so one keystroke documents a typical exam
- **Boxed warning list** (multi-select, one entry per drug class)
- **Monitoring labs** (multi-select)
- **Billing codes** (multi-select)

**Building defaults:** set the first option in each list as the "normal" default, but keep `***` in required fields (mood quote, chief complaint, diagnosis) so the note cannot be signed with an unreviewed default.

## 5. Efficiency tips for pulling information forward

- **Before the visit:** review the chart and have staff send intake questionnaires (PHQ-9, GAD-7, MDQ, PC-PTSD-5, AUDIT-C, ASRS) through MyChart. Scores then flow into flowsheets.
- **Reconcile first:** do medication, allergy, and problem-list reconciliation in the visit navigator so `@CMEDS@`, `@ALLERGY@`, and `@PMH@` populate accurately.
- **Copy forward carefully:** if the patient is established, you may reuse prior history in later notes, but re-verify social, substance, and safety content each time.
- **Dragon:** Dragon commands such as "next field" can jump between wildcards if configured. Dictate the HPI freely, then use F2 for the rest.
- **Follow-up notes:** consider making a short `.PSYFU` SmartPhrase that reuses the MSE, safety, medication education, and time blocks.

## 6. Using it for any patient vs. one patient

The template is designed to be **patient-neutral**. Everything patient-specific is either a SmartLink (auto-filled) or a wildcard/pick-list (filled by you). Default pertinent-negative statements (denies SI/HI, mania, AVH, paranoia) were kept from your example. **Edit them whenever positive,** because the note should only assert what was actually elicited.

## 7. Clinical and legal content to review before use

This template is a documentation aid and not legal advice. Before using it clinically:

- Have **your compliance/legal team** approve the consent, confidentiality, telehealth, and controlled-substance language to match your **state law** (duty to warn, mandated reporting, minors, PDMP requirements, telehealth consent, prescribing rules).
- Confirm **billing choices** with a certified coder. The E/M time thresholds, 90792, and add-on code rules (90833/90836/90838, 90785, 99417/G2212) change periodically, and payers differ.
- Boxed warnings and monitoring recommendations are summarized from commonly accepted guidance (e.g., APA/ADA antipsychotic monitoring consensus, FDA labeling). **Check current FDA labeling** for each drug you prescribe.
- Keep the Dragon disclaimer if your organization requires it.
- Do not enter real patient information into this repository. The files here contain only the blank template.

## 8. Suggested additions you may want later

- Separate SmartPhrases for **child/adolescent intake** (developmental, school, guardian consent, parent collateral), **geriatric intake** (cognitive screen, capacity, caregiver, fall risk), and **substance use intake** (ASAM dimensions).
- A **safety plan** (Stanley-Brown) SmartText.
- **Prior authorization** and **work/school letter** SmartTexts.
- An **ADHD evaluation add-on** (ASRS, collateral, controlled-substance agreement) and a **lithium/clozapine/LAI monitoring** follow-up block.
