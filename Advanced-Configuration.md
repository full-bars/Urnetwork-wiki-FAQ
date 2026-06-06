# Advanced Configuration

The URnetwork provider supports several advanced settings for monitoring and stability. For users of the **high-performance fork (v3.23-fix)**, additional tuning options are available.

### Standard Configuration
* **`URNETWORK_NODE_NAME`**: Set a custom label for your device that will appear in the web dashboard.
* **`URNETWORK_PUBLIC_IP`**: Manually specify your public IP if the automatic detection is failing behind complex NATs.
* **`urnetwork provide --help`**: Always check the local CLI help for the full list of supported flags in your specific version.

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
