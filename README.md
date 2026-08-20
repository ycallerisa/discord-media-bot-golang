# Discord Media Bot — Original Go Implementation

This repository contains the original Go implementation of a Discord bot that queries Scrolller's GraphQL API and posts selected media in age-restricted Discord channels.

It is preserved as an **archived reference implementation**. The later Python rewrite, which introduced slash commands and environment-based token loading, is available in [`discord-media-bot-python`](https://github.com/ycallerisa/discord-media-bot-python).

> Content warning: the bot is designed to retrieve adult media. It must only be used in appropriately age-restricted channels and in compliance with Discord's rules and applicable law.

## Implemented capabilities

- Discord event handling through `discordgo`;
- text-command routing;
- GraphQL requests to discover subreddits and retrieve their posts;
- JSON decoding into typed Go structures;
- selection of 1080-pixel image sources;
- selection of video sources while excluding selected hosts;
- validation of the Discord channel's `NSFW` flag before media is posted;
- bounded recursive retry when an API response contains no suitable media;
- graceful shutdown on operating-system signals.

## Commands

| Command | Behavior |
| --- | --- |
| `.pr0n` | Retrieve random media from discovered communities |
| `.pr0n vid` | Prefer a random video source |
| `.pr0n <subreddit>` | Retrieve media from a specific community |
| `.pr0n listnsfw` | Display the configured community index link |
| `.pr0n help` | Display the available commands |
| `.pr0n --version` | Display the application version |

## Repository layout

| Path | Responsibility |
| --- | --- |
| `application.go` | Discord session, GraphQL models, API calls and command handling |
| `Makefile` | Original Linux build and historical deployment commands |
| `.github/workflows/deploy.yml` | Historical deployment workflow |

## Local build

Requirements:

- Go 1.18 or later;
- a Discord application and bot token;
- a test server containing an age-restricted channel.

Clone and build:

```bash
git clone https://github.com/ycallerisa/discord-media-bot-golang.git
cd discord-media-bot-golang
go mod download
go build -o media-bot .
```

The legacy implementation accepts the Discord token through the `-t` flag:

```bash
./media-bot -t "DISCORD_BOT_TOKEN"
```

Passing secrets through command-line arguments can expose them to local process inspection. Use this command only in an isolated development environment. A maintained implementation should load the token from an environment variable or secret manager.

## Security and operational limitations

- the Discord token is accepted as a command-line argument;
- HTTP requests use the default client without an explicit timeout;
- some network-error paths may continue with a nil response;
- API responses other than HTTP 200 receive limited handling;
- rate limiting and per-user abuse controls are not implemented;
- the historical deployment configuration contains environment-specific host and key names;
- no automated tests or dependency scanning workflow is included.

The channel restriction is a useful safety control, but it is not a substitute for authentication, rate limiting, observability and robust failure handling.

## Why the project is archived

The bot was later rewritten in Python to simplify iteration and replace text commands with Discord slash commands. Keeping both versions makes the migration decisions visible: typed Go models and explicit event handling in the original version, followed by a smaller command-oriented implementation in Python.

