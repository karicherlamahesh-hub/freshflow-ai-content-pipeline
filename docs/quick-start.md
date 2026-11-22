# Quick start

This guide takes you from nothing to a working FreshFlow test in about 30 minutes. You do not need to know Python.

## Before you start

You need:

- An Anthropic account with API credit, and an API key
- A Google account (for Google Colab)
- An n8n Cloud account (the free trial lasts 14 days)
- A Notion account

## Step 1: Keep your API key safe

1. Copy your API key from the Anthropic Console.
2. Store it as a secret. In Google Colab, open the key icon in the left sidebar, add a secret named `ANTHROPIC_API_KEY`, and paste the key as its value.
3. Never paste the key into a file you upload to GitHub.

## Step 2: Make your first API call

1. Open a new Google Colab notebook.
2. Install the library by running this in a cell:

   ```
   !pip install anthropic
   ```

3. Copy the code from [Claude API call](api-call.md) into the next cell and run it.
4. You should see a short reply from Claude. If you do, your key and credit work.

## Step 3: Add the brand guide and rules

1. Save the FreshFlow brand guide and the 10 compliance rules in one document.
2. Paste them into the `system` prompt of the API call.
3. Run a request that breaks a rule on purpose, for example one that promises a result the company cannot guarantee. The reply should refuse or flag it.

## Step 4: Keep a prompt library

1. Create a Google Sheet named "FreshFlow Prompt Library".
2. Add one row per prompt version with these columns: version, date, prompt text, what changed, test result.
3. Whenever you change the prompt, add a new row. Do not overwrite the old one.

## Step 5: Run the test set

1. Write 20 test requests: 8 normal and 12 tricky ones (for example, requests that ask for false claims or leave out required details).
2. Run all 20 and score each result as pass or fail.
3. Fix the prompt, save it as a new version, and run the same 20 again.
4. Compare the two runs.

## Step 6: Build the workflow in n8n

1. Create a workflow named "FreshFlow content pipeline".
2. Add the nodes in this order:
   1. Form trigger
   2. Anthropic "Message a model" (writer)
   3. Code (parse the writer's JSON)
   4. Anthropic "Message a model" (checker)
   5. Code (read the checker's result)
   6. Edit Fields (set `final_status`)
   7. Notion (save the row)
3. Add your Anthropic and Notion credentials inside n8n. Do not put them in the workflow itself.
4. Run a test from the form and check the row in Notion.

## Step 7: Add Telugu translation

1. Add a translator step (Claude with a glossary) between Edit Fields and Notion.
2. Put prices, product names and disclaimers in the glossary so they stay exact.
3. Test with a request that contains a price and a disclaimer. Confirm both appear unchanged in the Telugu output.

## Troubleshooting

| Problem | What to check |
|---|---|
| API call fails with an authentication error | The key is correct and stored as a secret |
| API call fails with a credit error | Your Anthropic account has credit |
| n8n cannot read the writer's reply | The writer step must return valid JSON; check the parse step |
| Nothing appears in Notion | The Notion credential has access to the table |
