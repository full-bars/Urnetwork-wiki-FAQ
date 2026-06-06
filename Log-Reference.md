# Provider Log Reference

A plain-language guide to the log lines you'll see running a stock URnetwork provider. Examples are representative of real output.

---

### Startup Confirmation

```
Provider <version> started
client_id: 019e2d67-5a52-b4f0-a00f-0bb97281dfe0
instance_id: 019e2d67-5a73-4bb3-6661-df9b5c595003
```

| Field | Meaning |
|---|---|
| `Provider <version>` | The git commit hash or version tag the binary was built from |
| `client_id` | The provider's permanent identity on the platform |
| `instance_id` | Unique ID for this specific run; changes on restart |

---

### Connection Selection (`[net][s]select`)

```
[net][s]select: proxy[42] (1.2.3.4:1081) [fragment] success=6086 error=192 clients=12
[net][s]select: proxy[13] (5.6.7.8:1081) [normal] success=2221 error=223 clients=5
```

Logged at V(2) in stock (hidden at default verbosity; visible with `-vv`). Shows the provider selecting a route for a client session. Typically won't be visible unless you enable verbose logging.

**What to watch for:**
- `clients=N` shows the number of active multiplexed sessions on that route.
- A healthy error rate is under ~10% of successes. Higher suggests that route is having connectivity problems.

---

### Transport Auth Error (`[t]auth error`)

```
[t]auth error 019e2d83-3118-5186-995f-aabe3b2dcf0b = Timeout. (34 suppressed)
```

The provider failed to authenticate a transport connection to the platform. Each transport ID represents one connection attempt.

- Rate-limited to **1 log per minute** globally across all transports.
- `(34 suppressed)` tells you how many additional transports also failed since the last visible line.
- The error is usually `Timeout.` — the platform didn't respond in time.
- Occasional occurrences are normal. Continuous appearances for many minutes indicate a platform outage.

---

### Transport Stream Errors (`[ts]`)

```
[ts]019e28a3-...-> error = write tcp 172.17.0.2:58902->216.26.233.197:1081: i/o timeout
```

A TCP write to a destination timed out at the transport stream layer. Appears when network conditions are degraded (high latency, packet loss).

- Usually followed by a `[t]auth error` for the same transport ID.
- Common during transient network degradation.

---

### Drop Rate-Limiting (`[r]drop`)

```
[r]drop = write error: connection reset by peer (1,420 suppressed)
```

The provider dropped a packet because it couldn't be delivered to the final destination.

- **Rate-limited to 1 per minute** globally to prevent log flooding.
- The `(N suppressed)` count shows how many other drops occurred since the last line.
- High drop counts are normal during global outages or when a specific destination is unreachable.

---

### Contract Creation (`[s] exit could not create contract`)

```
[s]019e0f4d-...->[]...019e2f50-... s(00000000-0000-0000-0000-000000000000) exit could not create contract.
```

A session between two clients failed because no contract could be allocated.

- `s(00000000-...)` means no contract was ever assigned.
- Fires when the platform can't issue contracts (backend degraded, rate-limited, etc.).
- The session will retry automatically.

---

### Contract OOB Backoff (`[contract]oob err`)

```
[contract]oob err = Timeout.; backing off create contract OOB requests for 1m0s
```

The provider tried to request a contract via the out-of-band (OOB) control channel and got a timeout. It will stop sending OOB requests for 60 seconds before retrying.

- Rate-limited to at most 1 per minute.
- Sustained appearances over many minutes = platform OOB service degraded.
- Does not affect already-established sessions, only new contract negotiations.

---

### Debit Contract Near Capacity

```
[contract]debit contract ... failed +1420->13750 (12330/13107 total 94.1% full)
```

A contract is filling up and a new one will be negotiated.

- This line being present means **data is actually flowing** through the provider.
- When a contract fills up, a new one is negotiated automatically.

---

### TLS Session Events (`[tls]`)

```
[tls]... handshake complete
[tls]... handshake error = ...
[tls]... peer identity proof verified — cipher is now usable
```

Events from the per-peer TLS encryption system (`transfer_encrypt.go`). Visible at V(1) and above.

- `handshake complete` — a per-peer TLS session was established successfully.
- `peer identity proof verified` — the post-handshake identity exchange confirmed the peer's public key.
- These are informational; encryption is transparent to the operator once configured.

---

### Message Pool Health (`[mp]`)

```
pool[2048] tag=0 [] r=1616413/t=1617695/c=20087 = 99.92% return / 98.76% reuse
```

Fires every 60 seconds. Shows the internal buffer pool health.

| Field | Meaning |
|---|---|
| `pool[2048]` | Buffer size in bytes |
| `r=` / `t=` | Returned / Taken count (lifetime) |
| `c=` | Created count — allocations when pool was empty |
| `return %` | `r/t` — should be ~100%. A leak shows here. |
| `reuse %` | `(t-c)/t` — fraction of checkouts that reused an existing buffer |

**What to watch for:**
- `return %` dropping below 99% — buffers are being leaked.
- `reuse %` below 95% — the pool is undersized; GC pressure is higher than ideal.
- A flat `r=` counter that doesn't grow between minutes means no sessions are active.

---

### Verbosity Levels

The `-v` flag controls log verbosity:

| Flag | Level | What you see |
|---|---|---|
| *(none)* | 0 | `[contract]`, `[t]auth error`, `[r]drop` (rate-limited), errors |
| `-v` | 1 | Contract lifecycle, TLS session events, ack tracking |
| `-vv` | 2 | `[net][s]select`, per-packet events, full transport tracing |

---

*Note: This reference covers the stock URnetwork provider. Custom forks may add additional log patterns.*
