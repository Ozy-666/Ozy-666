# Systems & Network Architect // Ozy-666

TSI Alumnus (Class of '05, IT). Designing and maintaining highly available, high-concurrency edge infrastructure. Specialized in zero-allocation Go network daemons, high-QPS Linux kernel tuning (XDP, nftables), and radical codebase stripping to eliminate runtime overhead and security attack surface.

---

#### 🛡️ Active R&D: High-Throughput L4 / L7 DDoS Mitigation
*   **Layer 4 Kernel-Space (eBPF/XDP):** Developing ultra-low-latency packet-filtering engines in Go that leverage **eBPF/XDP** to drop volumetric garbage (SYN floods, UDP amplification) directly at the NIC driver level—long before the Linux kernel allocates `sk_buff` structures.
*   **Layer 7 Application-Space (Go/Nginx):** Crafting high-concurrency user-space HTTP/DoH sanitization engines designed to rate-limit and filter malicious application-layer requests using zero-allocation byte parsers.
*   **Dynamic nftables Orchestration:** Direct integration of the Go control plane with Linux **nftables** via Netlink sockets, dynamically pushing real-time blacklists directly to kernel-space sets to isolate malicious actors at scale with minimal CPU overhead.

#### 🤖 AI-Collaborative Systems Development
* Advanced static analysis, performance profiling, and runtime architecture refactoring executed in seamless integration with **Claude Code** and **Gemini API** / AI Studio CLI tooling.

#### ⚙️ Main Production Showcase: [DNSDOH.ART](https://dnsdoh.art)
* A globally distributed, independent encrypted DNS infrastructure supporting high-performance anycast routing over DoH3 (QUIC), DoH, DoQ, and DoT.

#### 🧰 Custom Hardened Go Stack
* **[AdGuardHome-edge-spec](https://github.com/Ozy-666/AdGuardHome-edge-spec)** — blueprint and modifications for an elite, stripped-down edge resolver (--13k LOC).
* **[dnscrypt-proxy](https://github.com/Ozy-666/dnscrypt-proxy)** — custom fork with `sync.Pool` packet buffers (0 B/op on hot paths) and compiled-out monitoring.
* **[urlfilter](https://github.com/Ozy-666/urlfilter)** — high-frequency rule matching engine patched with AST-based regex shortcut extraction (O(1) miss paths).
* **[dns-ultra](https://github.com/Ozy-666/dns-ultra)** — a high-precision DNSCrypt/DoH benchmarking and auto-tuning suite.

---
*QA & Process Inspection: Maine Coon Michelle 🐾. Strict control over system uptime, stability, and runtime allocation constraints.*
