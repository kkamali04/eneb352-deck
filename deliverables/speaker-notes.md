# ENEB352 — How CDNs Make the Web Fast
## Speaker Notes & Presentation Script
### Kian Kamali · Introduction to Networks and Protocols · Spring 2026

---

## Slide 1: Title Slide (30 seconds)

**Visual:** Title text on left, breathing globe with PoP dots and arcs on right, particle background.

**Say:**
"Hello, I'm Kian Kamali. Today I'll explain how Content Delivery Networks, or CDNs, make the web fast. This isn't just about caching files. It's about rethinking how the entire internet routes traffic, handles protocol negotiation, and delivers content to users across the globe."

**Key Points:**
- CDN stands for Content Delivery Network
- Topic assigned: How CDNs Make the Web Fast
- 18 slides covering protocols, architecture, real systems, and improvements

---

## Slide 2: The Problem (1 minute)

**Visual:** 200ms vs 20ms comparison, distance illustration.

**Say:**
"The internet wasn't built for distance. When a user in Tokyo requests a website hosted in Virginia, that packet travels approximately 11,000 kilometers through fiber optic cables. At two-thirds the speed of light, that's a 70-millisecond one-way trip, 140 milliseconds round-trip. Add TCP three-way handshake at 1 RTT, TLS negotiation at 1 to 2 RTTs, server processing, and database queries, and you're looking at 300 to 500 milliseconds before the browser even receives the first byte. On mobile 3G networks, this easily exceeds one second. Research shows that every 100 milliseconds of delay reduces conversion rates by 1 percent. For Amazon, that's billions in lost revenue."

**Key Points:**
- Physical distance creates inherent latency
- TCP handshake + TLS = multiple RTTs
- Mobile networks amplify the problem
- Latency directly impacts business metrics

---

## Slide 3: Protocols Overview (1 minute)

**Visual:** Protocol badges, two-column layout showing layer 3-4 and layer 5-7 protocols.

**Say:**
"CDNs don't work in isolation. They orchestrate a stack of protocols across multiple OSI layers. At the network and transport layers, we have IP Anycast, BGP for routing, TCP with BBR congestion control, and UDP for QUIC. At the application layer, DNS resolves names to IPs, HTTP delivers content, and TLS encrypts everything. Each protocol serves a specific purpose, and understanding how they interact is critical to understanding CDN performance."

**Key Points:**
- Layer 3: IP Anycast, BGP
- Layer 4: TCP, UDP
- Layer 5-7: DNS, HTTP/1.1, HTTP/2, HTTP/3, TLS 1.3
- Protocols work together, not in isolation

---

## Slide 4: DNS and Anycast Routing (1.5 minutes)

**Visual:** DNS resolution flow, terminal showing dig command output.

**Say:**
"The CDN journey starts with DNS. When you type example.com into your browser, the resolver queries the CDN's authoritative DNS server. Here's where it gets interesting. The CDN doesn't return a single IP address. Instead, it uses GeoDNS or Anycast routing to direct you to the nearest Point of Presence, or PoP. With Anycast, hundreds of servers worldwide share the same IP address. BGP routing protocols then direct your packet to the geographically closest server based on network topology. If a PoP goes offline, BGP automatically reroutes traffic to the next nearest location within seconds. Cloudflare operates over 300 PoPs globally. Akamai has over 4,000 edge locations. This density is what makes sub-20-millisecond response times possible."

**Key Points:**
- GeoDNS returns different IPs based on user location
- Anycast uses shared IPs across all PoPs
- BGP handles automatic failover
- Major CDNs: Cloudflare 300+ PoPs, Akamai 4,000+ edges

---

## Slide 5: HTTP Evolution (1.5 minutes)

**Visual:** Comparison table: HTTP/1.1 vs HTTP/2 vs HTTP/3.

