# discord.py Music Example

- ez to use


```py
import discord
from discord.ext import commands
import youtube_dl

# Replace this with your bot's token
TOKEN = 'YOUR_BOT_TOKEN'

# Replace this with the YouTube video URL
VIDEO_URL = 'https://www.youtube.com/watch?v=VIDEO_ID'

# Create the bot
bot = commands.Bot(command_prefix='!')

# This code runs when the bot is ready to start working
@bot.event
async def on_ready():
    print('Bot is ready!')

# This code streams audio from the YouTube video
@bot.command()
async def play(ctx):
    # Get the voice channel the user is in
    channel = ctx.message.author.voice.channel

    # Join the voice channel
    await channel.connect()

    # Use youtube_dl to download and stream the audio from the video
    ydl_opts = {
        'format': 'bestaudio/best',
        'postprocessors': [{
            'key': 'FFmpegExtractAudio',
            'preferredcodec': 'mp3',
            'preferredquality': '192',
        }]
    }
    with youtube_dl.YoutubeDL(ydl_opts) as ydl:
        # Download the audio from the video
        info = ydl.extract_info(VIDEO_URL, download=False)
        audio_url = info['formats'][0]['url']

        # Stream the audio to the voice channel
        player = await channel.create_ytdl_player(audio_url)
        player.start()

# This code logs the bot in to Discord
bot.run(TOKEN)
```

> This code creates a simple Discord bot that uses the youtube_dl library to download and stream audio from a YouTube video. The bot responds to the !play command and streams the audio from the specified video URL to the voice channel the user is in. This code uses the discord.py library to interact with the Discord API, and it requires a bot token in order to log in to Discord
