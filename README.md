# VNC for Ona

An optimized VNC setup for Ona Environments. It's based on Ubuntu 24.04 with an XFCE desktop, KasmVNC server, and Google Chrome preinstalled. It also has IntelliJ Idea if you want to run that. It is tailored for Ona workflows that need a lightweight yet complete remote desktop accessible from any modern browser.

## What's Included
- Ubuntu 24.04 base image wrapped in the devcontainers/base toolchain
- KasmVNC server configured on port 5901 with WebSocket access
- XFCE desktop session launched through `vncserver`
- Google Chrome available system-wide (also aliased to `chromium` for compatibility)
- IntelliJ Idea (on-demand)
- Docker-in-Docker feature enabled so Docker CLI commands work inside the container

## Quick Start

Click the button below to start your environment:

[![Open in Ona](https://gitpod.io/button/open-in-gitpod.svg)](https://app.gitpod.io/#https://github.com/gitpod-samples/vnc)

Then open port 5901 from the "Environment" tab in Ona. You can also launch your env in VSCode (desktop client) for localhost port forwarding and then open the 5091 port from the PORTS tab in VSCode.

Note: You can also fork this repository or copy the [.devcontainer](./devcontainer) folder to your own repo.

## Desktop Details
- Display: DISPLAY variable defaults to `:1` inside the container
- Session: XFCE (`xfce4-session`) is started via `~/.xinitrc`
- VNC password: stored in `~/.kasmpasswd`, set to `123456`
- Config: Additional KasmVNC settings live in `~/.vnc/kasmvnc.yaml`

## Chrome Notes
- Installed via Google-provided `.deb`
- Symlinked to `/usr/bin/chromium` so tools expecting Chromium also work
- Chrome first-run and default-browser prompts are disabled for better automation
- Qt WebEngine sandbox is disabled via `QTWEBENGINE_DISABLE_SANDBOX=1`

## Docker Access
The container is privileged and mounts the host Docker socket. Inside the dev container you can run Docker CLI commands against the host daemon. Be mindful that changes affect the host Docker runtime.

## Developing and Testing
- Use the integrated XFCE terminal (`xfce4-terminal`) or VS Code terminal for shell work
- Launch Chrome from the panel or the terminal (`google-chrome` or `chromium`)
- If you need to restart VNC: `sudo service dbus start` then `vncserver -kill :1 && vncserver -disableBasicAuth -alwaysshared`

## Troubleshooting
- **Cannot connect to VNC**: Confirm port 5901 is forwarded and VNC server is running (`vncserver -list`). Restart the VNC server with the command above.
- **Chrome rendering issues**: Ensure `/dev/shm` has sufficient space; this container sets `--shm-size=2gb` to avoid crashes.
- **Docker commands fail**: Verify the host Docker socket is mounted and your host user has permission to access it.