**Say:**
"HTTP has evolved dramatically. HTTP 1.1, released in 1997, allows only six concurrent connections per domain and suffers from head-of-line blocking. One slow resource blocks everything behind it. HTTP/2, standardized in 2015, introduced multiplexing, allowing multiple requests over a single connection. But it still runs over TCP, so a single lost packet blocks all streams. HTTP/3, published as RFC 9114 in 2022, solves this by running over QUIC, which uses UDP instead of TCP. QUIC provides independent streams, so packet loss in one stream doesn't affect others. It also supports zero-RTT connection resumption, meaning repeat visitors can send data immediately without any handshake. On lossy mobile networks, HTTP/3 loads pages 30 to 50 percent faster than HTTP/2."

**Key Points:**
- HTTP/1.1: 6 conn limit, head-of-line blocking
- HTTP/2: Multiplexing but still TCP-blocked
- HTTP/3: QUIC over UDP, independent streams, 0-RTT
- 30-50% faster on mobile/lossy networks

---

## Slide 6: TLS 1.3 (1 minute)

**Visual:** TLS handshake flow diagram, terminal showing the exchange.

**Say:**
"Security adds latency. TLS 1.2 requires two full round trips for the handshake. TLS 1.3, defined in RFC 8446, reduces this to one RTT for new connections. For returning visitors, it supports zero-RTT mode, where the client sends encrypted data in the very first packet. This works because the client and server have previously negotiated session keys. The server recognizes the session and can decrypt immediately. For a cached resource at the edge, this means the user gets content effectively instantly. The trade-off is that zero-RTT lacks forward secrecy for the first packet, but for idempotent GET requests, this risk is minimal."

**Key Points:**
- TLS 1.2: 2 RTT handshake
- TLS 1.3: 1 RTT new, 0 RTT resumed
- Session resumption keys enable 0-RTT
- Trade-off: First packet lacks forward secrecy

---

## Slide 7: Cache Hierarchy (1 minute)

**Visual:** Three-tier diagram: Edge -> Regional -> Origin.

**Say:**
"CDNs use a three-tier cache hierarchy. The edge layer sits closest to users, typically within the same metropolitan area. It serves 80 to 95 percent of all requests and has the shortest TTL, usually minutes to hours. The regional layer, sometimes called a shield, sits behind the edge and handles cache misses from multiple edge nodes. It reduces origin load by collapsing identical requests. The origin is your actual server. In a well-tuned CDN, the origin sees only 5 to 20 percent of total traffic. This tiered design means your infrastructure can handle 10 times the traffic without scaling."

**Key Points:**
- Edge: 80-95% of traffic, minutes-hours TTL
- Regional: Collapses misses, protects origin
- Origin: Only 5-20% of traffic
- 10x traffic capacity without scaling

---

## Slide 8: Cache Headers (1 minute)

**Visual:** Code block showing Cache-Control headers, explanation cards.

**Say:**
"Cache behavior is controlled entirely by HTTP headers sent from the origin. The Cache-Control header is the primary mechanism. Max-age tells the CDN how many seconds to cache the resource. S-maxage overrides max-age for shared caches like CDNs, while max-age still controls the browser cache. This lets you cache content at the edge for one hour while telling the browser to cache it for 24 hours. Stale-while-revalidate is a powerful but underused directive. It tells the CDN to serve stale content while fetching fresh data in the background. Users never wait for revalidation. For static assets like JavaScript and CSS, use versioned filenames with one-year TTLs. For HTML, use short TTLs with stale-while-revalidate."

**Key Points:**
- Cache-Control: max-age, s-maxage, stale-while-revalidate
- ETag and Last-Modified enable conditional requests
- Static assets: versioned URLs, 1-year TTL
- HTML: short TTL with stale-while-revalidate

---

## Slide 9: Cache Invalidation (1 minute)

**Visual:** Three methods illustrated: Purge API, Cache Busting, Surrogate Keys.

