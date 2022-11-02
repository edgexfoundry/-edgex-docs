# Architecture Decision Records Folder
This folder contains EdgeX Foundry decision records (ADR) and legacy design / requirement documents.

    /design
        /adr (architecture decision Records)
        /legacy-design (legacy design documents)
        /legacy-requirements (legacy requirement documents)

At the root of the ADR folder (/design/adr) are decisions that are relevant to multiple parts of the project (aka � *cross cutting concerns*).  Sub folders under the ADR folder contain decisions relevant to the specific area of the project and essentially set up along working group lines (security, core, application, etc.).

## Naming and Formatting
ADR documents are requested to follow RFC (request for comments) naming standard.  Specifically, authors should name their documents with a sequentially increasing integer (or serial number) and then the architectural design topic:  (sequence number - topic).  Example:  0001-SeparateConfigurationInterface.  The sequence is a global sequence for all EdgeX ADR.  
Per RFC and Michael Nygard [suggestions](https://github.com/joelparkerhenderson/architecture-decision-record/blob/main/templates/decision-record-template-by-michael-nygard/index.md) the makeup of the ADR document should generally include:

-	Title
-	Status (proposed, accepted, rejected, deprecated, superseded, etc.)
-	Context and Proposed Design
-	Decision
-	Consequences/considerations
-	References
-	Document history is maintained via Github history.

## Ownership
EdgeX WG chairman own the sub folder and included documents associated to their work group.  The EdgeX TSC chair/vice chair are responsible for the root level, cross cutting concern documents.

## Review and Approval

ADR’s shall be submitted as PRs to the appropriate edgex-docs folder based on the Architecture Decision Records Folder section above.  The status of the PR (inside the document) shall be listed as *proposed* during this period.  The PRs shall be left open (not merged) so that comments against the PR can be collected during the proposal period.  The PRs can be approved and merged only after a formal vote of approval is conducted by the TSC.  On approval of the ADR by the TSC, the status of the ADR should be changed to *accepted*.  If the ADR is not approved by the TSC, the status in the document should be changed to *rejected* and the PR closed.

## Legacy
A separate folder (/design/legacy-design) is used for legacy design/architecture decisions.
A separate folder (/design/legacy-requirements) is used for legacy requirements documents.
WG chairman take the responsibility for posting legacy material in to the applicable folders.

## Table of Contents
A README with a table of contents for current documents is located [here](./TOC.md). Legacy Design and Requirements have their own Table of Contents as well and are located in their respective directories at [/legacy-design](./legacy-design/README.md) and [/legacy-requirements](./legacy-requirements/README.md). Document authors are asked to keep the TOC updated with each new document entry.