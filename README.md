# Nazi Zombies Portable AMP Template

An AMP (Application Management Panel) template for hosting Nazi Zombies Portable (NZP) dedicated servers.

This template is part of the [AMPTemplates repository](https://github.com/l0cks-acab/AMPTemplates).

## About NZP

Nazi Zombies Portable (NZP) is a Quake-based first-person shooter game featuring zombie survival gameplay. This template allows you to easily deploy and manage NZP dedicated servers using AMP.

## Prerequisites

Before using this template, you need to download and set up the NZP server files from the [NZP GitHub Releases](https://github.com/nzp-team/nzportable/releases) page.

### Download Required Files

You need to download **3 files** from the NZP GitHub Releases page:

1. **NZP Binary**: `nzportable-linux64.zip` (or `nzportable-win64.zip` for Windows)
   - Extract to get the executable (`nzportable64-sdl` for Linux or `nzportable.exe` for Windows)

2. **Game Assets**: `pc-nzp-assets.zip`
   - This contains all the game assets and will create the `nzp/` folder

3. **QuakeC Files**: 
   - `fte-nzp-qc.zip`
   - `standard-nzp-qc.zip`
   - These contain the compiled QuakeC files (`.dat` files)

### Setting Up the Server Files Directory

After creating your AMP instance, you need to merge these 3 files into a `serverfiles/` directory with the following structure:

```
serverfiles/
├── nzportable64-sdl          ← Executable (from nzportable-linux64.zip)
├── nzp/                      ← Assets folder (from pc-nzp-assets.zip)
│   ├── csprogs.dat          ← QuakeC files go here (from fte-nzp-qc.zip)
│   ├── menu.dat
│   ├── qwprogs.dat
│   └── (other assets)
└── (config files if any)
```

**Setup Steps:**

1. Create a `serverfiles/` folder (or use the one created by AMP)
2. Extract the **NZP binary** (`nzportable-linux64.zip`) and place the executable (`nzportable64-sdl`) directly in `serverfiles/`
3. Extract **game assets** (`pc-nzp-assets.zip`) - this creates an `nzp/` folder. Place this `nzp/` folder directly in `serverfiles/`
4. Extract **QuakeC files**:
   - Extract `fte-nzp-qc.zip` into the `serverfiles/nzp/` folder
   - Extract `standard-nzp-qc.zip` into the `serverfiles/nzp/` folder
   - After extracting, you should see `.dat` files (like `csprogs.dat`, `menu.dat`, `qwprogs.dat`) in `serverfiles/nzp/`

5. Upload the entire `serverfiles/` folder to your AMP instance using:
   - **AMP File Manager**: Navigate to your instance → **File Manager** → upload to `serverfiles/`
   - **FTP/SFTP**: Upload directly to the server
   - **SSH/Command Line**: Copy files to the serverfiles directory

**Important Notes:**
- On Linux, make sure the executable has execute permissions: `chmod +x serverfiles/nzportable64-sdl`
- The executable name may vary (`nzportable64-sdl`, `nzportable64`, or `nzportable.exe` on Windows). The template is configured for `nzportable64-sdl` on Linux. If your executable has a different name, either rename it or update the executable path in AMP instance settings after creation.

### Setting Up the Server Files

After creating your AMP instance, follow the setup steps in the [Prerequisites](#prerequisites) section above to download and merge the 3 required files into a `serverfiles/` directory with the correct structure.

Then upload the `serverfiles/` folder to your AMP instance using:

1. **AMP File Manager**: Navigate to your instance → **File Manager** → upload files to `serverfiles/`
2. **FTP/SFTP**: Connect to your server and upload files directly
3. **SSH/Command Line**: Copy files directly to the serverfiles directory

**Important Notes**:
- The executable must be in `serverfiles/` (not a subdirectory)
- The `nzp/` folder with all assets must be directly in `serverfiles/` (alongside the executable)
- Make sure the executable has execute permissions on Linux: `chmod +x serverfiles/nzportable64-sdl`
- Verify that the `.dat` files (like `csprogs.dat`, `menu.dat`, `qwprogs.dat`) are present in the `serverfiles/nzp/` folder

## Configuration

### Configurable Settings

The template uses a config file approach to manage server settings. Settings are configured through the AMP interface and automatically written to `nzp/AMP_server.cfg`, which is loaded when the server starts.

The template uses:
- `nazi-zombies-portableconfig.json` - Defines the configurable settings in the AMP interface
- `nazi-zombies-portablemetaconfig.json` - Defines the config file location and template
- Config file: `nzp/AMP_server.cfg` - Where settings are written and loaded from

These settings should appear in the **Settings** tab of your AMP instance. Default values are defined in the config file:

- **Protocol Name**: `NZP-REBOOT` (default - for desktop clients)
- **Map**: `ndu` (default - Nacht der Untoten)

### Available Settings

The following settings are available to configure through the AMP interface:

- **Protocol Name** (`com_protocolname`): Server protocol identifier (default: "NZP-REBOOT")
  - Use "NZP-REBOOT" for desktop clients
  - Use "NZP-REBOOT-WEB" for web clients
- **Map** (`map`): The map to load on server start (default: "ndu" - Nacht der Untoten)
- **Server Port (UDP)** (`sv_port`): UDP port for game connections (default: 27500)
  - Must match the port configured in AMP's port settings
- **Server Port (TCP/WebSocket)** (`sv_port_tcp`): TCP/WebSocket port for web client connections (default: 27501)
  - Set to 0 to disable WebSocket support
- **Server Hostname** (`hostname`): Display name for your server in the server browser (default: "NZP Server")
- **Max Clients** (`maxclients`): Maximum number of players that can join the server (default: 8, max: 64)
- **Public Server** (`sv_public`): Whether the server should be publicly listed in the server browser (default: true)
- **Minimum Tick Rate** (`sv_mintic`): Minimum server tick rate in seconds (default: 0.045 = 45ms = ~22fps)
  - Lower values = higher tickrate but more bandwidth
- **Maximum Tick Rate** (`sv_maxtic`): Maximum server tick rate (default: 1000)
  - Higher values = smoother gameplay but more CPU usage

### Manual Configuration via Console

If the Settings tab is not available, you can configure settings manually via the server console:

1. Go to your AMP instance → **Console** tab
2. While the server is running, type these commands:

```bash
set com_protocolname NZP-REBOOT
set sv_port 27500
set hostname "NZP Server"
set maxclients 8
set sv_public 1
```

**Note**: Settings changed via console will reset when the server restarts. To make changes permanent, add them to the startup command in the **Configuration** tab → **Startup/Command Line** section using the format `+cvar value` (e.g., `+com_protocolname NZP-REBOOT`).

## Usage

1. Configure your server settings in the AMP interface
2. Start the server instance
3. The server will launch with the `-dedicated` flag and your configured settings
4. Players can connect using the server's IP address and UDP port

## Server Management

- **Start**: Starts the NZP dedicated server
- **Stop**: Sends the `quit` command to gracefully shut down the server
- **Restart**: Stops and starts the server
- **Console**: Access the server console through AMP to run commands and view logs

## Ports

The template defines one port that AMP will manage:

- **UDP Port 27500**: Main game port for UDP connections (configurable via `sv_port` setting)

The WebSocket/TCP port (`sv_port_tcp`) is configured via the server settings but not managed by AMP's port system. Make sure the UDP port is open in your firewall if you want the server to be accessible from the internet.

## Troubleshooting

### Settings not appearing in AMP interface

If the configurable settings don't appear in the **Settings** tab:

1. **Refresh the template**:
   - Go to **Configuration** → **Instance Deployment** in AMP
   - Click **"Fetch Latest"** or **"Update Templates"** button
   - Wait for the process to complete
   - Refresh your browser page (F5 or Ctrl+R)

2. **Verify template files are loaded**:
   - Check that `nazi-zombies-portableconfig.json` is present in the template directory
   - Ensure the config file is valid JSON (no syntax errors)

3. **Recreate the instance** (if settings still don't appear):
   - Create a new instance using the updated template
   - The settings should appear in the new instance's **Settings** tab

4. **Check AMP version**:
   - Ensure you're using AMP version 2.4.6.6 or newer (as specified in the template)
   - Older versions may not support configurable settings properly

5. **Manual configuration**:
   - If settings don't appear, you can still configure via console (see Manual Configuration section below)

### Server won't start / "Unable to start the application" / Permission errors

This error usually means the executable doesn't have execute permissions. Here's how to fix it:

**Option 1: Using AMP File Manager**
1. Go to your instance in AMP
2. Navigate to **File Manager** → `serverfiles/`
3. Find `nzportable64-sdl` (or whatever your executable is named)
4. Right-click on it → **Properties** or **Change Permissions**
5. Check the **Execute** permission box
6. Save and try starting again

**Option 2: Using SSH/Command Line (Recommended)**
1. SSH into your server
2. Navigate to the serverfiles directory:
   ```bash
   cd /home/amp/.ampdata/instances/[YOUR_INSTANCE_NAME]/nazi-zombies-portable/serverfiles/
   ```
   (Replace `[YOUR_INSTANCE_NAME]` with your actual instance name, e.g., `test01`)
3. Set execute permissions:
   ```bash
   chmod +x nzportable64-sdl
   ```
4. Verify the file exists and has permissions:
   ```bash
   ls -lh nzportable64-sdl
   ```
   You should see something like `-rwxr-xr-x` (the `x` means executable)

**Other things to check:**
- Verify that the `nzportable64-sdl` binary is in the `serverfiles/` directory (not in a subdirectory)
- Check that the `nzp` folder exists with the required `.dat` files (csprogs.dat, menu.dat, qwprogs.dat)
- Review the AMP console/logs for more specific error messages
- If you're using the SDL version (`nzportable64-sdl`), ensure SDL2 libraries are installed on your server

### "Target not registered" error when connecting

If players see "target not registered" when trying to connect:

1. **Protocol Name Mismatch** (Most Common Cause):
   - The protocol name on the server must exactly match what the client expects
   - **For desktop clients**: The server must use `NZP-REBOOT`
   - **For web clients**: The server must use `NZP-REBOOT-WEB`
   - **Fix via Console** (if Settings tab is not available):
     - Go to your AMP instance → **Console** tab
     - Type: `set com_protocolname NZP-REBOOT`
     - Press Enter
     - The server should accept connections immediately
   - **Fix via Startup Command** (permanent):
     - Go to your AMP instance → **Configuration** tab
     - Find the "Startup/Command Line" section
     - Ensure the command line includes: `+com_protocolname NZP-REBOOT`
     - Save and restart the server
   - **Verify**: Check the server console to confirm the protocol name is set correctly

2. **Server Not Fully Started**:
   - The server may still be initializing when the connection is attempted
   - Wait a few seconds after server start before connecting
   - Check the server console to see if it's fully initialized

3. **Port Mismatch**:
   - Ensure the client is connecting to the correct port
   - Verify the `sv_port` setting matches the port the client is using
   - Check that the port configured in AMP matches the `sv_port` setting
   - You can set the port via console: `set sv_port 27500` (or whatever port you're using)

4. **Server Console Check**:
   - Look at the server console logs when a player tries to connect
   - Check for any error messages or connection attempts being logged
   - Verify the server is actually receiving the connection request

### Players can't connect / SCTP/DTLS errors

If you see errors like `SCTP Abort` or `DTLS Terminated` when players try to connect:

1. **Set the Protocol Name** (Most Common Fix):
   - The server needs the protocol name to match the client type
   - For desktop clients: Use `NZP-REBOOT`
   - For web clients: Use `NZP-REBOOT-WEB`
   - **Quick Fix**: In your AMP instance console, run:
     ```
     set com_protocolname NZP-REBOOT
     ```
   - Or configure in AMP Settings → "Protocol Name" setting

2. **Verify Port Configuration**:
   - Desktop clients connect via UDP (default port 27500)
   - Web clients connect via WebSocket (different port/protocol)
   - Ensure the correct port is open in your firewall
   - Check that the port number matches between server and client

3. **Check Client Type**:
   - If using a web browser client, the server must support WebSocket connections
   - Desktop clients should use UDP protocol
   - Make sure the protocol name matches: `NZP-REBOOT` for desktop, `NZP-REBOOT-WEB` for web

4. **Network Issues**:
   - Ensure the UDP port is open in your firewall
   - Verify the server is set to public if you want it listed
   - Check NAT/port forwarding settings if behind a router

### AMP says port is not listening

If AMP indicates that the UDP port is not listening:

1. **This is often normal for UDP ports**:
   - UDP ports are connectionless and don't "listen" the same way TCP ports do
   - AMP may have difficulty detecting UDP port binding, which is a known limitation
   - The server may still be working correctly even if AMP shows the port as not listening

2. **Verify the port matches**:
   - Check the port number configured in AMP's port settings
   - Ensure the `sv_port` setting in your instance's Settings matches the port number in AMP
   - For example, if AMP is managing port 27015, set `sv_port` to 27015
   - The default port is 27500 - if you changed it in AMP, update `sv_port` to match

3. **Test if the server is actually working**:
   - Try connecting with a client - if connections work, the port is functioning correctly
   - Check the server console logs for any port binding errors
   - Verify the port is open in your firewall

4. **Verify port binding manually** (Linux):
   ```bash
   netstat -unlp | grep [PORT_NUMBER]
   # or
   ss -unlp | grep [PORT_NUMBER]
   ```
   Replace `[PORT_NUMBER]` with your actual port (e.g., 27500 or 27015)

### Missing assets
- Verify the directory structure matches the one in [Prerequisites](#prerequisites)
- Re-download and extract `pc-nzp-assets.zip` into `serverfiles/` to create the `nzp/` folder
- Ensure QuakeC files (`fte-nzp-qc.zip` and `standard-nzp-qc.zip`) are extracted into `serverfiles/nzp/` directory
- Check that you have the required `.dat` files in `serverfiles/nzp/` (csprogs.dat, menu.dat, qwprogs.dat, etc.)


## Additional Resources

- [NZP Official Documentation](https://docs.nzp.gay/)
- [NZP Server Setup Guide](https://docs.nzp.gay/server/server-setup)
- [NZP GitHub Repository](https://github.com/nzp-team/nzportable) - Download binaries and source code
- [NZP GitHub Releases](https://github.com/nzp-team/nzportable/releases) - Download latest builds
- [AMP Generic Module Documentation](https://github.com/CubeCoders/AMP/wiki/Configuring-the-'Generic'-AMP-module)
- [AMP Templates Repository](https://github.com/CubeCoders/AMPTemplates)

## Contributing

If you find issues with this template or have suggestions for improvements, please submit a pull request or open an issue in the repository.

## License

This template is provided as-is for the AMP community. Refer to the AMPTemplates repository for licensing information.

## Notes

- This template uses command-line arguments to configure the server. Settings are passed directly to the NZP binary on startup.
- The template supports both Windows and Linux platforms.
- Make sure you have sufficient resources allocated (minimum 512MB RAM recommended, 1GB+ for better performance).

