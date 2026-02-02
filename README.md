# Command Runner Integration for Home Assistant

[![hacs_badge](https://img.shields.io/badge/HACS-Custom-orange.svg)](https://github.com/custom-components/hacs)

Securely control your Mac remotely from Home Assistant using the Command Runner app.

## 🔒 Version 2.0 - Now with API Key Authentication

This version adds **API key authentication** for secure access to your Mac.

## Features

- 🔐 **Secure Authentication** - API key-based authentication
- 🎯 **Button Entities** - Each enabled command becomes a button
- 🔄 **Auto-Discovery** - Automatically detects all available commands
- 📊 **Real-time Updates** - Polls for new commands every 30 seconds
- 🏠 **Local Control** - No cloud required, works on your local network
- 📱 **Easy Setup** - Simple configuration with IP, port, and API key

## Prerequisites

1. **Command Runner app** v2.0+ installed and running on your Mac
2. Mac and Home Assistant on the same network
3. **API Key** generated from Command Runner Settings

## Installation

### HACS (Recommended)

1. Open HACS in Home Assistant
2. Click the three dots in the top right corner
3. Select "Custom repositories"
4. Add this repository URL: `https://github.com/yourusername/command_runner`
5. Select category: "Integration"
6. Click "Add"
7. Find "Command Runner" in HACS and click "Download"
8. Restart Home Assistant

### Manual Installation

1. Download the `command_runner` folder
2. Copy it to `config/custom_components/command_runner`
3. Restart Home Assistant

## Configuration

### Step 1: Generate API Key on Mac

1. Open **Command Runner** on your Mac
2. Go to **Settings** (⌘,)
3. Click **Generate Key...**
4. Enter a name: `Home Assistant`
5. Click **Generate**
6. **Copy the API key** (you won't see it again!)

### Step 2: Add Integration in Home Assistant

1. Go to **Settings** → **Devices & Services**
2. Click **+ Add Integration**
3. Search for "Command Runner"
4. Enter:
   - **IP Address**: Your Mac's IP (e.g., `192.168.1.100`)
   - **Port**: `8080` (or your custom port)
   - **API Key**: Paste the key you copied
5. Click **Submit**

The integration will:
- Connect securely to your Mac
- Fetch all enabled commands
- Create button entities for each command

## Security

### Why API Keys?

Without API keys, anyone on your network could execute commands on your Mac. API keys ensure only authorized clients (like Home Assistant) can access your Mac.

### Best Practices

✅ **Generate a unique key** for each client  
✅ **Name your keys** (e.g., "Home Assistant", "iPhone")  
✅ **Delete unused keys** when no longer needed  
✅ **Never share your keys** publicly  
✅ **Regenerate keys** if compromised  

### No API Keys (Legacy Mode)

If you don't generate any API keys in Command Runner, the server works without authentication (like version 1.0). This is **not recommended** for security reasons.

## Usage

### In Home Assistant

Each command appears as a button entity:
- `button.open_calculator`
- `button.lock_screen`
- `button.say_hello`

Press any button to execute the command on your Mac.

### Automations Example

```yaml
automation:
  - alias: "Lock Mac at midnight"
    trigger:
      platform: time
      at: "00:00:00"
    action:
      service: button.press
      target:
        entity_id: button.lock_screen

  - alias: "Open Calculator when arriving home"
    trigger:
      platform: state
      entity_id: person.you
      to: "home"
    action:
      service: button.press
      target:
        entity_id: button.open_calculator
```

### Lovelace Card Example

```yaml
type: entities
title: Mac Commands
entities:
  - button.open_calculator
  - button.open_safari
  - button.lock_screen
  - button.say_hello
```

## Troubleshooting

### Cannot Connect

1. Verify Command Runner is running on your Mac
2. Check the IP address: `System Settings → Network`
3. Ensure port is correct (check Command Runner → Settings)
4. Test connection: `curl http://YOUR_IP:8080/commands -H "X-API-Key: YOUR_KEY"`
5. Check firewall settings on your Mac

### Invalid API Key

1. Verify you copied the entire key
2. Check the key hasn't been deleted in Command Runner
3. Generate a new key if needed
4. Update the integration with the new key

### Commands Not Appearing

1. Open Command Runner on your Mac
2. Verify commands are enabled (checkboxes ticked)
3. In Home Assistant, reload the integration

### Command Execution Fails

1. Test command directly in Command Runner app
2. Check Home Assistant logs
3. Verify command is still enabled
4. Check API key is still valid

## Upgrading from v1.0

If upgrading from version 1.0 (no API keys):

1. Update Command Runner app on your Mac
2. Generate an API key in Settings
3. In Home Assistant:
   - Go to **Settings** → **Devices & Services**
   - Find Command Runner integration
   - Click **Configure**
   - Enter the API key
   - Save

## Advanced

### Command Attributes

Each button entity includes attributes:
- `command` - The actual shell command
- `allow_parameters` - Whether parameters are supported
- `voice_trigger` - Voice trigger phrase

Access in automations:
```yaml
{{ state_attr('button.open_calculator', 'command') }}
```

### Multiple Macs

You can add multiple Mac computers:
1. Each Mac needs Command Runner installed
2. Add the integration once per Mac
3. Each will have its own set of button entities

### API Key Management

In Command Runner Settings, you can:
- See all generated keys
- View creation dates
- Delete unused keys
- Generate new keys for different clients

## Support

- Report issues: https://github.com/yourusername/command_runner/issues
- Discussions: https://github.com/yourusername/command_runner/discussions

## Changelog

### v2.0.0
- ✨ Added API key authentication
- 🔒 Improved security
- 📝 Better error messages
- 🐛 Bug fixes

### v1.0.0
- 🎉 Initial release

## License

MIT License - See LICENSE file for details
