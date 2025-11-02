
# DC Status API Client

This repository hosts the installable Python client for the DC Status API. The client is designed to be a lightweight, background-service that sends periodic heartbeats from your Python application to your DC Status API instance.

The source code for this client and the API is kept in a private repository.

---

## Installation

The client can be installed directly from this repository's releases using `pip`. No cloning is required.

```bash
# Replace v0.1.0 with the desired version number
pip install https://github.com/CityBuilderBot-Tech/dcstatus-client-py/releases/download/v0.1.0/dcstatus-client-0.1.0.tar.gz
```

## Usage

Using the client is designed to be as simple as possible. Import the package and call the `start()` function once when your application starts.

```python
import dcstatus_client
import time

# --- Configuration ---
# The URL of your deployed DC Status API
API_URL = "https://your-api.coolify.app"

# Your bot's specific identifiers
BOT_ID = "123456789012345678"
APP_ID = "098765432109876543"
BOT_NAME = "MyAwesomeBot"


# --- Application Start ---

print("Starting the main application...")

# Initialize and start the heartbeat client in the background.
# This only needs to be called once.
dcstatus_client.start(
    api_url=API_URL,
    bot_id=BOT_ID,
    application_id=APP_ID,
    bot_name=BOT_NAME
)

print(f"Heartbeat client for '{BOT_NAME}' is running in the background.")

# Your main application logic goes here.
# For example, a loop that keeps your bot alive.
try:
    while True:
        print("Main application is doing work...")
        time.sleep(60)
except KeyboardInterrupt:
    print("\nShutting down main application.")
    # The client will be stopped automatically on exit.

```

### How it Works

- The `start()` function launches a separate, lightweight background thread.
- This thread sends a heartbeat to your API every 15 seconds.
- The client automatically calculates the bot's uptime.
- When your main application exits or crashes, the client's thread is terminated automatically, and no more heartbeats are sent.
