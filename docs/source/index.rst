==========
Pavlov-Bot
==========

Discord bot to interface with Pavlov VR RCON.

Setup
-----
This setup guide assumes you are running Ubuntu 18.04 or later. Later versions may already have the required Python 3.8 version. It also assumes you are running the bot on the same server running `pavlovserver`, following the setup guide found here.

Prerequisites
-------------
- `pip3`
- `python3.8`
- `pipenv`

Installing pip3
---------------

```bash
sudo apt install python3-pip
```

Installing Python 3.8 (required for Ubuntu prior to 20.04)
----------------------------------------------------------

Update your system and install prerequisites:

```bash
sudo apt update
sudo apt upgrade
sudo apt install software-properties-common
```

Install PPA for Python 3.8:

```bash
sudo add-apt-repository ppa:deadsnakes/ppa
```

Press `Enter` when prompted.

Install Python 3.8:

```bash
sudo apt install python3.8
```

Verify installation:

```bash
python3.8 --version
```

Getting Pavlov-Bot Code and Configuring
----------------------------------------

Log in as the steam user:

```bash
su - steam
```

Clone the repository:

```bash
cd ~ && git clone https://github.com/makupi/pavlov-bot
```

Copy configuration files:

```bash
cp ~/pavlov-bot/Examples/config.json.default ~/pavlov-bot/config.json
```

Modify `config.json` with:

```json
{"prefix": ";", "token": "replacemewithdiscordtoken"}
```

Copy and edit `servers.json`:

```bash
cp ~/pavlov-bot/Examples/servers.json.default ~/pavlov-bot/servers.json
```

Modify `servers.json` with your server details. Admins are specified using their Discord IDs.

Optional Configuration Files
----------------------------
- `aliases.json` - Define map and player aliases.
- `commands.json` - Configure permissions for commands.
- `polling.json` - Enable monitoring and auto-balancing.
- `lists.json` - Manage dropdown lists for `;menu` or `;gamesetup`.

Setting Up the Bot in Discord
-----------------------------
Follow the instructions [here](https://discord.com/developers/applications/) to obtain a bot token and configure it in `config.json`.

Enable Message Content Intent:

Navigate to:

```
https://discord.com/developers/applications/
```

Under `Bot` settings, enable `Message Content Intent`.

Installing Pipenv
-----------------

As root user:

```bash
pip3 install pipenv
```

Setup pipenv for Pavlov-Bot:

```bash
cd ~/pavlov-bot && pipenv install
```

Running the Bot
---------------

Test startup:

```bash
cd ~/pavlov-bot && /usr/local/bin/pipenv run python3.8 run.py
```

Try commands like `;help`, `;info`, and `;servers`.

Running as a Systemd Service
----------------------------

Create a service file:

```bash
/etc/systemd/system/pavlov-bot.service
```

Content:

```ini
[Unit]
Description=Pavlov-bot

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
systemctl enable pavlov-bot
systemctl start pavlov-bot
```

Check logs:

```bash
journalctl -n 20 -f -u pavlov-bot
```

Updating Pavlov-Bot
--------------------

To update from the master branch:

```bash
cd /home/steam/pavlov-bot
git pull
pipenv sync
systemctl restart pavlov-bot
```

Roles and Permissions
---------------------

Permission Levels:

- **Everyone**: `;servers`, `;serverinfo`, `;players`, `;batch`
- **Captain**: Everything above + `;switchmap`, `;resetsnd`, `;switchteam`, `;rotatemap`
- **Mod**: Everything above + `;ban`, `;unban`, `;kick`
- **Admin**: Full access

Admins are defined in `servers.json`, while other roles use Discord roles in the format `{role}-{server}` (e.g., `Mod-testserver`). `Captain-bot` and `Mod-bot` provide global permissions.

Advanced Features
-----------------

**Aliases:**
Defined in `aliases.json`, allowing custom names for players and maps.

**Team Management**

- `;ringer add` / `;ringer delete` / `;ringer reset`
- `;teamsetup` for ad-hoc teams
- `;matchsetup <CT Team> <T Team> <server>`

**Game Control**

- `;gamesetup` - Button-based SND match control
- `;menu` - Server selection via dropdowns
- `;custom "<command string>" <server>` - Execute custom RCON commands
- `;flush <server>` - Kick random non-aliased player
- `;repeat <command> <number>` - Execute command multiple times
- `;switchmap` - Can accept `UGC###` or workshop URLs
- `;command <command_name>` - Execute predefined server commands

**Monitoring & Auto-Management**

- `;anyoneplaying` - Check all servers
- `polling.json` - Auto-balancing and notifications

==========
Pavlov-Bot for Windows
==========

Windows Installation
--------------------

The same steps apply, with the following additions:

After installation, run:

```bash
pip install aiohttp  # This is a common error, so install it manually
python run.py        # Start the bot
```


