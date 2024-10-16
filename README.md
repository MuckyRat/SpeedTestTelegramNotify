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
3. Copy the Script: Copy and paste the code from speedtest_telegram.sh
4. Save and Exit: If using nano, press CTRL + O to save, then CTRL + X to exit.

### Step 3: Make the code executable. 
   ```bash
   chmod +x ~/scripts/speedtest_telegram.sh
   ```
### Configuration





