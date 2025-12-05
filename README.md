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

The template provides the following configurable settings through AMP's interface. These can be configured in the **Settings** section of your AMP instance:

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
   - Or add to startup: `+com_protocolname NZP-REBOOT`

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

