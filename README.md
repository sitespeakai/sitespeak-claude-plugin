# SiteSpeak Chatbot Manager

Manage your [SiteSpeak](https://sitespeak.ai) chatbots from Claude Code by asking in
plain English. The plugin bundles a skill and the SiteSpeak management MCP server, so
Claude can read your chatbot's real analytics, find what is not working, and fix it.
Nothing changes without your confirmation first.

## Install

```
/plugin marketplace add sitespeakai/sitespeak-claude-plugin
/plugin install sitespeak-chatbot-manager@sitespeak-plugins
```

Then sign in to SiteSpeak once: run `/mcp`, select the `sitespeak` server, and finish
the sign-in in your browser. That is the only setup step. In Claude chat or Cowork,
connect SiteSpeak from the plugin's Connectors tab instead.

## What you can ask

- "Look at my support bot's last 3 months and optimize it for lead capture."
- "Why is my bot failing to answer questions? Find the gaps and fix them."
- "Suggest a few starter questions for new visitors and add the best ones."
- "Find the conversations where people got frustrated and improve the bot."
- "Turn on human handoff and escalate after two unanswered messages."
- "Add our new pricing page to the bot's knowledge and retrain it."
- "Which of my chatbots has the worst answer rate?"
- "Move my bot to a cheaper model without hurting answer quality."

## How it works

One loop, every time: read your data, diagnose against your goal, propose concrete
changes, wait for your yes, apply them, then verify. You stay in control - Claude
confirms before any write and makes the smallest change that does the job.

## Safe by default

- Claude confirms before every write. It never changes your bot on its own.
- Model changes are limited to the models your plan allows, checked on the server.
- Lead capture and escalations may need a plan upgrade to enable; if so, Claude tells
  you instead of failing quietly.

## What is included

| Component | Detail |
| --- | --- |
| Skill | `sitespeak-chatbot-manager` - the plain-English management loop and goal playbooks |
| MCP server | `sitespeak` - HTTP transport to `https://api.sitespeak.ai/mcp`, you sign in with your SiteSpeak account |

## Connect

The plugin is the easiest way to connect. Any MCP client that supports Streamable
HTTP with a bearer token can also connect to the management MCP directly:

```json
{
  "mcpServers": {
    "sitespeak": {
      "type": "http",
      "url": "https://api.sitespeak.ai/mcp",
      "headers": {
        "Authorization": "Bearer YOUR_API_TOKEN"
      }
    }
  }
}
```

Unauthenticated clients can list the available tools; every tool call requires a
valid token. Create one at
[sitespeak.ai/user/api-tokens](https://sitespeak.ai/user/api-tokens).

## Tools

- `list_chatbots` - list the chatbots your account can manage
- `get_chatbot` - read a chatbot's settings, allowed models, and per-model credit cost
- `update_chatbot_settings` - update model, prompts, temperature, lead capture, and escalations
- `list_inbox_threads` - list conversation threads in the inbox
- `get_thread_messages` - read the messages in a conversation
- `update_thread_status` - triage or archive a thread
- `reply_to_thread` - send an approved reply to the visitor in an inbox thread
- `list_sources` - list knowledge sources and their training status
- `add_source` - add a new knowledge source (page or site)
- `recrawl_source` - re-index an existing source to pick up its latest content
- `get_chatbot_analytics` - conversations, answered %, sentiment, leads, top sources and pages
- `get_chatbot_insights` - AI-clustered answered and unanswered question themes
- `list_leads` - read captured leads
- `list_suggested_messages` - list the starter prompts shown to visitors
- `add_suggested_message` - add a suggested starter prompt
- `update_suggested_message` - edit a suggested starter prompt
- `remove_suggested_message` - remove a suggested starter prompt

## Requirements

- Claude Code
- A SiteSpeak account

## Links

- Website: https://sitespeak.ai
- Docs: https://sitespeak.ai/docs
- API tokens: https://sitespeak.ai/user/api-tokens

## License

MIT - see [LICENSE](LICENSE).
