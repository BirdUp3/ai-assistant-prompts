Test Script Template: Docusign CLM UAT (Contractor Agreements)
Each test script = one user story (from `user-story-template.md`) + a header block + numbered steps. Every step is one action the tester takes, paired with an observable expected result. Fill in every `{{placeholder}}`.
---
1. Script structure
Header block (pre-filled by the author)
Field	Value
Script ID	TS-{{###}}
Feature / Function	{{feature}}
Release / Change Type	{{release}} | {{Enhancement / Fix / Announcement}}
Functional Area	{{area}}
User Story	As a {{persona}}, I want {{capability}} so that {{outcome}}.
Persona(s)	{{persona list}}
Prerequisites / Licensing	{{flag}}. {{settings / permissions / setup}}
Relevance to Contractor Agreements	{{High / Medium / Low}}
Test Data	{{documents, folders, users, values}}
Source	{{release-note URL}}
Execution block (filled in by the tester)
Field	Value
Environment	UAT (default)
Tester	{{tester input}}
Test Date	{{tester input}}
Overall Status	Calculated from the step statuses (see section 4)
Steps table
Step	Action	Expected Result	Actual Result	Status	Comments / Defect ID
1	{{action}}	{{expected}}	tester	tester	tester
2	…	…			
Business sign-off
Approver Name · Role · Sign-off Date · Notes
---
2. Step-writing rules
One action per step. Start with an imperative verb: Open, Select, Enter, Upload, Run, Save, Publish, Start, Check, Compare, Confirm.
Name the role when it changes. Begin the step with "As the {{persona}}, …" whenever the actor differs from the previous step (e.g., Admin sets up, then Reviewer tests).
Use real UI labels from the release note or the CLM User Guide, in single quotes or Title Case (e.g., select Cancel Current Step on the Activity tab). If the menu path isn't certain, describe the destination ("Open the folder's settings") rather than inventing a path.
Use the test data. Refer to named documents, values, and folders, not "a document".
The expected result must be something the tester can see, stated as fact: "The task completes and the workflow advances." Not "It should work."
Include the boundary or negative check the release note implies: permissions (a user without rights can't do it), limits (250 files / 200 MB, 5 reference docs, first 100 pages), defaults (setting is off by default), and limitations ("does not appear in the Obligations dashboard").
Put setup first: admin configuration steps come before user steps in the same script, so the script runs end to end.
End with persistence or downstream proof: reopen the item, check version history, check the next workflow step, or check the integrated system.
Length: Enhancements 4–8 steps · Fixes 2–4 steps · Readiness 3–6 steps.
Plain language. Expected results quote Docusign behavior; they don't speculate. Record limitations as expected results so they aren't logged as defects.
---
3. Step patterns by script type
Enhancement: configure → use → verify → boundary
As the Admin/Builder, enable or configure the feature.
Save or publish.
As the end user, run the feature on the test data.
Confirm the main expected outcome.
Exercise a variation (second option, other report type, other experience).
Negative or limit check (permissions, limits, defaults).
Confirm persistence or the downstream effect.
Fix: reproduce the original condition → confirm corrected behavior → regression
Set up the exact condition the fix describes.
Do the action that used to fail.
Confirm the corrected behavior word for word from the note.
(Optional) Repeat or run the normal path to confirm nothing regressed.
Announcement (readiness): confirm eligibility → act → validate daily tasks → record gaps
Confirm dates, eligibility, site ID, or version.
Do the required action (enable setting, update software, remediate expressions).
Run the legal team's everyday tasks under the new behavior.
Record gaps or dependencies for change management.
Integration (Coupa / Ariba / Workday / MCP): trigger in source → check target → mismatch check
Confirm connector prerequisites (authorization, region).
Trigger the event in the source system (test tenant).
Confirm the record or document in the target system.
Compare key metadata; test one mismatch or unauthorized case.
---
4. Status values and overall status rule
Step Status	Use when
Pass	The actual result matches the expected result
Fail	The actual result differs. Enter a defect ID
Blocked	Can't run (setup, licensing, environment)
N/A	Doesn't apply to this client's configuration. Explain in Comments
Overall Status = `Fail` if any step failed → else `Blocked` if any step is blocked → else `Pass` if every step is Pass or N/A → else `Not Started` if no status has been entered → else `In Progress`.
---
5. Quality checklist (every script)
[ ] Header block complete; story matches `user-story-template.md`
[ ] Every step is one action with one observable expected result
[ ] Role changes are stated at the start of the step
[ ] Setup steps come before user steps; the script runs end to end on its own
[ ] At least one boundary, negative, or limitation check where the note implies one
[ ] Final step proves persistence or the downstream effect
[ ] Step count is within the range for the script type
[ ] Actual Result, Status, and Comments are left blank for the tester
---
6. Filled example
Field	Value
Script ID	TS-064
Feature / Function	Cancel human steps in CLM workflows
Release / Change Type	July 2026 | Enhancement
Functional Area	Workflow
User Story	As a CLM Administrator or authorized Legal Reviewer, I want to cancel an approval step that went to the wrong person so that I can re-route the contractor agreement without restarting the workflow.
Persona(s)	Workflow Builder (CLM Admin); Legal Reviewer (Attorney / Paralegal)
Prerequisites / Licensing	Core CLM
Relevance	High
Test Data	Contractor Agreement workflow with Approve and Review and Send for Signature steps; a Legal Reviewer user given the "Workflow Step Cancellation" delegated permission.
Source	{{July 2026 Enhancements URL}}

Step	Action	Expected Result
1	In the Workflow Designer, open the Approve step and select Enable Step Cancellation.	The setting saves.
2	Under Allow additional users or task groups to cancel workflow step, add the Legal Ops task group.	The users or task groups save.
3	Connect the step's Step Cancelled output to a re-route step, then publish.	The workflow publishes. Without this connection, cancellation could cause a workflow failure.
4	Start the workflow on the contractor agreement so the Approve task is assigned.	The task is pending.
5	As an authorized user, open the document in Document Review > Activity and select Cancel Current Step.	The step is cancelled with "Step Cancelled" output and the workflow continues on the connected path.
6	As a user without cancellation rights, open the same document's Activity tab.	Cancel Current Step is not available.
7	Grant a user the Workflow Step Cancellation delegated permission and retest.	That user can now cancel.
