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
* **[dns-ultra](https://github.com/Ozy-666/dns-ultra)** — a DNSCrypt / DoH benchmarking and auto-tuning suite.

#### 🤖 How I work
Profile-driven: every change is proven with `pprof` / `benchstat` on real hardware before it ships, and the dead-ends get documented alongside the wins. Static analysis, profiling and refactoring done with AI tooling (**Claude Code**, **Gemini** / AI Studio CLI) in the loop.

---
*QA &amp; uptime inspection: Maine Coon Michelle 🐾 — strict about stability and allocation budgets.*
