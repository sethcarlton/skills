---
name: complete-ticket
description: Finish a ticket after implementation by committing its changes with a closing issue reference and updating follow-up issues only when needed.
---

# Complete Ticket

1. Identify the issue from the conversation or branch; ask if unclear. Read its requirements and inspect the uncommitted changes.
2. Confirm the requirements are met and relevant checks pass. Stop and report blockers rather than closing unfinished work.
3. Load `/committing`. Stage only this ticket's changes, preserving unrelated work, and commit with `Closes #<issue>` (or a qualified issue reference) in the message body.
4. Update follow-up issues only if implementation changed facts they need: dependencies, interfaces, constraints, or remaining work. Add only the missing context; otherwise skip.
5. Report the commit and any issue updates. Note that the closing reference takes effect when the commit reaches the default branch; do not push unless asked.
