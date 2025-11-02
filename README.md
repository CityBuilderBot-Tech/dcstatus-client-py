
# 🐍 DC Status API Client

This repository hosts the installable Python client for the DC Status API. It's a lightweight, background-service that sends periodic heartbeats from your Python application to your API instance.

> **Note**: The source code for this client and the API is kept in a private repository.

---

## 📦 Installation

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

Run the following command in your terminal. This will read the `requirements.txt` file and install all listed packages at once.

```bash
pip install -r requirements.txt
```

### Method 2: Direct URL Installation

For a quick setup or for testing without a `requirements.txt` file, you can install the package directly from the URL.

```bash
pip install https://github.com/CityBuilderBot-Tech/dcstatus-client-py/releases/download/pip/dcstatus_client-pip.tar.gz
```

---

## 🚀 Quickstart Usage

Import the client and add the `start()` call to your bot's `on_ready` event. The client automatically detects the Bot Name, Bot ID, and Application ID after connecting to Discord.

**1. Configure your `.env` file**

Make sure you have the URL to your API instance set.

```.env
# Your Discord Bot Token
DISCORD_BOT_TOKEN="your_secret_bot_token_here"

# The full URL of your deployed DC Status API
STATUS_API_URL="https://your-api.coolify.app"
```

**2. Update your Bot Code**

Import the client and call the `start()` function once in your bot's `on_ready` event.

```python
import discord
from discord.ext import commands
from dotenv import load_dotenv
import os
import dcstatus_client # 1. Import the client

load_dotenv()

# --- Load Configuration ---
BOT_TOKEN = os.getenv('DISCORD_BOT_TOKEN')
STATUS_API_URL = os.getenv('STATUS_API_URL')

intents = discord.Intents.default()
bot = commands.Bot(command_prefix="!", intents=intents)

@bot.event
async def on_ready():
    print(f'Bot "{bot.user}" is online. 🤖')

    # --- Start the Status Client ---
    if STATUS_API_URL:
        print("Status API URL found. Starting heartbeat client...")
        try:
            dcstatus_client.start(
                api_url=STATUS_API_URL,
                bot_id=str(bot.user.id),              # Fetched automatically
                application_id=str(bot.application_id), # Fetched automatically
                bot_name=bot.user.name                 # Fetched automatically
            )
            print("Heartbeat client is running in the background. ❤️")
        except Exception as e:
            print(f"ERROR starting heartbeat client: {e}")
    else:
        print("STATUS_API_URL not set. Heartbeat client is disabled. 💔")

# --- Run the Bot ---
if __name__ == "__main__":
    bot.run(BOT_TOKEN)
```
