# LupusAria

LupusAria is a standalone local Twitch bot. It owns Twitch credentials, bot settings, chat behavior, raffle state, Media Actions and their OBS overlay, and Stream Wrap-Up.

## LivePanel integration

The SIP v1 integration branch adds `LupusAria.exe --service`. The process exposes loopback SIP at `127.0.0.1:47050` and starts with the Twitch bot idle. The desktop app and headless service use the same user configuration and state. One instance owns the bot at a time.

LivePanel starts the bot on a committed Go Live action and stops it on End Stream. The SIP service remains reachable after the bot stops. Users can save feature switches through LivePanel and see the effective state while running. Bot lifecycle errors, feature activation errors, and recent operational logs are available in LivePanel. LupusAria exposes safe summaries and keeps credentials, chat transcripts, and AI content local.

LupusAria uses its own persistent feature preferences. It exposes no profile selector in LivePanel.

See the [LupusAria repository](https://github.com/LastUrsa/LupusAria) for setup, endpoint testing, and application-specific details.
