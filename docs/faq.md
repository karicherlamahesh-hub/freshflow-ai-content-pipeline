# FAQ

Short answers to common questions about FreshFlow.

## Do I need to know Python?

No. The [Quick start](quick-start.md) takes you from nothing to a working FreshFlow test in about 30 minutes, and you do not need to know Python.

## What accounts do I need?

You need:

- An Anthropic account with API credit, and an API key
- A Google account (for Google Colab)
- An n8n Cloud account (the free trial lasts 14 days)
- A Notion account

## Where should I keep my API key?

Store it as a secret. In Google Colab, open the key icon in the left sidebar, add a secret named `ANTHROPIC_API_KEY`, and paste the key as its value. Never paste the key into a file you upload to GitHub. See [Step 1 of the Quick start](quick-start.md#step-1-keep-your-api-key-safe).

## What do the writer, checker and translator do?

The writer drafts the content using the brand guide. The checker tests a draft against the 10 compliance rules. The translator translates approved content into Telugu using a glossary. See the [Glossary](glossary.md) for other terms.

## Can I trust the checker?

Not on its own. The checker is an AI model, so it can miss problems or flag good content. Human review is the safety net. The 10 rules and the company are made up for this demo. Read more on [How it works](how-it-works.md).

## My API call shows an error. What should I do?

Check the table of common errors on the [API call](api-call.md#common-errors) page. It lists the likely cause and the fix for a wrong or missing key, no API credit, a wrong model name and a reply that is cut off.
