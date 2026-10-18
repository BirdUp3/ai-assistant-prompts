User Story Template: Docusign CLM Release Notes → UAT User Stories
Use this template to turn each Docusign CLM release-note item into one user story. The user story is the first part of every test script (see `test-script-template.md`). Fill in every `{{placeholder}}`. Don't leave a field blank: write `None` or `N/A` instead.
---
1. Triage: script it or log it?
Read the release note's three pages (Announcements, Enhancements, Fixes) and sort every item:
Decision	When	What to produce
Script	A user or admin can do something in CLM (or a connected add-in/integration) and see a result.	User story + test script
Readiness script	An announcement that changes behavior or needs action before a deadline (e.g., default experience changes, Docusign Edit update, expression migration, Azure migration).	User story + readiness test script
Not Scripted	Scheduling, infrastructure, or informational only (release windows, IP or certificate notices, API shutdowns with no UI, product renames, status-page changes, "no updates" pages, sections not yet published).	One row in the Not Scripted log with the reason, or the script ID that already covers it
One feature per script.
Split a heading that bundles several features (e.g., "Enhancements to AI-Assisted Review" with four sub-features becomes four stories).
Combine items that are the same feature across releases (e.g., a Limited Availability release in March and General Availability in April become one story). Log the later item as Not Scripted with "Covered by TS-###".
Announcements repeated across releases are covered once. Log the repeats.
---
2. User story fields
```
Script ID:            TS-{{###}}
Feature / Function:   {{Short name of the single feature, using Docusign's wording}}
Release:              {{e.g., July 2026 | 26.6.0 (Oct 2026)}}
Change Type:          {{Enhancement | Fix | Announcement}}
Functional Area:      {{pick from the Area list in section 5}}
User Story:           As a {{persona}}, I want {{capability, described in contractor-agreement terms}}
                      so that {{business or legal outcome for the real estate legal team}}.
Persona(s):           {{one or more from the Persona catalog in section 3; separate with ";"}}
Prerequisites /
Licensing:            {{flag from the Licensing catalog in section 4}}. {{settings, permissions, setup data}}{{" | Limited Availability" if applicable}}
Relevance:            {{High | Medium | Low}}
Test Data:            {{specific documents, folders, users, values}}
Source:               {{exact URL of the release-note page (Announcements / Enhancements / Fixes)}}
```
Writing rules for the user story sentence
Persona: the person who gets the value, not always the person who sets it up. If an admin configures something and a reviewer uses it, the story is the reviewer's. List both in Persona(s).
"I want": describe the capability as it applies to contractor agreements (independent contractor agreements, MSAs with service vendors, change orders, COIs, lien waivers, W-9s, scopes of work, property/portfolio folders). Don't copy the release note word for word.
"So that": a concrete outcome the legal team cares about, such as risk reduction, speed, audit readiness, accuracy, fewer handoffs, or compliance. Avoid vague outcomes like "it's better".
Keep it to one sentence of 25–45 words.
For Fixes, state the corrected behavior as the want (e.g., "I want validation errors to appear on the panel where I entered the bad value").
For Readiness items, frame the story around validating before the deadline (e.g., "I want to validate my daily tasks in Document Review now so that the January 2027 switch causes no disruption").
---
3. Persona catalog
Persona	Typical role at the client	Use for
Legal Reviewer (Attorney / Paralegal)	In-house counsel, paralegals	Reviewing, redlining, approving, tagging attributes, sending for signature
Contract Requester (Project / Property Manager)	Property, construction, or facilities managers	Doc gen forms, intake requests, uploading contractor paper
CLM Administrator	CLM system owner	Account settings, users, folders, reports, licensing
Workflow Builder (CLM Admin)	CLM admin who builds workflows	Workflow Designer, steps, expressions, XML, HTTP Client
Playbook Manager (Legal Ops)	Legal operations	AI-Assisted Review playbooks, rules, clause library
IT / CLM Administrator	IT plus the CLM admin	Allowlists, add-in manifests, Docusign Edit deployment, migrations
Integration Owner (Procurement / Finance Systems)	Procurement or finance systems	Coupa, SAP Ariba, Workday, MCP/AI assistant connections
---
4. Licensing / prerequisite flag catalog
Start the Prerequisites / Licensing field with exactly one of these flags, then add the specific settings or setup.
Flag	Meaning
`Core CLM`	Included in standard CLM
`AI-Assisted Review add-in for Word (playbook permissions)`	Needs the AI-Assisted Review add-in
`Iris AI: CLM+, AI Extension for CLM, or IAM plan (not CLM.CM / Essentials / IL4)`	Iris attribute suggestions, Use AI Data Insights
`IAM plan (CLM + IAM / Agreement Manager)`	CLM + IAM integration features
`Docusign CLM Connector for {{Coupa / SAP Ariba / Workday Financials}}`	Procurement or finance integrations
`Docusign MCP server connected to an approved AI assistant`	MCP features
`Applies once {{event}} occurs`	Readiness checks tied to a date or event
Add ` | Limited Availability` when the release note says so. Add region, locale, or site limits word for word (e.g., "North America, English only", "not IL4").
---
5. Functional area list
Document Review · Document Generation · Workflow · Workflow / AI · Reporting · Search · Unified Task Management · AI-Assisted Review · Clause Library · Folder Administration · User Administration · CLM + IAM · Agreement Manager for CLM · Docusign Edit · Platform Readiness · Integrations - {{Coupa | SAP Ariba | Workday | AI Assistant}}
Combine areas with " / " when a feature spans two (e.g., `Workflow / eSignature`).
---
6. Relevance rating
Rating	Rule
High	Used directly in the contractor agreement lifecycle: request, review, redline, approve, sign, store, find, or report.
Medium	Supporting or admin capability the contractor process depends on (setup, reporting, workflow plumbing).
Low	Depends on a license, integration, locale, or edge case the client may not use.
---
7. Standard test data (reuse across stories)
`Independent Contractor Agreement - Apex Roofing LLC - 250 Park Ave (UAT)`: v1 company paper, v2 contractor redlines (Payment Terms, Indemnification), Exhibit A (Scope), COI, W-9.
`Master Services Agreement - Harbor HVAC Services - Riverside Portfolio (UAT)`: parent agreement for hierarchy tests.
Folders: `Contractor Agreements > Riverside Portfolio > 250 Park Ave`; `Contractor Agreements > Inbound Submissions`.
Use fictitious or anonymized data only. No live tax IDs or contractor PII in UAT.
---
8. Quality checklist (every story)
[ ] Covers exactly one feature or fix
[ ] Persona comes from the catalog; the story reads from the beneficiary's point of view
[ ] "So that" names a real legal or business outcome
[ ] Licensing flag present; Limited Availability and region limits copied from the note
[ ] Relevance rated with the rule in section 6
[ ] Test data is specific (named documents, values, folders)
[ ] Source URL points to the exact release-note page
---
9. Filled examples
Enhancement
```
Script ID:            TS-064
Feature / Function:   Cancel human steps in CLM workflows
Release:              July 2026
Change Type:          Enhancement
Functional Area:      Workflow
User Story:           As a CLM Administrator or authorized Legal Reviewer, I want to cancel an approval
                      step that went to the wrong person so that I can re-route the contractor
                      agreement without restarting the workflow.
Persona(s):           Workflow Builder (CLM Admin); Legal Reviewer (Attorney / Paralegal)
Prerequisites /
Licensing:            Core CLM
Relevance:            High
Test Data:            Contractor Agreement workflow with Approve and Review and Send for Signature steps;
                      a Legal Reviewer user given the "Workflow Step Cancellation" delegated permission.
Source:               {{July 2026 Enhancements URL}}
```
Fix
```
Script ID:            TS-050
Feature / Function:   Fix: correct dates when using the "Edit CLM Agreement Attributes" step
Release:              May 2026
Change Type:          Fix
Functional Area:      Workflow
User Story:           As a Paralegal, I want workflow-set dates to be correct so that contractor term and
                      expiration dates drive accurate renewal alerts.
Persona(s):           Workflow Builder (CLM Admin)
Prerequisites /
Licensing:            Core CLM
Relevance:            High
Test Data:            Effective Date 03/04/2026 and Expiration Date 03/03/2027 (dates where day/month order matters).
Source:               {{May 2026 Fixes URL}}
```
Not Scripted log row
```
Release:  May 2026 | Type: Announcement
Item:     Navigator renamed Agreement Manager; Maestro renamed Workflow Builder
Reason:   Label change only, with no functional impact. Update training materials and test scripts to use the new names.
Source:   {{May 2026 Announcements URL}}
```
