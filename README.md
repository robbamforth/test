# Command Runner

A macOS application that allows you to remotely execute commands on your Mac via HTTP API.

## Features

- Execute shell commands remotely via HTTP
- Manage commands through a user-friendly interface
- Status bar menu for quick access
- Enable/disable individual commands
- Support for command parameters
- Built-in HTTP server on port 8080

## Installation

1. Open `CommandRunner.xcodeproj` in Xcode
2. Build and run the project (Cmd+R)
3. The app will appear in your menu bar

## Usage

### Managing Commands

1. Click the terminal icon in your menu bar to open the main window
2. Use the "Add" button to create new commands
3. Select a command and click "Edit" to modify it
4. Use "Test" to execute a command locally
5. Use "Remove" to delete commands

### Remote Execution

From any Mac on your network, use curl:

```bash
# List all available commands
curl http://YOUR_MAC_IP:8080/commands

# Execute a specific command
curl http://YOUR_MAC_IP:8080/run/Open%20Calculator

# Execute with parameters
curl "http://YOUR_MAC_IP:8080/run/MyCommand?params=value"
```

### API Endpoints

- `GET /` - Welcome page
- `GET /commands` - List all enabled commands
- `GET /run/{commandName}` - Execute a command
- `GET /run/{commandName}?params=value` - Execute with parameters

## Security

⚠️ **Warning**: This app executes shell commands without authentication. Only use on trusted networks.

## Requirements

- macOS 13.0 or later
- Xcode 14.0 or later
- Swift 5.0 or later

## License

MIT License

## Sensors

The integration provides the following sensors to monitor your Command Runner server:

- **Status**: Current server status (running/stopped)
- **Version**: Application version
- **Port**: Server port number
- **Uptime**: Server uptime in seconds with formatted display (e.g., "2d 5h 30m")
- **Total Requests**: Total number of requests handled since server start
- **Requests Processing**: Current number of requests being processed
- **API Keys Configured**: Whether API keys are configured (Yes/No)
- **Last Request**: Timestamp of the last request received

All sensors update every 30 seconds automatically.

## Version History

### v1.1.0
- Added server status sensors
- Added `/status` endpoint support
- Enhanced monitoring capabilities

### v1.0.0
- Initial release
- Button entities for command execution
- API key authentication support
