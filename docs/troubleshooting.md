# Troubleshoot FreshFlow on Linux

This page lists the problems you are most likely to meet when you follow the [setup guide](setup-guide.md). For each problem you see the symptom (what you notice), the cause (why it happens) and the fix.

!!! note
    Never paste your API key, your Notion token or your webhook path into a file, a screenshot or a chat message while you fix a problem.

## n8n and the browser

| Symptom | Cause | Fix |
| --- | --- | --- |
| n8n shows old workflows, not the new empty one | You opened an older n8n tab, for example an n8n Cloud account, not the n8n in your codespace. | Close that tab. In the **Ports** tab, find port **5678** and click the globe icon (**Open in Browser**). A new n8n shows **Set up owner account**. |
| **Connection lost** appears at the top right of the n8n editor, and the terminal repeats `Origin header does NOT match the expected origin` | The Codespaces proxy changes the address that the browser reports (the origin), so n8n refuses the live connection. The settings `WEBHOOK_URL` and `N8N_PROXY_HOPS` from the setup guide do not fix this. | Keep working. The editor still saves your changes and n8n still runs. Do the steps that need the live connection, such as publishing, from the terminal. See the next row. |
| The **Publish** button is missing | The editor hides it while the connection is lost. | Open a second terminal and run `docker exec n8n n8n list:workflow`. Each line shows the workflow ID, then a vertical bar, then the name. Copy the ID of your FreshFlow workflow, which is the part before the vertical bar. Then run `docker exec n8n n8n publish:workflow --id="<ID>"`, and replace `<ID>` with that part. |

## Terminal commands

| Symptom | Cause | Fix |
| --- | --- | --- |
| `1s: command not found` | You typed the number 1 instead of the letter l. | Type `ls` with a lowercase letter L. |
| You typed a command and nothing appeared, not even a new prompt | The command is waiting because you did not press **Enter**. | Press **Enter**. Some commands wait on purpose: `read -rs P` shows nothing while it waits for you to paste, and the n8n terminal stays busy while n8n runs. |
| The terminal says `bash: read: ...: not a valid identifier` | You pasted the value on the same line as the `read` command. | Run `read -rs P` alone and press **Enter**. The cursor waits. Then paste once with **Ctrl + Shift + V** and press **Enter**. |
| The length check prints a number other than `36` | The clipboard held other text, not the **Path** field of the **Webhook** node. | Copy the **Path** field again. Run `read -rs P` again, paste once with **Ctrl + Shift + V**, and check the length again. |

## The test request

| Symptom | Cause | Fix |
| --- | --- | --- |
| The curl test returns `HTTP 404` | The workflow is not published, the path is wrong, or you used the `webhook-test` address, which works only while the editor listens. | Publish the workflow (see the **Publish** row above). Check that the address uses `/webhook/` and that `$P` has 36 characters. Then send the request again. |

## Still stuck?

1. Stop n8n with **Ctrl + C** in the first terminal.
2. Start it again with the `docker run` command from [step 5 of the setup guide](setup-guide.md#5-start-n8n). Your workflow stays in the `n8n_data` volume.
3. Open n8n from the **Ports** tab and repeat the step where you were stuck.
