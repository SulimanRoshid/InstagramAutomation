# Instagram DM AI Agent with n8n, DeepSeek, and Notion

This project contains an n8n workflow that turns your Instagram Direct Messages into an AI-powered chat experience. Incoming DMs are received via a Meta webhook, processed by a DeepSeek-powered AI agent, sent back to the user on Instagram, and optionally logged to a Notion database for later review.[web:46][web:54]

## Features

- Receive Instagram DMs via Meta Webhooks and the Instagram Messaging API.[web:54]
- Generate AI responses using a DeepSeek chat model through n8n’s AI Agent node.[web:46][web:51]
- Maintain short-term conversation context per user with a memory buffer.
- Send responses back to the original Instagram sender using the Graph API. [web:54]
- Log both the received message and the AI response into a Notion database for tracking and analysis.[web:46][web:48]

## Architecture

The workflow is implemented entirely in n8n as a JSON export. At a high level:

1. **Webhook (POST)** – Receives Instagram DM events from Meta and forwards valid messages for processing.[web:54]
2. **If node** – Ensures the event contains a message text and that the sender is not your own account.
3. **AI Agent + DeepSeek** – Builds a prompt from the DM text and calls DeepSeek to generate a reply.[web:46][web:51]
4. **Code node** – Cleans up the AI output (removing newlines, tabs, and extra whitespace).
5. **HTTP Request** – Calls the Graph API `/messages` endpoint to send the reply back to the original Instagram user.[web:54]
6. **Notion node** – Creates a database page storing the original message and the AI-generated response in Notion.[web:46][web:48]

There is also a **Respond to Webhook** node that can be used to echo the `hub.challenge` query parameter during Meta webhook verification.

## Prerequisites

To run this workflow you’ll need:

- An n8n instance (self-hosted or cloud) with access to the internet.[web:58]
- A Meta app configured for Instagram Messaging, with a webhook set up and verified.
- An Instagram Business or Creator account connected to the Meta app.[web:54]
- A valid Graph API access token with permissions to read and send Instagram messages.[web:54]
- A DeepSeek API key and credentials configured in n8n.
- A Notion integration and database for storing message logs.[web:46][web:48]

   ```bash
   git clone https://github.com/<your-username>/<your-repo>.git
