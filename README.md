# Internet Speed Test Script

This repository contains a Bash script that performs an internet speed test using `speedtest-cli` and sends the results to a specified Telegram channel.

## Table of Contents
- [Installation](#installation)
- [Configuration](#configuration)
- [Usage](#usage)
- [License](#license)

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
3. Copy the Script: Copy and paste the following code into the text editor:
   ```bash
   #!/bin/bash

# -------------------------------
# Internet Speed Test Script
# -------------------------------
# This script performs an internet speed test every hour and sends
# the results to a specified Telegram channel using MarkdownV2 formatting.
# -------------------------------

# -------------------------------
# Configuration
# -------------------------------

# Telegram Bot Token and Chat ID
# IMPORTANT: Replace these with your actual Bot Token and Chat ID.
BOT_TOKEN="YOUR_BOT_TOKEN"
CHAT_ID="YOUR_CHAT_ID"

# Path to speedtest-cli
SPEEDTEST_CMD="/usr/bin/speedtest-cli"

# Log File Path
LOG_FILE="$HOME/scripts/speedtest.log"

# -------------------------------
# Perform the Speed Test
# -------------------------------

# Check if speedtest-cli exists and is executable
if [ ! -x "$SPEEDTEST_CMD" ]; then
    echo "$(date +"%Y-%m-%d %H:%M:%S") - ERROR: speedtest-cli not found or not executable at $SPEEDTEST_CMD" >> "$LOG_FILE"
    exit 1
fi

# Perform the speed test and capture the output
SPEEDTEST_OUTPUT=$("$SPEEDTEST_CMD" --simple 2>&1)
SPEEDTEST_EXIT_CODE=$?

# Log the raw speedtest output
echo "$(date +"%Y-%m-%d %H:%M:%S") - Speedtest Output: $SPEEDTEST_OUTPUT" >> "$LOG_FILE"

if [ $SPEEDTEST_EXIT_CODE -ne 0 ]; then
    # Speed Test Failed
    CURRENT_TIME=$(date +"%H:%M:%S")  # Get only the time
    MESSAGE="*Internet Speed Test Failed*\nError: $SPEEDTEST_OUTPUT\nTime: $CURRENT_TIME"
else
    # Speed Test Succeeded
    # Extract the required metrics
    PING=$(echo "$SPEEDTEST_OUTPUT" | grep 'Ping' | awk '{print $2 " " $3}')
    DOWNLOAD=$(echo "$SPEEDTEST_OUTPUT" | grep 'Download' | awk '{print $2 " " $3}')
    UPLOAD=$(echo "$SPEEDTEST_OUTPUT" | grep 'Upload' | awk '{print $2 " " $3}')
    
    # Log the extracted metrics
    echo "$(date +"%Y-%m-%d %H:%M:%S") - Extracted Metrics - Ping: $PING | Download: $DOWNLOAD | Upload: $UPLOAD" >> "$LOG_FILE"

    # Escape reserved characters for MarkdownV2
    escape_md() {
        local input="$1"
        # Escape the special characters for MarkdownV2
        echo "$input" | sed -e 's/_/\\_/g' -e 's/\*/\\*/g' -e 's/\[/\\[/g' -e 's/\]/\\]/g' -e 's/~/\\~/g' -e 's/`/\\`/g' -e 's/-/\\-/g' -e 's/\./\\./g'
    }

    PING=$(escape_md "$PING")
    DOWNLOAD=$(escape_md "$DOWNLOAD")
    UPLOAD=$(escape_md "$UPLOAD")

    # Create the message with MarkdownV2 for bold labels and arrows
    CURRENT_TIME=$(date +"%H:%M:%S")  # Get only the time
    MESSAGE=$(printf "*Internet Speed Test Results*\n\n*Ping:* %s\n*Download:* ⬇️ %s\n*Upload:* ⬆️ %s\n*Time:* %s" "$PING" "$DOWNLOAD" "$UPLOAD" "$CURRENT_TIME")
fi

# -------------------------------
# Send the Message to Telegram
# -------------------------------

# Log the message before sending
echo "$(date +"%Y-%m-%d %H:%M:%S") - Preparing to send message: $MESSAGE" >> "$LOG_FILE"

# Send the message to Telegram with MarkdownV2 formatting
RESPONSE=$(curl -s -X POST "https://api.telegram.org/bot${BOT_TOKEN}/sendMessage" \-d chat_id="${CHAT_ID}" \-d text="${MESSAGE}" \-d parse_mode="MarkdownV2")

# Log the Telegram API response
echo "$(date +"%Y-%m-%d %H:%M:%S") - Telegram Response: $RESPONSE" >> "$LOG_FILE"

# Check for success or failure
if [[ $RESPONSE == *'"ok":true'* ]]; then
    echo "Message sent successfully"
else
    echo "Failed to send message"
fi
```


