# Bandwidth Limits & Quota Logic

The URnetwork bandwidth system operates on a **rolling daily allowance** model. Rather than a fixed monthly cap, the network provides "vouchers" that overlap to ensure a smooth user experience.

### 1. The Daily Allowances
Usage limits are defined by your account tier. Every 24 hours (at midnight UTC), the system adds a new data voucher to your account:

| Tier | Refresh Amount | Total Monthly (Approx) | Max Dashboard Display |
| :--- | :--- | :--- | :--- |
| **Free** | **34 GiB** | ~1 TiB | **68 GiB** |
| **Supporter** | **600 GiB** | ~18 TiB | **1200 GiB (1.17 TiB)** |

### 2. The 30-Hour Rolling Window
This is the "secret sauce" behind the dashboard numbers. 
* **Refresh Frequency:** Every 24 hours.
* **Voucher Lifespan:** 30 hours.

Because vouchers last 6 hours longer than the refresh cycle, there is a **6-hour daily overlap** where two vouchers are active simultaneously.

**The Supporter Math Example:**
* **12:00 AM:** You receive a **600 GiB** voucher.
* **Next 12:00 AM:** You receive a *second* **600 GiB** voucher.
* **12:00 AM – 6:00 AM:** Both vouchers are active. The dashboard sums them to **1200 GiB** (which converts to **1.17 TiB**).
* **6:01 AM:** The first voucher expires, and the display drops back to **600 GiB**.

### 3. Usage for Providers
Providers and high-volume contributors often see "virtually unlimited" usage. This happens via two primary paths:
1. **Status Persistence:** High contributors may be manually or automatically flagged with "Supporter" status, putting them on the 18 TiB/month track.
2. **Balance Codes:** The network can issue specialized balance codes that stack on top of the daily refresh for specific community rewards.

### Technical Reference (Source Code)
The logic is handled in the `urnetwork/server` repository:
* **Tier Constants:** `server/controller/subscription_controller.go` (Lines 40-41)
* **Overlap Logic:** `server/controller/subscription_controller.go` (Line 34)
* **Summing Logic:** `SubscriptionBalance` function in `server/controller/subscription_controller.go`.

---
*Last Updated: June 2026*
