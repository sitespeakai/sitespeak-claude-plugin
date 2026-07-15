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

Claude Code prompts for your SiteSpeak API token on install and stores it securely
(macOS Keychain, never in a settings file). Create a token at
[sitespeak.ai/user/api-tokens](https://sitespeak.ai/user/api-tokens). That is the only
setup step - the bundled MCP server connects on its own.

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
| MCP server | `sitespeak` - HTTP transport to `https://api.sitespeak.ai/mcp`, authenticated with your token |

## Requirements

- Claude Code
- A SiteSpeak account with an API token

## Links

- Website: https://sitespeak.ai
- Docs: https://sitespeak.ai/docs
- API tokens: https://sitespeak.ai/user/api-tokens

## License

MIT - see [LICENSE](LICENSE).
