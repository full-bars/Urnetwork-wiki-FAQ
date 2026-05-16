# Common errors

| Error | Meaning & Fix |
| :--- | :--- |
| `auth error ... = No successful strategy found` | The node cannot announce itself to the network. Usually caused by a **URnetwork API outage** or local network interruption. **Fix:** Wait for service recovery; check [status.ur.io](https://status.ur.io). |
| `panic: Invalid auth code` | Auth codes expire in 5 minutes and are single-use. **Fix:** Generate a fresh code from [app.ur.network/account](https://app.ur.network/account) and don't reuse codes across multiple devices. |
| `exit could not create contract` | Matchmaking failed to assign a contract to your node. This is often a **network-wide infrastructure issue** and not a problem with your local setup. **Fix:** No action needed; missed credits are often issued retroactively. |
| `panic: 500 Internal Server Error: User does not exist` | Occurs when using an email/Google SSO account with the `--auth_user` CLI flag. SSO accounts do not support this method. **Fix:** Generate an **Auth Code** or **JWT Token** from the web dashboard instead. |
| `Stuck on "Connecting..." (client)` | Your node is having trouble establishing a tunnel. **Fix:** Ensure you are using the **Client ID** (not the network name) to connect. Check logs for specific errors. |
| `User auth attempts exceeded limits` | You are being rate-limited for too many authentication attempts in a short time. **Fix:** Wait 15–20 minutes and try again one device at a time. |

### Monitoring Best Practices
* **success:error Ratio:** In older versions, this was the primary health check. In versions **2026.3.23+**, these logs are hidden. 
* **Reliability Graph:** Use the dashboard graph to confirm 24/7 uptime. If the graph is mostly red/yellow, your device is being throttled or disconnected by the OS (common on mobile).
* **NSSF (No Successful Strategy Found):** If this spams for more than a few minutes, it usually indicates a temporary API lag or outage.

> **Staff Tip:** As long as you see occasional traffic movements or your reliability score remains high, your node is functioning. Infrastructure-related errors (like contract creation failures) are managed by the dev team and do not require user intervention.