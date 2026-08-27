# Template: SKILL.md for Sokigo's writing rules

Copy, fill in, delete what doesn't fit. The frontmatter block at the top is what makes the file a real skill; during the workshop it's enough to paste the whole file into a chat.

```markdown
---
name: sokigo-writing-rules
description: Sokigo's writing rules and tone of voice. Use for all text aimed at Sokigo's customers or users - release notes, help articles, UI copy, guides.
---

# Sokigo Writing Rules

## Tone and address
- Address: [you (formal/informal)?] Write to the reader, not about the reader.
- Tone: [factual? warm? public-sector-adjacent?] Describe with 2-3 adjectives and one example.
- Active voice: "You export the report" - not "The report can be exported".

## Terminology (extend this list!)
| Write | Do NOT write |
|---|---|
| case | ticket, issue |
| caseworker | administrator (when the role is meant) |
| [your term] | [wrong term] |

## Structure for release notes
- Heading: what the user can do now - not what we built.
- Order: new features, improvements, fixes.
- Each item: benefit first, detail second. Max 2 sentences per item.
- Internal references (issue numbers, developer names, branch names) are removed.

## Forbidden
- Exclamation marks in factual content.
- "Easy", "seamless", "powerful" without evidence.
- Jargon where a plain term exists.
- [add your house sins]

## Examples

WRONG: "Fixed the export bug (SOK-4711), thanks Mike!"
RIGHT: "PDF export now works for cases with more than 100 attachments."

WRONG: "We have implemented a new seamless filtering feature!"
RIGHT: "You can now filter the case list by caseworker and status."

[Add at least 3 of your own WRONG/RIGHT pairs - they do more work than all the rules above.]
```

## Tips while filling it in

- **Examples beat rules.** Claude generalizes well from 3–5 WRONG/RIGHT pairs.
- **Write rules for the mistakes Claude actually makes** – run the baseline first and codify what you see.
- **The description line matters** – it decides when the skill activates in real use.
