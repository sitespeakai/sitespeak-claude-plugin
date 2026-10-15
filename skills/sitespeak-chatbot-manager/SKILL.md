---
name: sitespeak-chatbot-manager
description: Use when the user wants to inspect, optimize, configure, or triage a SiteSpeak chatbot - "look at my chatbot's analytics and optimize it for X" (more leads, better answers, smoother onboarding, human handoff, happier customers, fewer support tickets, more signups), change the model or prompts, manage knowledge sources, suggested messages, lead capture or escalations, read captured leads, or work the inbox. Connects to the SiteSpeak management MCP.
---

# SiteSpeak Chatbot Manager

Improve a SiteSpeak chatbot by asking in plain English. This skill connects your AI
assistant to the SiteSpeak management MCP, so it can read your chatbot's real
analytics, find what is not working, and fix it. Nothing changes without your
confirmation first.

## What you can ask

- "Look at my support bot's last 3 months and optimize it for lead capture."
- "Why is my bot failing to answer questions? Find the gaps and fix them."
- "Suggest a few starter questions for new visitors and add the best ones."
- "Find the conversations where people got frustrated and improve the bot."
- "Turn on human handoff and escalate after two unanswered messages."
- "Add our new pricing page to the bot's knowledge and retrain it."
- "Which of my chatbots has the worst answer rate?"
- "Move my bot to a cheaper model without hurting answer quality."

## Setup (once)

```
/plugin marketplace add sitespeakai/sitespeak-claude-plugin
/plugin install sitespeak-chatbot-manager@sitespeak-plugins
```

Then sign in to SiteSpeak once: run `/mcp`, select the `sitespeak` server, and finish
the sign-in in your browser. In Claude chat or Cowork, connect SiteSpeak from the
plugin's Connectors tab instead.

## How it works

One loop, every time: read your data, diagnose against your goal, propose concrete
changes, wait for your yes, apply them, then verify. You stay in control - the
assistant confirms before any write and makes the smallest change that does the job.

## Tools

**Read:** `list_chatbots`, `get_chatbot` (settings, allowed models, credit cost per
model), `get_chatbot_analytics` (conversations, feedback, answered %, negative
sentiment %, leads, top sources/pages/countries), `get_chatbot_insights`
(AI-clustered answered and **unanswered** question themes), `list_inbox_threads`,
`get_thread_messages`, `list_sources`, `list_suggested_messages`, `list_leads`.

**Write:** `update_chatbot_settings` (model, instructions prompt, welcome message,
default answer, temperature, lead capture, escalations), `update_thread_status`,
`reply_to_thread` (only text the user approved), `add_source`, `recrawl_source`,
`add_suggested_message`, `update_suggested_message`, `remove_suggested_message`.

## The loop (every time)

1. **Find the bot** - `list_chatbots`. If there is more than one, confirm which.
2. **Read the period asked for** - `get_chatbot` for the current setup,
   `get_chatbot_analytics` (for "the last few months" use `period: "custom"` with
   `from` and `to`), `get_chatbot_insights`, plus the goal reads below.
3. **Diagnose** against the goal.
4. **Propose** the changes with a clear before and after.
5. **Confirm**, then apply.
6. **Verify** by reading back, and say which number to watch next period.

## Goal playbooks

Each goal: what it improves, what to measure, and the levers to pull.

- **More leads** (turn chats into contacts) - Measure: `leads` in
  `get_chatbot_analytics` and `list_leads`. Levers: `enable_lead_capture` and
  `lead_capture_fields`, an `instructions_prompt` that asks for an email at the right
  moment, the `welcome_message`, and a suggested message that invites contact.
- **Better answers** (fewer "I don't know") - Measure: `answeredQuestionsPercentage`
  and the **unanswered** themes from `get_chatbot_insights` (your real knowledge
  gaps). Levers: `add_source` and `recrawl_source` to fill the gaps,
  `instructions_prompt`, `default_answer`.
- **Smoother onboarding** - Measure: `messagesPerConversation` and where new visitors
  drop off. Levers: `welcome_message`, suggested starter questions,
  `instructions_prompt`.
- **Human handoff** - Measure: escalation settings in `get_chatbot` and handoff
  threads in the inbox. Levers: `enable_escalations`, `escalate_message_threshold`,
  `escalations_email`, `enable_escalations_notification`, and `update_thread_status`
  to triage.
- **Happier customers** - Measure: `negativeSentimentPercentage`, `negativeFeedback`,
  and the unhappy threads (`list_inbox_threads` feedback filter, then
  `get_thread_messages`). Levers: `instructions_prompt` tone, sources,
  `default_answer`.
- **Fewer support tickets** (deflection) - Measure: answer rate up, escalations down.
  Levers: sources, `instructions_prompt`, suggested messages.
- **More signups or sales** - Measure: `uniqueLinkClicks` and top pages. Levers:
  suggested messages of type `redirect` that point to your signup or pricing page,
  `instructions_prompt` nudges, the `welcome_message`.
- **Lower cost** - Measure: conversation volume and the credit cost per model from
  `get_chatbot`. Levers: a lower-credit `gpt_model` (only from `allowed_models`).
- **Fresh knowledge** - Measure: `trained_at`, `sync_frequency`, and `status` in
  `list_sources`. Levers: `recrawl_source` for stale pages, `add_source` for new ones.

## Safe by default

- The assistant confirms before every write. It never changes your bot on its own.
- Model changes are limited to the models your plan allows, checked on the server.
- Lead capture and escalations may need a plan upgrade to enable; if so, the assistant
  tells you instead of failing quietly.
- "Mark a thread as done" archives it.

## Good to know

- Answer rate, sentiment, and insight themes come from scheduled jobs, so they lag and
  are empty for brand-new or low-traffic bots. Empty does not mean "no gaps".
- A new source starts as `training` and indexes in the background. Check `list_sources`
  for `trained` before you rely on it in answers.
- Your bot can run on models from OpenAI, Anthropic, Google, or xAI, so "pick a better
  model" is a real lever.

## Worked example: "optimize my bot for lead capture over the last quarter"

1. `list_chatbots`, pick the bot.
2. Read the quarter: `get_chatbot_analytics` with `period: "custom"` and the date
   range for the `leads` baseline, `list_leads` for who was captured, `get_chatbot`
   for the current lead-capture setup, `get_chatbot_insights` for what visitors ask.
3. Diagnose, for example: plenty of conversations, few leads, the form is off, and the
   prompt never asks for contact details.
4. Propose: turn on lead capture with name and email, rewrite the instructions prompt
   to ask for an email once the visitor shows intent, and add a "Talk to sales"
   suggested message.
5. Confirm, then apply with `update_chatbot_settings` and `add_suggested_message`.
6. Read back `get_chatbot`, and tell the user to compare `leads` next month.
