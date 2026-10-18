Fixed Output Format: Docusign CLM UAT Test Scripts
Every run of the skill produces exactly the outputs below, in this structure, with no extra sections. The content rules are in `user-story-template.md` and `test-script-template.md`. This file covers the format only.
---
Output 1: Excel workbook (the deliverable)
File name: `Docusign_CLM_{{YYYY}}_UAT_Scripts_{{User_Group}}.xlsx` (e.g., `Docusign_CLM_2026_UAT_Scripts_Contractor_Agreements.xlsx`)
Global styling: Arial 10 throughout · header rows: navy fill `#1F3864`, white bold text · label cells: grey fill `#F2F2F2`, bold · tester-input cells: yellow fill `#FFF2CC` · thin grey borders `#BFBFBF` · wrapped text, top-aligned · gridlines hidden on Read Me, Summary, and script tabs · landscape printing, fit to page width.
Status colors (conditional formatting everywhere status appears): Pass = green `#C6EFCE` / `#006100` · Fail = red `#FFC7CE` / `#9C0006` · Blocked = amber `#FFEB9C` / `#9C5700` · In Progress = blue `#DDEBF7` / `#1F4E78`.
Tab order (fixed)
`Read Me`
`Summary`
`Script Index`
`Not Scripted`
`TS-001` … `TS-###` (one tab per script, in ID order)
Script ID numbering
`TS-` + 3 digits, assigned in release order (oldest first). Within a release: Enhancements → Fixes → Announcements (readiness), following the order they appear on each page.
Tab: Read Me
Sections in this order: Title (`Docusign CLM {{YYYY}} Release UAT Scripts: {{User Group}}`) and subtitle (client industry · stakeholder · user group · prepared date) → Purpose (scope, release notes covered, script count) → How to run a script (numbered) → Legend (yellow = input; Pass / Fail / Blocked / N/A) → Example of a completed step (one filled row: Step, Action, Expected, Actual, Status) → Standard test data → Source release notes (one hyperlink per release + CLM User Guide link).
Tab: Summary (all formulas, no typed numbers)
Table 1 columns: `Release | All Scripts | In Scope | Not Started | In Progress | Pass | Fail | Blocked`, one row per release, plus a Total row (SUM).
`Pass rate (in scope)` = Pass ÷ In Scope · `Executed (Pass + Fail)` = (Pass + Fail) ÷ In Scope (format `0.0%`).
Table 2 columns: `Relevance | All Scripts | In Scope | Pass | Fail` for High / Medium / Low.
Counts use `COUNTIFS` against Script Index and include only rows where In Scope = `Y`.
Tab: Script Index (row 1 = headers, freeze panes at B2, autofilter on)
Col	Header	Content
A	Script ID	Hyperlink to the script tab
B	Release	e.g., `July 2026`, `26.6.0 (Oct 2026)`
C	Type	Enhancement / Fix / Announcement
D	Area	Functional area
E	Feature / Function	Feature name
F	User Story	Full sentence
G	Persona(s)	Persona list
H	Prerequisites / Licensing	Licensing flag + setup
I	Relevance	High / Medium / Low
J	In Scope (Y/N)	Default `Y`; Y/N dropdown; yellow
K	# Steps	`=COUNTA('TS-###'!B20:B{{last}})`
L	Status	`='TS-###'!$C$17`
M	Tester	Yellow input
N	Target Date	Yellow input, `yyyy-mm-dd`
O	Source	Hyperlink "Release note"
Tab: Not Scripted (freeze row 1)
Columns: `Release | Type | Release Note Item | Reason Not Scripted / Where Covered | Source`. Highlight rows for unpublished or pending content in amber.
Script tabs: `TS-###` (fixed cell layout)
Rows	Content
1	Title: `TS-### | {{Feature}}`: Arial 14 bold navy, merged A1:F1
2	Hyperlink `<- Back to Script Index`
4–17	Header block. Labels merged A:B (grey); values merged C:F
19	Steps header row (navy)
20 →	One row per step
last + 2 →	Business Sign-off: Approver Name, Role, Sign-off Date, Notes (yellow inputs)
Header block rows (fixed): 4 Script ID · 5 Feature / Function · 6 Release / Change Type · 7 Functional Area · 8 User Story · 9 Persona(s) · 10 Prerequisites / Licensing · 11 Relevance to Contractor Agreements · 12 Test Data · 13 Source (hyperlink) · 14 Environment (yellow, default `UAT`) · 15 Tester (yellow) · 16 Test Date (yellow) · 17 Overall Status (formula, bold).
Steps columns (fixed widths): `A Step (7) | B Action (58) | C Expected Result (52) | D Actual Result (40, yellow) | E Status (12, yellow, dropdown Pass/Fail/Blocked/N/A) | F Comments / Defect ID (28, yellow)`.
Overall Status formula (C17), where `R` = `E20:E{{last}}`:
```
=IF(COUNTIF(R,"Fail")>0,"Fail",IF(COUNTIF(R,"Blocked")>0,"Blocked",
 IF(COUNTIF(R,"Pass")+COUNTIF(R,"N/A")=ROWS(R),"Pass",
 IF(COUNTA(R)=0,"Not Started","In Progress"))))
```
Repeat row 19 as the print title row.
Validation before delivery
Recalculate all formulas; zero formula errors.
Every Script Index row links to an existing tab; # Steps > 0; Status = `Not Started` on a fresh workbook.
Every release note item appears either in Script Index or in Not Scripted.
---
Output 2: Chat response (fixed, after delivering the workbook)
Plain prose plus one short list. No tables or headings. In this order:
One sentence: what was built: `{{N}}` scripts / `{{N}}` steps covering `{{list of releases}}`.
Bullets (3–5, bold lead-ins):
Structure: tabs and what the tester fills in.
Scope / licensing: how many are High relevance; which add-ons are flagged; In Scope = N to prune.
Not scripted: what was excluded and why, in one line.
Gaps / dates: unpublished release-note sections, upcoming UAT or Production deployment dates.
One sentence: recommended next step (e.g., dry-run the High-relevance scripts in UAT).
Sources: one markdown link per release note used.
---
Output 3: Project index (if a Project is attached)
Markdown file `claude/clm-{{YYYY}}-uat-script-index.md`:
Header lines: deliverable file name, build date, coverage, decisions, OPEN items.
Table: `| ID | Release | Type | Feature | Relevance | Prerequisites |`
`## Not scripted` bullet list: `Release (Type): Item. Reason`.