**Say:**
"Cache invalidation is famously one of the two hard problems in computer science, alongside naming things and off-by-one errors. When you update content, stale versions may persist at edge locations until TTL expires. There are three primary strategies. First, the Purge API: CDN providers expose APIs to invalidate specific URLs instantly. Cloudflare propagates purges across all PoPs in 1 to 5 seconds. Second, cache busting: append content hashes to filenames, like app-dot-abc123-dot-js. When the content changes, the hash changes, and the URL changes, making old cache entries irrelevant. Third, surrogate keys: tag related content with keys, like product-123, then purge by key to invalidate thousands of URLs simultaneously. Fastly pioneered this approach."

**Key Points:**
- Purge API: Instant invalidation, 1-5s propagation
- Cache busting: Versioned filenames
- Surrogate keys: Tag and purge groups of URLs
- Choose strategy based on content type and update frequency

---

## Slide 10: Case Study - Cloudflare (1 minute)

**Visual:** Cloudflare stats, feature cards.

**Say:**
"Cloudflare, founded in 2009, now operates over 300 Points of Presence across 100 countries, processing 55 million HTTP requests per second at peak. They handle approximately 25 percent of all web traffic. Their key innovation is Cloudflare Workers: V8 JavaScript isolates that run at every edge location. You deploy code globally in under 30 seconds with zero cold starts. This isn't just caching anymore; it's a distributed application platform. Their R2 object storage competes with Amazon S3 at a fraction of the cost, with zero egress fees. And their network has mitigated the largest DDoS attack ever recorded: 71 million requests per second in February 2023."

**Key Points:**
- 300+ PoPs, 55M req/s, 25% of web
- Workers: Edge compute platform
- R2: Zero-egress object storage
- DDoS mitigation at unprecedented scale

---

## Slide 11: Case Study - Akamai and Fastly (1 minute)

**Visual:** Four-column comparison table.

**Say:**
"Cloudflare isn't the only player. Akamai, the oldest CDN founded in 1998, operates over 4,000 edge locations and dominates enterprise media delivery. They invented the CDN concept at MIT. Fastly, founded in 2011, differentiates with instant configuration propagation: changes deploy globally in 150 milliseconds. Their edge dictionaries enable real-time A/B testing at the edge. Amazon CloudFront, launched in 2008, integrates deeply with the AWS ecosystem. Netflix uses CloudFront for their control plane, though they built their own CDN, Open Connect, for video delivery. Each provider has strengths: Akamai for media, Fastly for real-time config, CloudFront for AWS integration, and Cloudflare for developer experience and pricing."

**Key Points:**
- Akamai: 4,000+ edges, enterprise/media focus
- Fastly: 150ms config propagation, edge dictionaries
- CloudFront: Deep AWS integration
- Netflix uses multiple CDNs including their own Open Connect

---

## Slide 12: Trade-offs - Latency vs Cost (1 minute)

**Visual:** Two-column cost/benefit analysis.

**Say:**
"CDNs aren't free, and they're not always the right choice. The primary trade-off is latency versus cost. A CDN can reduce perceived latency by 30 percent for geographically distributed users, but you pay for every gigabyte egressed from the edge. At 90 percent cache hit ratio, you save approximately 80 dollars per terabyte compared to serving from origin. But at 50 percent hit ratio, the savings diminish significantly. There's also operational complexity: managing cache invalidation logic, debugging distributed state across hundreds of PoPs, and dealing with vendor-specific APIs. For a small local business with users in one city, a CDN might add more complexity than value."

**Key Points:**
- 30% latency reduction possible
- Cost scales with egress bandwidth
- 90% cache hit ratio = $80/TB savings
- Not needed for geographically concentrated audiences

---

## Slide 13: Trade-offs - Complexity (1 minute)

**Visual:** Three cards: Thundering Herd, SSL Termination, Vendor Lock-in.

