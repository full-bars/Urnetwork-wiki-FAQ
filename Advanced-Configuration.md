# Advanced Configuration

For power users and providers running on dedicated hardware, URnetwork supports several environment variables to tune performance and resource usage.

### 1. Performance Profiles
Use the `URNETWORK_PROFILE` variable to adapt the provider to your hardware:

| Profile | Description |
| :--- | :--- |
| `auto` | (Default) Balanced settings for most users. |
| `lowmem` | Reduced buffer sizes and strict memory limits. Ideal for low-end VPS or IoT devices. |
| `eco` | Optimized for power saving (mobile/laptops). |
| `turbo-v4` | Expanded message pools and deeper buffers for 1Gbps+ connections. |
| `turbo-v8` | Maximum performance settings for multi-gigabit infrastructure. |

### 2. Logging & Monitoring
* `URNETWORK_RAMLOGS=1`: Enables high-speed logging to RAM. Reduces disk I/O, which is critical for extending the lifespan of SD cards (Raspberry Pi).
* `URNETWORK_NODE_NAME`: Set a custom label for your device that will appear in the web dashboard.
* `URNETWORK_PUBLIC_IP`: Manually specify your public IP if the automatic detection is failing behind complex NATs.

### 3. Docker Optimization
When running in Docker, it is recommended to use the optimized community image:
`ghcr.io/full-bars/urnetwork-3.23-fix`

**Key Features of Optimized Images:**
* **vnStat Integration:** Real-time traffic monitoring inside the container.
* **Multi-Arch Support:** Native builds for both `AMD64` (PCs) and `ARM64` (Raspberry Pi/Mac).
* **Hardened Buffers:** 4x deeper IP buffer depths to prevent packet drops during traffic bursts.

---
*Tip: Always use `urnetwork provide --help` to see the full list of CLI flags supported by your current version.*
