# Pavlov Bot Documentation

## Introduction
Pavlov Bot is a Discord bot designed to interface with Pavlov VR RCON, allowing server administrators to manage their servers efficiently.

## Setup Guide
This guide provides installation instructions for both Linux and Windows environments.

---

## Linux Installation (Ubuntu 18.04 or Later)

### Prerequisites
Ensure you have the following installed:
- `pip3`
- `python3.8`
- `pipenv`

### Installing Required Packages
#### Install `pip3`
```bash
sudo apt install python3-pip
```
#### Install `python3.8` (Required for Ubuntu versions prior to 20.04)
```bash
sudo apt update
sudo apt upgrade
sudo apt install software-properties-common
sudo add-apt-repository ppa:deadsnakes/ppa
sudo apt install python3.8
```
Verify installation:
```bash
python3.8 --version
```
#### Install `pipenv`
```bash
pip3 install pipenv
```

### Download and Configure Pavlov Bot
```bash
su - steam
cd ~ && git clone https://github.com/makupi/pavlov-bot
```
Copy and configure required files:
```bash
cp Examples/config.json.default ~/pavlov-bot/config.json
cp Examples/servers.json.default ~/pavlov-bot/servers.json
cp Examples/aliases.json.default ~/pavlov-bot/aliases.json # Optional
cp Examples/commands.json.default ~/pavlov-bot/commands.json # Optional
cp Examples/polling.json.default ~/pavlov-bot/polling.json # Optional
cp Examples/lists.json.default ~/pavlov-bot/lists.json # Optional
```
Edit `config.json` to insert your bot token.

### Install Dependencies
```bash
cd ~/pavlov-bot && pipenv install
```

### Start Pavlov Bot
```bash
cd ~/pavlov-bot && pipenv run python3.8 run.py
```

### Running as a Systemd Service
Create a systemd service file:
```ini
[Unit]
Description=Pavlov Bot

[Service]
Type=simple
WorkingDirectory=/home/steam/pavlov-bot
ExecStart=/usr/local/bin/pipenv run python3.8 run.py
RestartSec=1
Restart=always
User=steam
Group=steam

[Install]
WantedBy=multi-user.target
```
Enable and start the service:
```bash
sudo systemctl enable pavlov-bot
sudo systemctl start pavlov-bot
```
Check logs:
```bash
journalctl -n 20 -f -u pavlov-bot
```

---

## Windows Installation

### Prerequisites
- Install [Python 3.8](https://www.python.org/downloads/release/python-380/)
- Install [Git](https://git-scm.com/downloads)
- Install [pipenv](https://pipenv.pypa.io/en/latest/):
  ```powershell
  pip install pipenv
  ```

### Download and Configure Pavlov Bot
```powershell
git clone https://github.com/makupi/pavlov-bot
cd pavlov-bot
```
Copy and configure required files:
```powershell
Copy-Item Examples\config.json.default -Destination config.json
Copy-Item Examples\servers.json.default -Destination servers.json
Copy-Item Examples\aliases.json.default -Destination aliases.json  # Optional
Copy-Item Examples\commands.json.default -Destination commands.json  # Optional
Copy-Item Examples\polling.json.default -Destination polling.json  # Optional
Copy-Item Examples\lists.json.default -Destination lists.json  # Optional
```
Edit `config.json` to insert your bot token.

### Install Dependencies
```powershell
pipenv install
```

### Start Pavlov Bot
```powershell
pipenv run python run.py
```

### Running as a Windows Service (Optional)
To make the bot run automatically:
1. Open Task Scheduler
2. Create a new task
3. Set the action to run `pipenv run python run.py` in the bot directory.

---

## Updating Pavlov Bot
To update, run:
```bash
cd ~/pavlov-bot && git pull && pipenv sync
systemctl restart pavlov-bot  # Linux only
```
For Windows:
```powershell
cd pavlov-bot
git pull
pipenv sync
```

---

## Roles and Permissions
The bot has four permission levels:
- **Everyone**: Basic commands like `;servers`, `;serverinfo`, `;players`.
- **Captain**: Additional map rotation commands.
- **Mod**: Kick and ban commands.
- **Admin**: Full control.

Admins are defined in `servers.json`. Other roles are configured in Discord as `{role}-{server}` (e.g., `Mod-TestServer`).

---

## Advanced Features
- **Aliases**: Use `aliases.json` to map UGC###/SteamID to player-friendly names.
- **Team Management**: Set up teams using `;teamsetup`.
- **Automated Monitoring**: Configure polling in `polling.json`.
- **Custom Commands**: Define CLI commands in `commands.json`.
- **Automated Game Setup**: Use `;gamesetup` for Discord-controlled match management.

For further details, check the [official documentation](https://github.com/makupi/pavlov-bot).


