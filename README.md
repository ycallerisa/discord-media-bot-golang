# Discord Media Bot (Go)

This project is a Go-based Discord bot that fetches and posts media from Scrolller and Reddit subreddits using GraphQL queries.  
It provides text-based commands to retrieve random images or videos and is restricted to NSFW Discord channels.

This repository contains the original Go implementation, later rewritten in Python for improved maintainability and modern Discord slash commands.  
Both versions remain available for reference.

Important: This bot retrieves NSFW content and must only be used in Discord channels explicitly marked as NSFW. Always respect Discord’s Terms of Service.

## Features

- Text-based commands such as:
  - .pr0n – fetch random media from discovered NSFW subreddits  
  - .pr0n vid – fetch a high-quality video  
  - .pr0n [subreddit] – fetch media from a specific subreddit  
  - .pr0n help – show available commands  
  - .pr0n listnsfw – share the NSFW subreddit index  
- Integration with Scrolller’s GraphQL API:
  - DiscoverSubreddits query  
  - SubredditQuery for media retrieval  
- Media filtering:
  - Prefer high-resolution 1080-width images  
  - Video selection avoiding unwanted hosts (static, redgifs, etc.)  
- Recursive retry logic when a subreddit contains no valid media  
- Basic error handling for failed API calls  
- Full NSFW channel validation before posting media  

## Tech Stack

- Language: Go  
- Discord Framework: discordgo  
- HTTP Client: net/http  
- JSON Handling: encoding/json  
- OS / signals: program listens for SIGINT, SIGTERM for graceful shutdown

## Getting Started

### Prerequisites

- Go 1.18 or newer  
- A Discord application and bot token  
- A Discord server where you can test the bot

### Installation

1. Clone the repository

    git clone https://github.com/kathiouchka/discord-media-bot-golang.git  
    cd discord-media-bot-golang

2. Build the application

    go build -o discord-media-bot-golang

3. Run the bot

    ./discord-media-bot-golang -t YOUR_DISCORD_BOT_TOKEN

The bot will connect to Discord and begin listening for text commands in channels.

## Usage

Commands must be typed in a channel marked as NSFW.

### .pr0n  
Fetch random NSFW media sourced from Scrolller’s DiscoverSubreddits API.

### .pr0n vid  
Fetch a video (mp4) while filtering out unwanted hosts.

### .pr0n [subreddit]  
Fetch an image or video from the specified subreddit, depending on available media.

### .pr0n help  
Show all available commands.

### .pr0n listnsfw  
Share the NSFW411 index link, but only in NSFW channels.

### .pr0n contact  
Show contact information for the bot author.

If the command is used in a non-NSFW channel, the bot refuses to post media.

## Internals

The Go implementation uses:

- Two GraphQL queries embedded as strings  
- Functions getRandData and getSubData to retrieve media:
  - POST requests with custom headers  
  - JSON unmarshalling into strongly typed structs  
- Filtering logic:
  - Only keep media where width == 1080  
  - Videos must end with .mp4 and avoid static or redgifs hosts  
- Recursive retry when a subreddit responds with zero valid media  
- Random selection from the filtered list  
- A messageCreate handler that:
  - Parses commands  
  - Detects subreddit requests with regex  
  - Validates NSFW status  
  - Sends media accordingly  

The program also registers signal handlers (SIGINT, SIGTERM, os.Interrupt) to properly close the Discord session on shutdown.

## Limitations

- Text-command only (no slash commands)  
- No concurrency optimization  
- No advanced error messaging  
- No rate-limit handling  
- No configuration file  
- Hard-coded GraphQL queries  
- No CI/CD or tests in this version

The Python rewrite addresses several of these points.

## Relationship to the Python Version

A newer Python rewrite of this bot is available:  
[https://github.com/kathiouchka/pr0nbot_python](https://github.com/kathiouchka/discord-media-bot-python)

The Go version remains:
- A reference for the original architecture  
- A demonstration of Go, JSON unmarshalling, and Discord bot design  
- A functional example of a GraphQL consumer written in Go

## Disclaimer

This bot retrieves NSFW content and must only be used in compliance with Discord’s Terms of Service and local regulations.  
The author assumes no responsibility for misuse or violations.
