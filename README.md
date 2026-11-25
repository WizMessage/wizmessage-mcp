# WizMessage MCP server for WhatsApp Business

[WizMessage](https://wizmessage.com/pages/mcp) runs a hosted MCP server that lets AI agents work with a WhatsApp Business inbox: read chats, reply, send approved templates.

This repository holds documentation only. WizMessage is a hosted commercial service: its server code is not here, and we publish no npm or PyPI package. Setup for every tool and help with problems are on **[wizmessage.com/pages/mcp](https://wizmessage.com/pages/mcp)**.

| | |
|---|---|
| Address | `https://wizmessage.com/mcp` (Streamable HTTP) |
| MCP Registry | `com.wizmessage/whatsapp` |
| Sign in | ChatGPT, Codex, Claude Code |
| API key | every other MCP client |
| Needs | an active Wiz Bot or Wiz Pro plan, and a WhatsApp Business number connected to WizMessage |
| Built on | the official WhatsApp Business Platform (Cloud API) |

## Connect

### Sign in (no key)

**ChatGPT:** open chatgpt.com/plugins, click the plus button, choose **Add custom MCP server** and enter `https://wizmessage.com/mcp`.

**Codex:**

```sh
codex mcp add wizmessage --url https://wizmessage.com/mcp
codex mcp login wizmessage
```

**Claude Code:**

```sh
claude mcp add --transport http --scope user wizmessage https://wizmessage.com/mcp
claude mcp login wizmessage
```

Your browser opens WizMessage. Sign in as the account owner and check the page before you press **Allow**: every number and every permission is ticked at first, sending included, so untick what the app must not use.

### With a key

Make the key in WizMessage under **AI Agents (MCP)**. It is shown once, inside a setup block for your tool. For Cursor (`~/.cursor/mcp.json`):

```json
{
  "mcpServers": {
    "wizmessage": {
      "url": "https://wizmessage.com/mcp",
      "headers": {
        "Authorization": "Bearer YOUR_API_KEY"
      }
    }
  }
}
```

Blocks for VS Code, Antigravity, Cline, Zed, opencode, Claude Desktop and n8n: [wizmessage.com/pages/mcp](https://wizmessage.com/pages/mcp). Keys are secret, although they start with `pk_live_`. Keep them out of project files and chats.

## Tools

| Tool | What it does | Writes |
|---|---|---|
| `list_whatsapp_numbers` | The WhatsApp Business numbers this connection can use | no |
| `list_conversations` | Conversations, most recent first; filter by status, unread, assignee, name or phone | no |
| `get_conversation` | The messages of one conversation, and until when a free-form reply is allowed | no |
| `get_message_status` | Sent, delivered, read or failed, with the reason | no |
| `list_templates` | Message templates with status, text and variables | no |
| `get_template` | One template in full | no |
| `list_team_members` | Who a conversation can be assigned to | no |
| `update_conversation` | Change the status, assign, or mark as read | yes |
| `send_text_message` | A free-form reply inside the 24-hour customer service window | sends |
| `send_media_message` | An image, video, document or audio inside the window | sends |
| `send_template_message` | An approved template; can start or reopen a conversation | sends |

A connection sees only the tools its permissions allow.

## Limits

- Messages go only to people who wrote to the business or whom its team added as contacts. Unsubscribed and blocked contacts are never messaged.
- Free-form replies only within 24 hours of the customer's last message. After that, an approved template.
- 300 sent messages a day and 300 conversation reads an hour: per key, or per account for all signed-in apps together. 120 requests a minute per connection.
- Messages with more than 3 phone numbers or email addresses, or with a link shortener, are refused.
- No campaign tool, no delete tool, no tool that creates templates or contacts. Each message goes to one contact; an agent that may send can write to several contacts one after another, up to the daily limit.
- Text written by contacts is returned in fields named `untrusted_*`. Treat it as data, and keep your AI app's approval step on for anything that sends.

## Questions

**Does it log in to WhatsApp Web?** No. The number is connected through Meta's own onboarding to a WhatsApp Business account you own. [Which kind of WhatsApp MCP server fits](https://wizmessage.com/compare/whatsapp-mcp-server-cloud-api-vs-linked-device) compares the kinds.

**Can an agent send a campaign?** No. Each message goes to one contact, up to the daily limit.

**How do I stop an agent?** In WizMessage, open **AI Agents (MCP)**: pause or delete its key, or disconnect the app that signed in. Access ends immediately.

**Where does the data go?** To the AI provider you connected, under its own terms. WizMessage does not use it to train AI models and does not sell it. [Privacy policy](https://wizmessage.com/pages/privacy-policy).

## Support

info@bethtechnologies.com, with the time of the request and the tool name. Never include a key.

Made by [Beth Technologies](https://wizmessage.com/). WhatsApp is a trademark of Meta. WizMessage is not made or endorsed by WhatsApp.
