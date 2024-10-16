# Internet Speed Test Script

This repository contains a Bash script that performs an internet speed test using `speedtest-cli` and sends the results to a specified Telegram channel.

## Table of Contents
- [Installation](#installation)
- [Configuration](#configuration)
- [Usage](#usage)

## Installation

### Step 1: Install Speedtest CLI

To install `speedtest-cli`, you need to have Python and pip installed on your system. Follow these steps:

1. **Install Python and pip** (if not already installed):
   ```bash
   sudo apt update
   sudo apt install python3 python3-pip -y
   ```
2. **Install speedtest-cli**
   ```bash
   pip3 install speedtest-cli
   ```

Step 2: Create the Script
1. Create the Script Directory:
   ```bash
   mkdir -p ~/scripts
   ```
2. Create the Speedtest Script: Open a text editor (e.g., nano or vim) to create the script file:
   ```bash
   nano ~/scripts/speedtest_telegram.sh
   ```
3. Copy the Script:
   ```bash
   Copy and paste the code from speedtest_telegram.sh
   ```
5. Save and Exit: If using nano, press CTRL + O to save, then CTRL + X to exit.

### Step 3: Make the code executable. 
   ```bash
   chmod +x ~/scripts/speedtest_telegram.sh
   ```
## Configuration
To set up your Telegram bot and obtain the necessary credentials, follow these steps:
1. Create a Telegram Bot
- **BotFather**: Start by creating a Telegram bot and obtaining the token. You can do this by chatting with [BotFather](https://core.telegram.org/bots#botfather).
2. Obtain Your Chat ID
- **Chat ID**: To send messages to a specific group or individual, you'll need the chat ID. Here are a couple of methods to find it:
  - Send a message to your bot and check the API response.
  - Use tools like [GetIDBot](https://getid.bot/) to easily retrieve your chat ID.

## Usage
Run the script manually with the following command:
   ```bash
   ~/scripts/speedtest_telegram.sh
   ```
You can also set up a cron job to run the script automatically at specified intervals. For example, to run it every hour, add the following line to your crontab:
   ```bash
   0 * * * * ~/scripts/speedtest_telegram.sh
   ```







