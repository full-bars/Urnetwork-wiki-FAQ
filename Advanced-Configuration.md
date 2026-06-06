# Advanced Configuration

The URnetwork provider supports several advanced settings for monitoring, stability, and deployment automation.

### Environment Variables

#### Authentication (Docker/CI)
* **`URNETWORK_AUTH_CODE`**: Pass a fresh auth code for first-time authentication on headless servers.
* **`URNETWORK_JWT`**: Pass a persistent JWT token for long-term deployments (avoids needing a fresh code every 5 minutes).

#### Network & Identity
* **`URNETWORK_NODE_NAME`**: Set a custom label for your device that will appear in the web dashboard.
* **`URNETWORK_PUBLIC_IP`**: Manually specify your public IP if automatic detection is failing behind complex NATs.

### CLI Flags

The provider binary (`urnetwork provide`) supports several run-time flags:

| Flag | Default | Description |
| :--- | :--- | :--- |
| `--api_url` | `https://api.bringyour.com` | Custom API server URL |
| `--connect_url` | `wss://connect.bringyour.com` | Custom WebSocket connect URL |
| `--port` | `0` (random) | Status server port |
| `--max-memory` | none | Soft memory limit (supports `b`, `kib`, `mib`, `gib` suffixes) |
| `-v` | none | Verbose logging (repeat for more detail: `-vv`) |
| `-f` | none | Force overwrite existing JWT during auth |

Use `urnetwork provide --help` to see the full list of supported flags for your installed version.

### Systemd Service Management (Linux)

When installed via the official script, the provider runs as a systemd user service:

```bash
systemctl --user start urnetwork    # Start
systemctl --user stop urnetwork     # Stop
systemctl --user enable urnetwork   # Start on login
systemctl --user disable urnetwork  # Disable start on login
```

### Memory Limits

The `--max-memory` flag applies a soft memory limit. When the provider's memory usage approaches the limit, it reduces buffer sizes and throttles new connections to stay within bounds. This is useful for low-memory devices like Raspberry Pi or cheap VPS instances.

---

### High-Performance Fork (v3.23-fix) Features
The [v3.23-fix fork](https://github.com/full-bars/urnetwork-3.23-fix) introduces several optimizations for power users and high-volume nodes.

#### 1. Performance Profiles
Use the `URNETWORK_PROFILE` variable to adapt the provider to your hardware:

| Profile | Description |
| :--- | :--- |
| `auto` | (Default) Balanced settings for most users. |
| `lowmem` | Reduced buffer sizes and strict memory limits. Ideal for low-end VPS or IoT devices. |
| `eco` | Optimized for power saving (mobile/laptops). |
| `turbo-v4` | Expanded message pools and deeper buffers for 1Gbps+ connections. |
| `turbo-v8` | Maximum performance settings for multi-gigabit infrastructure. |

#### 2. Logging & Monitoring
* **`URNETWORK_RAMLOGS=1`**: Enables high-speed logging to RAM. This is exclusive to the fix-fork and critical for extending the lifespan of SD cards on Raspberry Pi nodes.

#### 3. Optimized Docker Images
When running in Docker, the optimized image (`ghcr.io/full-bars/urnetwork-3.23-fix`) includes:
* **vnStat Integration:** Real-time traffic monitoring inside the container.
* **Hardened Buffers:** 4x deeper IP buffer depths to prevent packet drops during traffic bursts.
* **Multi-Arch Support:** Native builds for both `AMD64` and `ARM64`.