**Say:**
"Beyond cost, there are three major complexity concerns. First, the thundering herd problem: when cache expires, thousands of simultaneous requests hit the origin before the edge can re-cache. Origin shield and stale-while-revalidate mitigate this. Second, SSL termination: CDNs decrypt TLS at the edge, which means your data travels in plaintext inside the CDN's network. You're trusting the CDN as a third party. Third, vendor lock-in: edge compute platforms like Workers and VCL are proprietary. Moving to another provider requires rewriting your edge logic. Multi-CDN strategies reduce this risk but multiply operational complexity."

**Key Points:**
- Thundering herd: Mitigate with origin shield
- SSL termination: Trust model changes
- Vendor lock-in: Proprietary edge compute
- Multi-CDN: Reduces risk, increases complexity

---

## Slide 14: Improvement 1 - Edge Compute (1 minute)

**Visual:** Edge-first architecture diagram.

**Say:**
"The most exciting improvement in CDN technology is edge compute. Instead of just caching static files, you can run actual application logic at the edge. Cloudflare Workers, AWS Lambda at Edge, and Fastly Compute all enable this. The traditional model routes every dynamic request back to the origin. With edge compute, A/B testing, personalization, API aggregation, and even database queries happen within 5 milliseconds of the user. Cloudflare's D1 database and KV store are specifically designed for edge access patterns. This fundamentally changes the architecture from origin-centric to edge-first."

**Key Points:**
- Workers, Lambda@Edge, Fastly Compute
- A/B testing and personalization at edge
- Edge databases: D1, KV, FaunaDB
- Architecture shift: origin-centric to edge-first

---

## Slide 15: Improvement 2 - HTTP/3 Adoption (45 seconds)

**Visual:** HTTP/3 stats, performance comparison.

**Say:**
"As of 2026, approximately 35 percent of web traffic uses HTTP/3. The holdouts are enterprise firewalls that block UDP and legacy load balancers that don't support QUIC. HTTP/3 shines brightest on mobile networks, where packet loss of 1 to 3 percent is common. On a 3 percent loss connection, HTTP/3 loads pages twice as fast as HTTP/2. For CDN providers, enabling HTTP/3 is typically a configuration toggle. For website operators, the performance gains are essentially free. My recommendation: enable HTTP/3 on your CDN immediately if you haven't already."

**Key Points:**
- 35% adoption as of 2026
- Blockers: Enterprise firewalls, legacy load balancers
- 2x faster on 3% packet loss networks
- Usually a one-click enable in CDN dashboard

---

## Slide 16: Improvement 3 - Smarter Caching (45 seconds)

**Visual:** Three cards: ML Prefetch, Tiered TTL, Real-time Purge.

**Say:**
"The third major improvement is intelligent caching. Machine learning can predict what content users will request next and prefetch it to the edge before they ask. Netflix uses this for thumbnail images. Tiered TTL automatically adjusts cache duration based on access patterns: popular content gets shorter TTLs for freshness, while cold content gets longer TTLs for efficiency. Real-time purge using WebSockets pushes invalidation events to all edge nodes instantly, eliminating the 1 to 5 second propagation delay of traditional purge APIs. The next frontier is predictive content distribution that anticipates demand before it happens."

**Key Points:**
- ML prefetch: Predict and pre-cache content
- Tiered TTL: Dynamic based on access patterns
- Real-time purge: WebSocket-based instant invalidation
- Future: Predictive content distribution

---

## Slide 17: Summary (30 seconds)

**Visual:** Three-column recap: Problem, Solution, Trade-offs.

**Say:**
"To summarize: the problem is physical distance creating inherent latency. The solution is CDNs, which use Anycast routing, tiered caching, and modern protocols to reduce round-trip times from 200 milliseconds to 20 milliseconds. The trade-offs are cost, cache invalidation complexity, SSL trust model changes, and vendor lock-in. CDNs have evolved from simple file caches to distributed application platforms, and they're now essential infrastructure for any web application serving a global audience."

