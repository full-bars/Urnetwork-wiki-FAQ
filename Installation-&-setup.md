# Installation & setup

### Which platforms are supported?

| Platform | Method |
| :--- | :--- |
| **Android** | App (Android 8+), or Termux for Android 7+ |
| **iOS** | App Store |
| **Linux** | Binary or Docker (`bringyour/community-provider:g4-latest`) |
| **macOS** | Binary via install script |
| **Windows** | PowerShell install script |

*The Android app requires Android 8 (Oreo) or newer. Using Termux can extend support to Android 7. Note: Apple and Google Play policies can limit background uptime on mobile devices.*

### How do I install the provider?
The official install script is available at [ur.io](https://ur.io). On Linux or macOS, you can manage your node via CLI:

```bash
urnetwork provide     # Start providing bandwidth
urnetwork auth        # Set or update authentication (JWT/Auth Code)
urnet-tools -help     # View all available management tools
```

On macOS, you can stop the provider service using:
```bash
sudo launchctl stop /Library/LaunchAgents/urnetwork-provider.plist
```

> **Success:** No port forwarding is required. The provider is designed to work automatically behind most home routers.

### Version 2026.3.23 Update (Quiet Logs)
In the latest versions, the provider binary has streamlined logging. The previously common `fragment success=1 error=0` messages are now hidden by default to reduce CPU overhead and log noise.
* If your logs appear "idle", it often means the node is waiting for a contract or performing internal API calls (e.g., to `api.bringyour.com`).

### Where do I get the latest APK for Android?
Latest builds are on GitHub: `https://github.com/urnetwork/build/releases/latest`

Use **Obtainium** for automatic updates. Add `https://github.com/urnetwork/build` and select the appropriate release flavor.
* **Stable:** Recommended for most users.
* **Nightly:** Includes the latest features but may have bugs.

### Monitoring Node Health
Visit [app.ur.network/account](https://app.ur.network/account) to see your active clients.
* **Green:** Connected and healthy.
* **Yellow/Red:** Connectivity issues.
* **Reliability Graph:** Shows your uptime over time. Note that mobile devices often show lower reliability due to OS background restrictions. For 24/7 uptime, a wired PC or VPS is recommended.

> **Tip:** Log data is not considered personal; sharing logs in Discord `#support` is the fastest way to get help with specific errors.
