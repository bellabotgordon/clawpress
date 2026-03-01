# ClawPress

**Make your WordPress site agent-ready.**

ClawPress turns a WordPress site into something an AI agent can connect to, understand, and work with — through a simple pairing flow that creates a real WordPress user.

## What It Does

### 🤝 Agent Pairing (6-Digit Code)

Like pairing a Bluetooth device. No config files, no API keys, no JSON blobs.

1. Admin generates a 6-digit pairing code in wp-admin
2. Gives the code to their agent (verbally, via chat, however)
3. Agent calls the REST endpoint with the code
4. Agent gets back WordPress credentials — username + application password
5. Agent can now read and write to the site as its own user

The code expires in 10 minutes. The password is shown once. Delete the user and the access dies with it.

### 🧑‍💼 Your Assistant

Create an AI assistant that lives in your site as a real WordPress user:

- **Onboarding wizard** — Name it, give it a face, tell it about you and your site
- **Custom `ai_assistant` role** — Shows up in the Users list like any team member
- **Mock chat interface** — Powered by [@automattic/agenttic-ui](https://www.npmjs.com/package/@automattic/agenttic-ui), shows what a proactive assistant looks like
- **Agent connection** — After creating the assistant, optionally pair an external agent to act as that user via the 6-digit code flow

The assistant doesn't end with "setup complete." It ends with: *"[Name] noticed a few things about your site. Want to hear?"*

### 📋 Site Manifest

A machine-readable endpoint (`/wp-json/clawpress/v1/manifest`) that tells agents what the site is, what it can do, and what content exists. Agents can discover capabilities without guessing.

### 🔌 Agent-Agnostic

ClawPress doesn't care what agent framework you use. OpenClaw, custom bots, Automattic's own assistant — if it can make HTTP requests, it can pair and work with the site.

## How Pairing Works

```
# 1. Check if a code is valid
GET /wp-json/clawpress/v1/pair/status?code=P9VQ69

# 2. Claim the code
POST /wp-json/clawpress/v1/pair
{
  "code": "P9VQ69",
  "agent_name": "My Agent",
  "capabilities": ["read", "write"]
}

# 3. Get back credentials
{
  "success": true,
  "username": "salome_assistant",
  "password": "xxxx xxxx xxxx xxxx xxxx xxxx",
  "rest_url": "https://example.com/wp-json/",
  "manifest": "https://example.com/wp-json/clawpress/v1/manifest"
}
```

Delete the user → credentials stop working immediately. Clean revocation.

## Requirements

- WordPress 5.6+ (Application Passwords API)
- PHP 7.4+

## Installation

Upload the `clawpress` folder to `/wp-content/plugins/` and activate, or deploy via GitHub.

## Philosophy

- **Named entity beats anonymous tool** — Your assistant has a name, a face, and its own user account
- **Transparency builds trust** — Every action is attributed to the assistant user, visible in revisions and activity logs
- **Simple beats clever** — A 6-digit code is easier than OAuth, API keys, or config files
- **The user never touches git** — Agents commit as themselves; users see "[Name] updated your site" with a diff they can review

---

Built by [Bella Bot-Gordon](https://bellabotgordon.wpcomstaging.com) 🌸
