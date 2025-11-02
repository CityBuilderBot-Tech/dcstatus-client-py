
# DC Status API Client

This repository hosts the installable Python client for the DC Status API. It's a lightweight, background-service that sends periodic heartbeats from your Python application to your API instance.

The source code for this client and the API is kept in a private repository.

---

## Installation

The recommended way to install the client is by adding it to your project's `requirements.txt` file.

### 1. Create `requirements.txt`

Create a file named `requirements.txt` in your bot's main directory and add the following content. This file lists all Python packages your project depends on.

```
# requirements.txt

# Core Discord library for bot functionality
discord.py

# Custom DC Status API Client from this GitHub Release
https://github.com/CityBuilderBot-Tech/dcstatus-client-py/releases/download/pip/dcstatus_client-pip.tar.gz
```

### 2. Install Dependencies

Run the following command in your terminal. This will read the `requirements.txt` file and install all listed packages at once.

```bash
pip install -r requirements.txt
```

---

## Quickstart Usage

Import the client and add the `start()` call to your bot's `on_ready` event. The client will automatically use your bot's user ID and application ID.

```python
import discord
from discord.ext import commands
import dcstatus_client # 1. Import the client

intents = discord.Intents.default()
bot = commands.Bot(command_prefix="!", intents=intents)

@bot.event
async def on_ready():
    print(f'Bot "{bot.user}" is online.')

    # 2. Start the heartbeat client
    try:
        dcstatus_client.start(
            # --- CONFIGURATION ---
            api_url="https://your-api.coolify.app",  # <-- Enter your API URL here
            bot_name="My Awesome Bot",             # <-- Enter the name for your bot here
            # ---------------------
            bot_id=str(bot.user.id),
            application_id=str(bot.application_id)
        )
        print("Heartbeat client is running in the background.")
    except Exception as e:
        print(f"ERROR starting heartbeat client: {e}")

# Replace with your actual bot token
bot.run("YOUR_DISCORD_BOT_TOKEN")
```
