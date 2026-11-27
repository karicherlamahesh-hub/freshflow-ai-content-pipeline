# API quick start

This page shows how to start the FreshFlow workflow with one cURL command, and what each answer means. For every field and response, see the [API reference](api-reference.md).

## Before you start

You need:

- The FreshFlow workflow running in n8n, with its Webhook node set to the POST method.
- Your own webhook address from the Webhook node.
- A terminal that has `curl` (Git Bash, macOS Terminal and most Linux terminals have it).

!!! note
    The addresses on this page are placeholders. Replace `YOUR-N8N-HOST` and `your-webhook-path` with your own values. Never share your real webhook address in a public place.

## Step 1: Save your address

Save your webhook address in a variable, so you do not type it again and again:

```bash
export FRESHFLOW_URL="https://YOUR-N8N-HOST/webhook-test/your-webhook-path"
```

The address has two forms:

| Mode | Address starts with | When it works |
|---|---|---|
| Test | `https://YOUR-N8N-HOST/webhook-test/` | For one call, after you click **Execute workflow** in n8n |
| Production | `https://YOUR-N8N-HOST/webhook/` | Any time the workflow is active |

## Step 2: Send a request

The body has one field, `request`. Write what you want and the facts the post may use.

```bash
curl -X POST "$FRESHFLOW_URL" \
  -H "Content-Type: application/json" \
  -d '{"request": "Write a short LinkedIn post about our new inventory tracking feature. Use only these facts: live stock counts for each shop, an alert when stock drops below 10 units, and mobile support. Do not add benefits or results that are not listed. Do not mention price."}'
```

## Step 3: Read the answer

A good call returns this:

```json
{"message": "Workflow was started"}
```

!!! warning
    This answer means the workflow **started**. It does not mean the post is ready. The finished post is saved to Notion, not returned in the answer.

Open your FreshFlow review table in Notion to see the post, its status and the checker notes. A new row can take about a minute to appear.

## What the answers mean

| What you see | What it means | What to do |
|---|---|---|
| `200` and `{"message": "Workflow was started"}` | The workflow started. The post is not finished yet. | Open the review table in Notion and find the post. |
| `404` and "The requested webhook ... is not registered" | You used the test address, and n8n is not listening. | Click **Execute workflow** in n8n, then send the call again. The test address works for one call each time. |
| `200`, but no post appears in Notion | The `request` field is missing or empty. The answer is still 200, because the workflow starts first and fails inside. | Check that the body has a `request` field with text. Then look at the failed run in the n8n **Executions** list. |
| `200`, and the post is saved with the status **Needs human review** | The checker found a sentence that is not backed by your facts. The reason is in the **Checker Notes** column. | Read the note, fix the post or the request, and send the call again if needed. |
| `200`, and the post is saved with the status **Refused** | The request has no facts, so the content check refused to write the post. | Add the facts the post may use, then send the call again. |

## Next step

For every field, example and response, read the [API reference](api-reference.md).
