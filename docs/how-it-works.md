# How it works

## Overview

FreshFlow uses two AI roles. A **writer** drafts content. A **checker** reviews the draft against the rules. Separating the two roles means the checker is not grading its own work.

## Workflow steps

| Step | Node in n8n | What it does |
|---|---|---|
| 1 | Content request form | Receives the content request |
| 2 | Writer | Drafts the content using the brand guide and returns JSON |
| 3 | Parse writer output | Reads the writer's JSON so later steps can use it |
| 4 | Checker | Tests the draft against the 10 brand and compliance rules |
| 5 | Parse checker result | Reads the checker's result |
| 6 | Set final status | Sets `final_status` |
| 7 | Translator | Translates approved content into Telugu using a glossary |
| 8 | Parse translation | Reads the Telugu translation |
| 9 | Save to Notion | Saves the row in the "FreshFlow Content Review" table |
| Error path | Save error to Notion | If the Writer step fails, saves the error in Notion |

## The three outcomes

| Status | Meaning | What happens next |
|---|---|---|
| Approved | The draft passes every rule, or the checker fixed it and explained the changes | Content is translated and saved for human review before publishing |
| Refused | The request cannot be written safely, or has no facts to work from | Saved as refused, not translated |
| Needs Human Review | The checker is unsure, or the request is too vague | A person reads it and decides |

Examples from test runs:

- A request that only said the product "saves time", with no proof or detail, went to Needs Human Review in an earlier test.
- A request for a stock tracking intro with no product facts was refused, and the checker asked for facts.
- A request asking for a guaranteed 40% cost cut and a competitor comparison was approved after the checker removed those claims and explained the changes in Checker Notes.

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
