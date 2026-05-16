# Authentication

### Auth Codes vs. JWT Tokens
Authentication in URnetwork is handled through the **Account Dashboard** at [app.ur.network/account](https://app.ur.network/account).

* **Authentication Code:**
    * **Validity:** Short-lived (expires in 5 minutes).
    * **Use Case:** Initial setup of a new provider node.
    * **Behavior:** Single-use; each device needs a fresh code.
* **Client JWT Token:**
    * **Validity:** Persistent (Static).
    * **Use Case:** For Docker deployments or running multiple instances.
    * **Behavior:** Does not expire every 5 minutes. Useful for stable, long-term deployments.

### SSO & Login Limitations
If you signed up using **Google** or **Solana Wallet SSO**, you cannot use a traditional username/password for CLI or Docker logins.
* ✅ Use **Auth Codes** or **JWT Tokens** instead.
* You can generate these by logging into the web dashboard at `app.ur.network`.

### How to Authenticate
Run the following command to update or set your credentials:
```bash
urnetwork auth
```
You will be prompted to enter your Auth Code, which the binary will then exchange for a session token (JWT).

### Error: "User auth attempts exceeded limits"
This indicates you are being rate-limited. This usually happens when attempting to authenticate multiple devices in very rapid succession.
* **Fix:** Wait 15–20 minutes. Authenticate your devices one at a time with a short delay between them.

> **Danger:** Never share your JWT token or Auth Codes publicly. They provide access to your account and earnings.