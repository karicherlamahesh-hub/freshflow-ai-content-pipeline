# FreshFlow AI Content Pipeline

A writer-plus-checker pipeline that drafts marketing content with the Claude API, checks every draft against 10 brand and compliance rules, and sends each request to one of three outcomes: **Approved**, **Refused** or **Needs Human Review**.

FreshFlow Software is a made-up company (software for food and manufacturing businesses in India). This is a personal portfolio project built to practise AI content workflows, prompt versioning and output testing.

## What it does

1. A user submits a content request through a form.
2. A **writer** step (Claude) drafts the content using the FreshFlow brand guide.
3. A **checker** step (Claude) tests the draft against 10 brand and compliance rules.
4. The result is saved to a Notion table with one of three statuses:
   - **Approved**: the draft passes all rules. A person still reviews it before publishing.
   - **Refused**: the request breaks a rule and cannot be written safely.
   - **Needs Human Review**: the checker is not sure, so a person decides.
5. Approved content gets a **Telugu translation** step that uses a glossary, so prices and disclaimers stay exact.

## Tools used

| Tool | Purpose |
|---|---|
| Claude API (`claude-haiku-5-5`) | Writer, checker and translator steps |
| n8n Cloud | Connects the steps into one workflow |
| Notion | Stores results in the "FreshFlow Content Review" table |
| Google Colab | Early prompt testing |
| Google Sheets | Prompt library with version history |

## Project status and results

- Prompts were versioned in a prompt library and improved after testing.
- 20 test requests (8 normal, 12 tricky) were run twice, before and after prompt fixes.
- The n8n workflow was tested with 6 requests: 3 approved and 3 refused. A vague request ("saves time") now goes to Needs Human Review.
- A pricing test confirmed that the price disclaimer and the amount (Rs 4,999) stay exact in the Telugu translation.

## Documentation

| Document | What it covers |
|---|---|
| [Quick start](docs/quick-start.md) | Set up and run the pipeline step by step |
| [How it works](docs/how-it-works.md) | Workflow steps, statuses and testing method |
| [Claude API call](docs/api-call.md) | One API call explained line by line |
| [Brand guide and rules](docs/rules.md) | The brand guide and 10 compliance rules |

## Limits

- This is a demo with a fictional company. Do not use it for real compliance decisions.
- The checker is an AI model and can make mistakes. That is why uncertain cases go to human review.
- Never commit your API key. See the quick start for how to keep it safe.

## Author

Mahesh Karicherla, Hyderabad. [LinkedIn](https://www.linkedin.com/in/mahesh-k-li)
