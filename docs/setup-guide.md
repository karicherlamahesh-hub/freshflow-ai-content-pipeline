# Set up FreshFlow on Linux with Docker

This guide shows you how to run FreshFlow on a Linux machine with Docker. You use GitHub Codespaces, so you need no install on your own computer. At the end, a test request creates a new row in your Notion table.

## Before you start

You need:

- A GitHub account with **Codespaces**. The free plan gives 120 hours of compute a month.
- A web browser.
- An Anthropic API key.
- A Notion integration token, and the **FreshFlow Content Review** database shared with that integration.
- The workflow file `docs/freshflow-workflow.json` in your repo.

!!! warning "Keep your keys private"
    Paste your API key and your Notion token only into the credential boxes in n8n. Never put them in a file, in your repo, in the terminal or in a screenshot.

## Steps

### 1. Open a Linux terminal

1. Open your FreshFlow repo on GitHub.
2. Click **Code**, then the **Codespaces** tab.
3. Click **Create codespace on main**.

A code editor opens in your browser. At the bottom you should see a terminal with a prompt that ends in `(main) $`.

### 2. Check that Docker is installed

In the terminal, run:

```bash
docker --version
```

You should see a line that starts with `Docker version`.

Then run:

```bash
docker ps
```

You should see only a header line (`CONTAINER ID`, `IMAGE`, and so on). An empty list means no container is running.

### 3. Create a storage volume

n8n keeps your workflows and credentials in a Docker volume. Run:

```bash
docker volume create n8n_data
```

You should see `n8n_data` printed on the next line.

### 4. Download n8n

Run:

```bash
docker pull docker.n8n.io/n8nio/n8n
```

This takes 1 to 3 minutes. You should see many lines that end with `Pull complete`, and a last line that starts with `Status: Downloaded newer image`.

### 5. Start n8n

Run this as one line:

```bash
docker run -it --rm --name n8n -p 5678:5678 -e GENERIC_TIMEZONE=Asia/Kolkata -e TZ=Asia/Kolkata -e N8N_ENFORCE_SETTINGS_FILE_PERMISSIONS=true -e N8N_RUNNERS_ENABLED=true -e N8N_PROXY_HOPS=1 -e WEBHOOK_URL="https://${CODESPACE_NAME}-5678.${GITHUB_CODESPACES_PORT_FORWARDING_DOMAIN}/" -e N8N_EDITOR_BASE_URL="https://${CODESPACE_NAME}-5678.${GITHUB_CODESPACES_PORT_FORWARDING_DOMAIN}/" -v n8n_data:/home/node/.n8n docker.n8n.io/n8nio/n8n
```

If you are not in India, change the two timezone values.

Wait about 30 seconds. You should see a line that starts with `Version:` and the lines `Editor is now accessible via`.

The terminal stays busy while n8n runs. Do not close it and do not press **Ctrl + C**.

!!! note
    You may see notices about settings that will change in a future version. They are not errors. A pop-up may say that port 5678 is available. Do not click **Make Public**.

### 6. Open n8n in your browser

1. In the panel at the bottom, click the **Ports** tab.
2. Find port **5678**. Check that **Visibility** says **Private**.
3. Hover over the address and click the globe icon (**Open in Browser**).

You should see the page **Set up owner account**.

4. Enter your email, your name and a new password. This account lives only inside your own n8n.
5. Click **Next**. If n8n asks questions about you, click **Get started**. If it offers a free licence key, click **Skip**.

### 7. Import the workflow

1. In the left menu, click **+**, then **Workflow**.
2. Click the three dots (**...**) next to the workflow name, then **Import from URL**.
3. Enter this address:

```text
https://raw.githubusercontent.com/karicherlamahesh-hub/freshflow-ai-content-pipeline/main/docs/freshflow-workflow.json
```

If you use your own copy of the repo, replace `karicherlamahesh-hub` with your GitHub user name.

You should see the workflow with the nodes **Webhook**, **Writer**, **Checker**, **Translation** and **Save to Notion**. Some nodes have a red mark because they have no credentials yet.

### 8. Add your credentials

1. Double-click the **Writer** node.
2. Next to **Credential**, click **Connect to Anthropic**, then create a new credential.
3. Paste your API key into **API Key** and click **Save**.
4. Open the **Checker** node and the **Translation** node. In each one, choose the same credential from the **Credential** list.
5. Double-click **Save to Notion**. Create a new Notion credential, paste your token into **Internal Integration Secret** and click **Save**.
6. Open **Save error to Notion** and choose the same Notion credential.
7. Press **Ctrl + S**.

You should see no red marks on the nodes.

### 9. Publish the workflow

Click **Publish** at the top right.

In a codespace you may see **Connection lost** at the top right instead of the **Publish** button. This is a known problem with the Codespaces proxy. Publish the workflow from the command line instead:

1. In the terminal panel, click the **+** icon to open a second terminal.
2. Run:

```bash
ID=$(docker exec n8n n8n list:workflow | cut -d'|' -f1)
docker exec n8n n8n publish:workflow --id="$ID"
```

You should see a line that starts with `Publishing workflow with ID:`.

### 10. Save your webhook path

Your workflow starts when a request reaches its webhook. You send the request from the terminal, so you need the path.

1. In n8n, double-click the **Webhook** node.
2. In the **Path** box, press **Ctrl + A**, then **Ctrl + C**. Close the node.
3. In the second terminal, run:

```bash
read -rs P
```

4. Press **Ctrl + Shift + V** once to paste, then press **Enter**. Nothing shows on screen.
5. Check the length:

```bash
echo -n "$P" | wc -c
```

You should see `36`.

## Check that it works

### Send a test request

Run this as one line:

```bash
curl -s -w "\nHTTP %{http_code}\n" -X POST "http://localhost:5678/webhook/$P" -H "Content-Type: application/json" -d '{"request":"Write a short LinkedIn post about FreshFlow batch tracking"}'
```

You should see:

```text
{"message":"Workflow was started"}
HTTP 200
```

This means the workflow started. It is not finished yet. Wait 1 to 2 minutes.

### Check the result in Notion

Open your **FreshFlow Content Review** table. A new row should appear with your request.

| Status | What it means |
| --- | --- |
| Approved | The draft passed all rules. |
| Refused | The request gave no product facts, so FreshFlow wrote nothing. |
| Needs human review | The checker found a claim that the request does not support. |

The request above has no product facts, so the status should be **Refused**. This is the correct result.

## Next steps

When you finish:

1. In the second terminal, run `unset P` to remove the webhook path from memory.
2. In the first terminal, press **Ctrl + C** to stop n8n. Your workflow stays in the `n8n_data` volume.
3. Stop your codespace on `github.com/codespaces` to save your free hours.

To learn more, see the [API quick start](api-quick-start.md) and the [API reference](api-reference.md).

If a step fails, see [Troubleshooting](troubleshooting.md).
