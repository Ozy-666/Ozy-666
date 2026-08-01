# Ozy-666 — Systems & Network Engineer

TSI alumnus (IT, Class of '05). I build and run high-availability, high-concurrency edge infrastructure — zero-allocation Go network daemons, Linux kernel tuning (XDP, nftables), and aggressive codebase stripping to cut runtime overhead and attack surface.

My main project is **[DNSDOH.ART](https://dnsdoh.art/)** — a public encrypted-DNS resolver I build and run as a hobby. No logs, no telemetry, no company behind it. → [ozy-666.github.io](https://ozy-666.github.io/)

---

#### 🛡️ Active R&D — DDoS mitigation (L4 / L7)
The goal behind all of it: a single node that simply does not fall over.
* **L4, kernel-space (eBPF/XDP):** packet-filtering engines in Go that drop volumetric garbage (SYN floods, UDP amplification) at the NIC driver level — before the kernel ever allocates an `sk_buff`.
* **L7, user-space (Go / nginx):** zero-allocation byte parsers that rate-limit and sanitize malicious DoH / HTTP requests.
* **Dynamic nftables orchestration:** a Go control plane talking to **nftables** over Netlink sockets, pushing real-time blocklists straight into kernel-space sets to isolate abusers with minimal CPU cost.

#### ⚙️ The resolver behind it — [DNSDOH.ART](https://dnsdoh.art/)
A single, carefully-tuned server — **not a global anycast network** — running encrypted DNS over DoH, DoH3 (QUIC), DoQ and DoT, with ad/tracker blocking and a strict no-logs policy. The focus is reliability and zero tolerance for DDoS and abuse, not out-scaling the big providers.

#### 🧰 The hardened Go stack
* **[AdGuardHome-edge-spec](https://github.com/Ozy-666/AdGuardHome-edge-spec)** — the engine: a stripped-down AdGuard Home fork (~13k LOC removed, zero-allocation hot paths, Unbound on BoringSSL).
* **[dnsproxy](https://github.com/Ozy-666/dnsproxy)** — transport fork: pooled connections, `SO_REUSEPORT` listener sharding, lock-free upstream RTT map.
* **[dnscrypt-proxy](https://github.com/Ozy-666/dnscrypt-proxy)** — encrypted-upstream fork: `sync.Pool` packet buffers (0 B/op on hot paths), monitoring compiled out, security-audited.
* **[urlfilter](https://github.com/Ozy-666/urlfilter)** — filtering engine: AST-based required-literal extraction (O(1) regex miss paths).
* **[dns-ultra](https://github.com/Ozy-666/dns-ultra)** — upstream resolver profiler: separates warm-cache from uncached lookups so a slow authoritative walk can't misrank a good anycast resolver, and prints a ready-to-paste config.

#### 🧱 The C edge — build tooling, not forks
The TLS and DNS daemons themselves are stock. Every customisation lives in build flags, configuration and systemd units, and both scripts fetch the official upstream release at build time — so there is no source tree to re-sync and nothing to merge on each release.
* **[nginx-edge](https://github.com/Ozy-666/nginx-edge)** — BoringSSL-linked nginx: HTTP/3, post-quantum key exchange, Zen 2 tuning, plus annotated TLS-hardening and L7 anti-DDoS example configs. Carries one patch: server-side Encrypted Client Hello, which nginx implements only against the OpenSSL 3.x API and not BoringSSL.
* **[unbound-edge](https://github.com/Ozy-666/unbound-edge)** — BoringSSL-linked, Zen 2-optimised Unbound. BoringSSL was chosen on measured DNSSEC throughput, not assumed parity, and the repo ships the benchmark that re-proves it after every library bump.

#### 🤖 How I work
Profile-driven: every change is proven with `pprof` / `benchstat` on real hardware before it ships, and the dead-ends get documented alongside the wins. Static analysis, profiling and refactoring done with AI tooling (**Claude Code**, **Gemini** / AI Studio CLI) in the loop.

---
*Career note: I left IT around 2012 and came back to it in late 2025. This is evenings-and-weekends work, not a day job — which is also why there's no company behind DNSDOH.ART and never will be.*

*QA &amp; uptime inspection: Maine Coon Michelle 🐾 — sits on the keyboard during deploys. Zero incidents attributable to her so far.*