---

## Slide 18: Q&A (15 seconds)

**Visual:** Thank you slide with contact info.

**Say:**
"Thank you. I'm happy to answer any questions about CDN architecture, protocol details, or specific provider implementations."

---

## Slide-by-Slide Questions

### Slide 1
- Q: What is a CDN?
  - A: A CDN is a network of edge servers that deliver content from locations closer to users.
  - Why it matters: the whole talk is about reducing distance, not just storing files.

### Slide 2
- Q: Why does distance matter so much?
  - A: Every extra round trip adds delay before the page can even start loading.
  - Why it matters: TCP and TLS stack latency before content appears.

### Slide 3
- Q: Why do protocols matter if caching already helps?
  - A: CDNs improve routing, transport, encryption, and delivery together.
  - Why it matters: performance comes from the full stack, not one trick.

### Slide 4
- Q: How does Anycast pick the nearest server?
  - A: Multiple PoPs share one IP, and BGP routes traffic to the closest healthy path.
  - Why it matters: this is how CDNs get users to an edge quickly.

### Slide 5
- Q: Why is HTTP/3 better on bad networks?
  - A: QUIC keeps streams independent, so packet loss does not stall everything.
  - Why it matters: mobile users feel the difference the most.

### Slide 6
- Q: What does TLS 1.3 save?
  - A: It reduces handshake round trips and makes repeat visits faster.
  - Why it matters: security no longer has to cost as much latency.

### Slide 7
- Q: Why use edge, regional, and origin layers?
  - A: The layers absorb traffic step by step so the origin stays protected.
  - Why it matters: this is how CDNs scale without overload.

### Slide 8
- Q: Why not cache everything forever?
  - A: Content changes, so cache headers control freshness and user experience.
  - Why it matters: fast delivery only works if stale data does not linger.

### Slide 9
- Q: Why is invalidation hard?
  - A: Because one change can affect many cached copies across many PoPs.
  - Why it matters: the harder the cache is to control, the more important purge strategy becomes.

### Slide 10
- Q: What makes Cloudflare a good case study?
  - A: It combines CDN delivery, edge compute, storage, and security at scale.
  - Why it matters: it shows how CDNs became platforms.

### Slide 11
- Q: Why compare Akamai, Fastly, and CloudFront?
  - A: Each provider emphasizes different strengths, so there is no one best CDN.
  - Why it matters: the right choice depends on workload and ecosystem.

### Slide 12
- Q: What is the biggest trade-off with CDNs?
  - A: You trade lower latency for added cost and more operational complexity.
  - Why it matters: performance wins have to justify the overhead.

### Slide 13
- Q: What is the main risk of edge caching and edge compute?
  - A: Staleness, security trust, and vendor lock-in all increase with distribution.
  - Why it matters: not every improvement is free.

### Slide 14
- Q: Why move logic to the edge?
  - A: So simple dynamic tasks happen near users instead of going back to origin.
  - Why it matters: this is where CDNs become application platforms.

### Slide 15
- Q: Should everyone enable HTTP/3?
  - A: Yes, if the CDN and client support it, because the gain is usually easy to get.
  - Why it matters: it is one of the simplest performance upgrades.

### Slide 16
- Q: What does smarter caching mean?
  - A: Predicting demand, tuning TTLs, and invalidating in real time instead of waiting.
  - Why it matters: the cache becomes adaptive instead of static.

### Slide 17
- Q: Why is the reference slide important?
  - A: It shows the talk is grounded in standards and current sources.
  - Why it matters: it makes the presentation defensible if the professor asks where a claim came from.

### Slide 18
- Q: What is the one sentence takeaway?
  - A: CDNs make the web fast by shortening distance, reducing round trips, and moving work closer to users.
  - Why it matters: that is the whole argument of the presentation.

## Final Questions to Practice

