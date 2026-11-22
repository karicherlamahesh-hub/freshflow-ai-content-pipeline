# How it works

## Overview

FreshFlow uses two AI roles. A **writer** drafts content. A **checker** reviews the draft against the rules. Separating the two roles means the checker is not grading its own work.

## Workflow steps

| Step | Node in n8n | What it does |
|---|---|---|
| 1 | Form trigger | Receives the content request |
| 2 | Message a model (writer) | Drafts the content using the brand guide and returns JSON |
| 3 | Code (parse JSON) | Reads the writer's JSON so later steps can use it |
| 4 | Message a model (checker) | Tests the draft against the 10 brand and compliance rules |
| 5 | Code | Reads the checker's result |
| 6 | Edit Fields | Sets `final_status` |
| 7 | Translator step | Translates approved content into Telugu using a glossary |
| 8 | Notion | Saves the row in the "FreshFlow Content Review" table |

## The three outcomes

| Status | Meaning | What happens next |
|---|---|---|
| Approved | The draft passes every rule | Content is translated and saved for use |
| Refused | The request breaks a rule | Saved as refused, not translated |
| Needs Human Review | The checker is unsure, or the request is too vague | A person reads it and decides |

Example: a request that only says the product "saves time" has no proof or detail behind it. The pipeline sends it to Needs Human Review instead of approving a claim nobody can support.

## Prompts and versions

- The system prompt holds the brand guide and the 10 compliance rules.
- Every prompt change is saved as a new version in the prompt library, with a note on what changed and why.
- Old versions are kept so you can compare results and go back if a change makes things worse.

## Testing method

1. Write 20 test requests: 8 normal and 12 tricky.
2. Run all 20 with the current prompt and score each result.
3. Fix the prompt based on the failures and save it as a new version.
4. Run the same 20 again and compare.
5. Run new requests through the full n8n workflow to check the three outcomes.

The same test set is reused each time so results can be compared fairly.

## Telugu translation

- A glossary lists terms that must not change, such as prices and disclaimers.
- A pricing test confirmed that the disclaimer and the amount (Rs 4,999) stayed exact in the Telugu output.
- A Telugu reader (the author) checked the output.
- Refused requests are not translated.

## Known limits

- The checker is an AI model, so it can miss problems or flag good content. Human review is the safety net.
- The 10 rules and the company are made up for this demo.
- Tests were run on a small set of requests and do not prove accuracy at scale.

## Ideas for next steps

- Add an error log in Notion.
- Add a short demo video.
- Test with more tricky requests and track the results over time.
