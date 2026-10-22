# Say It Again MCP server

Say It Again makes AI UGC videos for online stores from their product pages. A generated
presenter holds, uses or wears the product and speaks a script written from the page. This
remote MCP server lets an AI assistant do the same with your Say It Again account: read a
product, write three scripts, quote a video's price in credits, make the video and follow it
until it is ready.

This repository holds no server code. The server is hosted by Say It Again, and this README
says what it does and how to connect to it. Full reference: https://sayitagain.ai/docs/api

## Server

- Address: `https://app.sayitagain.ai/mcp`
- Transport: Streamable HTTP
- Sign-in: OAuth (sign in to Say It Again as your team's owner and press Allow), or an API
  key sent as `Authorization: Bearer <key>`
- Official MCP Registry name: `ai.sayitagain/sayitagain`

## Before you start

The API and the MCP server come with a plan: Lite, Starter, Growth or Scale. They are not in
the free trial. Plans and prices: https://sayitagain.ai/pricing

To use a key: sign in to Say It Again as the owner of your team, open Settings, then API,
and press Create key. The key is shown once. A key acts for your whole team and spends its
credits, so keep it secret, and revoke it there at any time.

## Connect

### Claude Code

```sh
claude mcp add --transport http sayitagain https://app.sayitagain.ai/mcp \
  --header "Authorization: Bearer $SIA_KEY"
```

Or leave out the header and sign in when Claude Code asks.

### VS Code (GitHub Copilot)

In `.vscode/mcp.json`:

```json
{
  "inputs": [
    {
      "type": "promptString",
      "id": "sia-key",
      "description": "Say It Again API key",
      "password": true
    }
  ],
  "servers": {
    "sayitagain": {
      "type": "http",
      "url": "https://app.sayitagain.ai/mcp",
      "headers": { "Authorization": "Bearer ${input:sia-key}" }
    }
  }
}
```

### Other clients

Add a remote MCP server with the address `https://app.sayitagain.ai/mcp`. Where the client
takes an API key or a bearer token, give it your key.

## Tools

Every tool is free except `make_video`, `change_video`, `make_longer` and `another_language`, which spend the team's credits.

| Tool | What it does |
| --- | --- |
| `get_balance` | The team's credits, its plan, and how many script writes it has left. |
| `read_product` | Reads a product's page from its link: name, price, description, benefits, photos, and an id for the other tools. |
| `write_scripts` | Writes three scripts for a video of a product, from what `read_product` read. |
| `get_scripts` | The scripts `write_scripts` wrote, once they are ready. |
| `quote_video` | What a video with these choices costs in credits, and whether the balance covers it. |
| `make_video` | Makes a UGC video of a product. Spends the price `quote_video` gave, refunded if the video fails. |
| `get_video` | How a video is going, and its download link once it is ready, with the steps it offers next. |
| `list_videos` | The team's videos, newest first. |
| `list_presenters` | The presenters a video can be made with. |
| `list_ads` | The Meta Ad Library ads the team's store already holds, to build a video like one of them. |
| `describe_presenter` | Draws a presenter of the team's own from a description of how they look. |
| `get_described` | How a drawing is going, with its pictures once it is ready. |
| `keep_presenter` | Keeps one of those pictures as one of the team's presenters. |
| `set_presenter_voice` | Sets the voice a described presenter speaks in. |
| `change_video` | Changes a finished video as you ask, into a new video beside it. Spends credits, refunded if it fails. |
| `make_longer` | Makes a finished video longer, with new lines from the product page. Spends credits, refunded if it fails. |
| `another_language` | Makes a finished video again in another language. Spends credits, refunded if it fails. |
| `other_sizes` | Lays a finished video out in another size. Free. |

Each tool that spends takes the price in credits that `quote_video` or the video's next
steps gave, and an idempotency key, every time: a price that is no longer right is refused
and nothing is spent, and a retry with the same key spends nothing more.

## Use

Ask your assistant, for example:

- "Read https://naseemhome.com/products/the-everyday-mug and write three scripts for a UGC video."
- "What would a 15-second video of it cost? Make it with the second script."
- "Is my video ready? Give me the download link."

## Limits

Requests per minute and presses per day are listed on https://sayitagain.ai/docs/api, as
are the errors each tool can answer. Videos made by Say It Again carry Content Credentials.

## Contact

support@sayitagain.ai. Say It Again is made by Apolonis Social Media Management Services -
FZCO, Dubai, United Arab Emirates. https://sayitagain.ai
