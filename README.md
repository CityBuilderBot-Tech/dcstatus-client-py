
# DC Status API Client

This repository hosts the installable Python client for the DC Status API. It's a lightweight, background-service that sends periodic heartbeats from your Python application to your API instance.

The source code for this client and the API is kept in a private repository.

---

## Installation

You have two options to install the client.

### Method 1: Using `requirements.txt` (Recommended)

This is the standard way to manage dependencies in a Python project. It ensures that anyone who sets up your project gets the correct version of the client.

**1. Add to `requirements.txt`**

Add the following line to your project's `requirements.txt` file:

```
# requirements.txt

# ... your other dependencies like discord.py ...

# Custom DC Status API Client from GitHub Release
https://github.com/CityBuilderBot-Tech/dcstatus-client-py/releases/download/pip/dcstatus_client-pip.tar.gz
```

**2. Install Dependencies**

Run the following command in your terminal. This will install all packages listed in the file.

```bash
pip install -r requirements.txt
```

### Method 2: Direct URL Installation

For a quick setup or for testing without a `requirements.txt` file, you can install the package directly from the URL.

```bash
pip install https://github.com/CityBuilderBot-Tech/dcstatus-client-py/releases/download/pip/dcstatus_client-pip.tar.gz
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
