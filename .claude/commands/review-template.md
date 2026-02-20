Review `templates/CLAUDE.md` as its intended reader — an AI coding assistant — and suggest improvements that will make the instructions clearer and more effective.

## Instructions

1. Read `templates/CLAUDE.md`.

2. Read the root `CLAUDE.md` to understand the principles that govern what makes a good rule in this project.

3. Evaluate the template against the following criteria, drawn from those principles:

   - **Actionable** — Can an AI follow or avoid the described behaviour specifically? Flag rules that are vague, describe outcomes rather than behaviours, or leave the AI to interpret what "good" looks like.
   - **Testable** — Could a reader verify from the AI's output that the rule was followed? Flag rules where compliance is invisible or entirely subjective.
   - **Concise** — Does the rule use the minimum words needed? Flag rules that pad with qualifications, restate context unnecessarily, or repeat other rules.
   - **Non-redundant** — Does the rule override a default or enforce something project-specific? Flag rules that restate behaviour an AI would exhibit without being asked.
   - **Well-ordered** — Are the most critical rules (security, communication) positioned before less critical ones? Flag rules that appear in the wrong section or out of priority order.
   - **Non-conflicting** — Does the rule contradict or undermine any other rule in the file?

4. For each issue found, report:
   - The exact rule text with the problem
   - Which criterion it fails and a concise explanation of why
   - Proposed replacement wording — exact text, not a description of what it would say

5. If a section has no issues, say so briefly rather than skipping it.

6. End with a short overall assessment: how close is the template to the standard described in the root `CLAUDE.md`, and what is the highest-priority change?

Do not suggest changes based on general best practices alone. Ground every suggestion in the criteria above and the intent expressed in the root `CLAUDE.md`.
