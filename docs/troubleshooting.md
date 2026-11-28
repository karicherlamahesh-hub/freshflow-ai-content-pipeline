# Troubleshoot FreshFlow on Linux

This page lists the problems you are most likely to meet when you follow the [setup guide](setup-guide.md). For each problem you see what you notice, why it happens and how to fix it.

!!! note
    Never paste your API key, your Notion token or your webhook path into a file, a screenshot or a chat message while you fix a problem.

## n8n and the browser

| Problem | Cause | Fix |
| --- | --- | --- |
| n8n shows old workflows, not the new empty one | You opened an older n8n tab, for example an n8n Cloud account, not the n8n in your codespace. | Close that tab. In the **Ports** tab, find port **5678** and click the globe icon (**Open in Browser**). A new n8n shows **Set up owner account**. |
| **Connection lost** appears at the top right of the n8n editor, and the terminal repeats `Origin header does NOT match the expected origin` | The Codespaces proxy changes the browser origin to `localhost`, so n8n refuses the live connection. The `WEBHOOK_URL` and `N8N_PROXY_HOPS` settings do not fix it. | Keep working. The editor still saves your changes. Do the steps that need the live connection, such as publishing, from the terminal. See the next row. |
| The **Publish** button is missing | The editor hides it while the connection is lost. | Open a second terminal and run `docker exec n8n n8n list:workflow` to get the workflow ID. Then run `docker exec n8n n8n publish:workflow --id="<ID>"`. Replace `<ID>` with the ID. |

## Terminal commands

| Problem | Cause | Fix |
| --- | --- | --- |
| `1s: command not found` | You typed the number 1 instead of the letter l. | Type `ls` with a lowercase letter L. |
| You typed a command and nothing happens | The command is still waiting because you did not press **Enter**. | Press **Enter**. |
| `read: ... not a valid identifier` | You pasted the value on the same line as the `read` command. | Run `read -rs P` alone and press **Enter**. The cursor waits. Then paste once and press **Enter**. |
| The character count of the webhook path is not `36` | The clipboard held other text, not the **Path** field of the **Webhook** node. | Copy the **Path** field again, then run `read -rs P` and paste once. Check the count again. |

## The test request

| Problem | Cause | Fix |
| --- | --- | --- |
| The curl test returns `HTTP 404` | The workflow is not published, the path is wrong, or you used the `webhook-test` address, which works only while the editor listens. | Publish the workflow (see the **Publish** row above). Check that the address uses `/webhook/` and that `$P` has 36 characters. Then send the request again. |

## Still stuck?

1. Stop n8n with **Ctrl + C** in the first terminal.
2. Start it again with the `docker run` command from [step 5 of the setup guide](setup-guide.md#5-start-n8n). Your workflow stays in the `n8n_data` volume.
3. Open n8n from the **Ports** tab and try the step again.