- Q: What is a CDN, in plain English?
  - A: A CDN is a distributed set of servers that gets content closer to the user.
  - Explanation: shorter distance means lower latency and better load times.

- Q: Why can’t we just use one bigger server?
  - A: Because physical distance and regional congestion still create delay.
  - Explanation: scale helps capacity, but not geography.

- Q: What is Anycast?
  - A: Many servers advertise the same IP address, and routing sends users to the nearest healthy one.
  - Explanation: this is one of the core tricks behind CDN speed.

- Q: What problem does HTTP/3 solve?
  - A: It reduces head-of-line blocking by using QUIC over UDP with independent streams.
  - Explanation: packet loss hurts less, especially on mobile networks.

- Q: What is the hardest CDN problem?
  - A: Cache invalidation.
  - Explanation: getting fresh content everywhere quickly is harder than serving cached content.

- Q: Why are edge compute platforms important?
  - A: They let you run logic near users instead of sending every request back to origin.
  - Explanation: that cuts latency and changes the architecture entirely.


---

## Timing Summary

| Section | Time |
|---------|------|
| Slides 1-2: Intro + Problem | 1.5 min |
| Slides 3-6: Protocols | 5 min |
| Slides 7-9: Caching | 3 min |
| Slides 10-11: Case Studies | 2 min |
| Slides 12-13: Trade-offs | 2 min |
| Slides 14-16: Improvements | 2.5 min |
| Slides 17-18: Summary + Q&A | 1 min |
| **Total** | **~15 min** |

---

## References (APA Format)

Cloudflare. (2024). *How does a CDN work?* Cloudflare Learning Center. https://www.cloudflare.com/learning/cdn/what-is-a-cdn/

Fielding, R., & Reschke, J. (2014). *Hypertext Transfer Protocol (HTTP/1.1): Conditional Requests* (RFC 7232). IETF. https://tools.ietf.org/html/rfc7232

Kurose, J. F., & Ross, K. W. (2021). *Computer networking: A top-down approach* (8th ed.). Pearson.

Mozilla. (2024). *HTTP caching*. MDN Web Docs. https://developer.mozilla.org/en-US/docs/Web/HTTP/Caching

Postel, J. (1981). *Transmission Control Protocol* (RFC 793). IETF. https://tools.ietf.org/html/rfc793

Rescorla, E. (2018). *The Transport Layer Security (TLS) Protocol Version 1.3* (RFC 8446). IETF. https://tools.ietf.org/html/rfc8446

Tech Interview. (2024). *CDN architecture: Edge caching, PoP, Anycast, cache invalidation*. https://www.techinterview.org/post/3233474198/

Geek Workbench. (2024). *CDN deep dive: Content delivery networks and edge computing*. https://geekworkbench.com/blog/technical/cdn-deep-dive/

---

## Technical Report Outline

### Section 1: Introduction (0.5 pages)
- What is a CDN?
- Why do we need them?
- Brief history

### Section 2: The Problem (0.5 pages)
- Physical distance and latency
- TCP/RTT fundamentals
- Impact on user experience

### Section 3: Protocols (1 page)
- DNS and Anycast
- HTTP/1.1, HTTP/2, HTTP/3
- TLS 1.3 and 0-RTT
- QUIC over UDP

### Section 4: Architecture (1 page)
- Three-tier cache hierarchy
- Cache headers and control
- Invalidation strategies

### Section 5: Real Systems (0.5 pages)
- Cloudflare, Akamai, Fastly comparison
- Performance metrics

### Section 6: Trade-offs (0.5 pages)
- Latency vs cost
- Complexity concerns
- When not to use a CDN

### Section 7: Future Improvements (0.5 pages)
- Edge compute
- HTTP/3 adoption
- ML-driven caching

### Section 8: Conclusion (0.25 pages)
- Summary of findings
- Personal assessment

**Total: 3-5 pages**
