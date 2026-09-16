# hosted dedicated servers: what they are, when you need one, and how DMIT's bare metal fits in

If you've been shopping around for hosting, you've probably hit the wall where shared hosting, VPS, and even "cloud" plans stop feeling like enough. Database queries queue up, a noisy neighbor eats your CPU, and the fine print on "unlimited bandwidth" turns out to have a very definite limit. That's usually the moment people start searching for hosted dedicated servers.

The term gets thrown around loosely, so let's get the basics straight before talking about any specific provider.

## What "hosted dedicated servers" actually means

A hosted dedicated server is a physical machine sitting in a data center that you rent in its entirety. Nobody else's workloads run on it. You get the full CPU, the full RAM, the full disks, and the full network port — not a slice, not a virtualized share, not a "fair use" portion of someone else's hardware.

That's the core distinction from a VPS or cloud instance. A VPS is a virtual machine carved out of a larger host using KVM, VMware, or similar hypervisor. Even when a provider sells "dedicated vCores," you're still sharing the memory bus, the NIC, and the physical motherboard with other tenants. A true dedicated server removes that layer entirely.

People search for hosted dedicated servers for fairly predictable reasons:

- A database that's bottlenecking on CPU steal or memory pressure under a VPS
- A game server or real-time application that needs consistent, low-latency single-thread performance with no contention
- Compliance or data-isolation requirements where sharing a hypervisor host isn't acceptable
- High-throughput workloads — CDN origin nodes, media processing, large file mirrors — where you want the whole 10Gbps port to yourself
- Custom hardware needs: specific GPU models, large NVMe arrays, or unusual RAM configurations that don't fit standard cloud SKUs

The tradeoff is straightforward: you get predictability and isolation, and in exchange you manage more of the stack yourself and pay more than you would for an equivalently-named VPS.

## Bare metal vs dedicated: the naming mess

Here's where it gets confusing. The industry uses "bare metal server" and "dedicated server" almost interchangeably, and DMIT is no exception — their dedicated server product line is literally called "Bare Metal Servers."

The practical distinction some providers draw is that a "dedicated server" historically came managed (with OS install, patches, and support handled by the provider), while "bare metal" usually means you get root and IPMI access and handle the OS layer yourself. DMIT's bare metal offering falls on the self-managed side: you get full root/IPMI access and reinstall control, and their TOS explicitly notes that "most of our services are unmanaged services" with a 72-hour support ticket response target.

If you're coming from a fully managed hosting background, that's worth knowing up front. DMIT gives you the hardware and the network; running the box is on you.

## DMIT's bare metal dedicated server lineup

DMIT positions its bare metal servers as single-tenant physical machines built to spec, rather than a fixed catalog of pre-built SKUs. The product page describes three configuration categories:

| Category | What it's built for | Hardware notes |
| --- | --- | --- |
| Compute Optimized | CPU-bound workloads — busy databases, app servers, virtualization hosts | AMD EPYC up to 128 cores / 256 threads, DDR4/DDR5 ECC, dedicated cores with no contention |
| Storage Optimized | Data-intensive workloads needing capacity and consistent low-latency I/O | All-NVMe / SSD / large HDD arrays, hardware and software RAID options |
| Enterprise & Custom | Special builds — GPU, large-memory, dedicated clusters | GPU and accelerator options on request, custom CPU/RAM/disk combos, IPMI included |

All three run on AMD EPYC platforms. DMIT's current generation stack across their locations breaks down as:

- **AN5** — AMD EPYC 9005 series (Zen 5), DDR5, PCIe 5.0 NVMe. Flagship, highest single-core and multi-core performance.
- **AN4** — AMD EPYC 9004 series (Zen 4). Field-tested, balanced per-core performance and core density.
- **AS3** — AMD EPYC 7003 series (Zen 3). Most competitive price-per-core; mature platform. The LAX AS3 platform is still being built out, so DMIT flags possible reduced disk performance and a lower SLA during that period.

Bare metal servers are quote-based. You describe your requirements and DMIT's team returns a tailored configuration and price. There's no published per-SKU monthly price list for bare metal on the site, which is normal for custom dedicated hardware but worth setting expectations around — you won't find a "$199 bare metal box" sitting in a pricing table the way you would with a VPS.

If you want to spec out a configuration and get a quote, 👉 [check DMIT's bare metal page through this link](https://bit.ly/DmiT).

## The network series: this is where DMIT differs from most providers

What actually separates DMIT from generic dedicated server hosts isn't the hardware — everyone can buy EPYC — it's the network engineering, specifically around China routing. Every plan is offered across three network series, and choosing the right one matters more than picking the CPU tier for a lot of buyers.

**Premium Network** combines Tier 1 transit with premium partners including DMIT's own backbone and China Telecom CN2 GIA. This is the top tier for reaching mainland China and the broader Asia-Pacific region with lower latency, fewer hops, and reduced packet loss. DMIT recommends it for corporate and e-commerce sites targeting China/APAC, live streaming and VOD, low-latency game servers for Asian players, and cross-border apps that need stable premium routing into China.

**Eyeball Network** pairs Tier 1 transit with reasonable-effort China routing via CMIN2/CMI and other Chinese eyeball ISPs. It's a middle ground — no premium routing guarantees, but noticeably better access for Chinese residential users than plain Tier 1. DMIT suggests it for websites and blogs with a mixed China/global audience, API backends and SaaS platforms, remote development servers, and download mirrors with moderate China traffic.

**Tier 1 Network** is the cost-efficient series with clean, optimized routing across Asia-Pacific and the Americas and no specific China-routing enhancements. It fits backup and archival servers, internal tooling and CI/CD infrastructure, VPN and relay nodes bridging APAC and the Americas, and cost-sensitive batch processing.

The honest summary from DMIT's own product copy: Premium (CN2 GIA) gives you the best quality at a higher cost per GB; Tier 1 is more economical but routes can vary by destination; and peak-hour congestion can affect non-premium routes to China. If your audience isn't in China, Premium is overkill. If it is, Eyeball might not be enough during evening peaks.

## Published plans with prices: the cloud instance tier

Since bare metal is quote-based, the only DMIT plans with fully published pricing on the site are their cloud instances — KVM virtual machines with dedicated cores and no vCPU oversubscription, which DMIT markets using "single-tenant" language in places. These aren't true dedicated servers in the bare-metal sense, but they're the closest thing to an orderable, priced product line, and a lot of people searching for hosted dedicated servers end up here first because they want a number they can budget against.

Here's what's currently published on the Premium Network in Los Angeles:

| Plan | vCore | RAM | Storage | Transfer | Port | Price (monthly) | Order link |
| --- | --- | --- | --- | --- | --- | --- | --- |
| TINY (AS3) | 1 | 2GB | 20GB SSD | 1000GB | 1Gbps | $10.90 | [Order](https://bit.ly/DmiT) |
| Pocket (AS3) | 2 | 2GB | 40GB SSD | 1500GB | 4Gbps | $16.90 | [Order](https://bit.ly/DmiT) |
| STARTER (AS3) | 2 | 2GB | 80GB SSD | 3000GB | 10Gbps | $34.90 | [Order](https://bit.ly/DmiT) |
| MINI (AS3) | 4 | 4GB | 80GB SSD | 5000GB | 10Gbps | $62.90 | [Order](https://bit.ly/DmiT) |
| MICRO (AS3) | 4 | 4GB | 160GB SSD | 7000GB | 10Gbps | $87.90 | [Order](https://bit.ly/DmiT) |
| MEDIUM (AS3) | 6 | 8GB | 160GB SSD | 15000GB | 10Gbps | $199.90 | [Order](https://bit.ly/DmiT) |
| LAX.AN5.Pro.MINI (AN5) | 4 | 4GB DDR4 | 80GB SSD | 5000GB | 10Gbps | $79.90 | [Order](https://bit.ly/DmiT) |
| LAX.AN5.Pro.MICRO (AN5) | 4 | 4GB DDR4 | 160GB SSD | 7000GB | 10Gbps | $110.90 | [Order](https://bit.ly/DmiT) |
| LAX.AN5.Pro.MEDIUM (AN5) | 6 | 8GB DDR4 | 160GB SSD | 15000GB | 10Gbps | $289.90 | [Order](https://bit.ly/DmiT) |

The AN5 (Zen 5) plans carry a clear premium over the AS3 (Zen 3) equivalents — the MEDIUM jumps from $199.90 to $289.90 for the same core/RAM/storage footprint. Whether that's worth it depends on whether your workload is single-core sensitive. For a database or a game server, the Zen 5 IPC gain can be meaningful. For a file mirror or backup box, it almost certainly isn't.

Note: the pricing page itself carries a disclaimer that "the products and prices in the table may not be updated in time due to adjustment, for reference only." Treat the numbers above as current as of this writing and verify on the order page before committing.

Eyeball and Tier 1 plans exist on the same footprint at lower price points — search results reference a LAX.AN5.T1.V2C2G plan at $14.90/month, for example — but the full published lists for those series weren't cleanly retrievable, so I'm not going to fabricate a table for them. If you want Tier 1 or Eyeball pricing, 👉 [open the DMIT cloud instance configurator here](https://bit.ly/DmiT) and pick your location and network series to see matching plans.

## Locations and what each is good for

DMIT runs three data center locations, each with a different geographic strength:

- **Los Angeles** — the flagship presence, sitting on the CoreSite and Digital Realty campuses. This is where DMIT concentrates their China-optimized peering (China Telecom AS4809, China Unicom AS9929, China Mobile International AS58807) plus CN2 GIA on the Premium tier. Also a strong pick for bridging APAC and the Americas because LAX is a major Pacific Rim interconnection point. Aggregate Tier 1 capacity here is up to 3.8Tbps.
- **Hong Kong** — closest physical proximity to mainland China, which shows up in reference latency measurements. AN5 plans here are currently Premium-only; AS3 plans are offered on Eyeball and Tier 1.
- **Tokyo** — runs on AMD EPYC 7003 (Zen 3) series. Good for Japan-facing and broader APAC workloads; reference latency to Shanghai is cited with low packet loss on Premium routing.

If your end users are in mainland China and latency is the priority, Hong Kong Premium is usually the first pick, with LAX Premium as the fallback when you want US-based infrastructure with good China reach. If your users are global and China isn't a factor, Tier 1 in any location will save you meaningful money.

## What you get and what you don't

A few things worth pulling out of DMIT's terms and product pages, because they affect the decision:

**SLA.** DMIT currently commits to 99% uptime. If SLA drops below 99%, you get half a month's credit; below 95%, a full month; below 90%, two months. That's a lower floor than the 99.9% or 99.99% SLAs enterprise dedicated hosts advertise, so if you're running something where every minute of downtime has a hard dollar cost, factor that in.

**Management.** Services are unmanaged. Support tickets are targeted at a 72-hour response window. You're expected to handle OS-level administration yourself or bring someone who can.

**IP handling.** IP replacement policies differ by network series. On Premium and Eyeball, without an `IP Care+` add-on you get a free replacement every 15 days; with `IP Care+` it's every 7 days. On Tier 1, without an `IP Guarantee+` add-on DMIT doesn't guarantee the IP is globally accessible — specifically calling out China, Russia, and countries with national network censorship. This matters more than people expect: if your China-facing service gets a poisoned IP, the replacement cadence determines how long you're dark.

**Refunds.** Full refund within 3 days and under 30GB transfer used (minus payment gateway fees). Partial refund within 30 days, calculated against either remaining transfer or remaining time, whichever is lower. No refund if you've been DDoSed, if the issue is "network not good enough," or if the IP isn't reachable in some region but you've used more than 3GB.

**OFAC restrictions.** DMIT doesn't accept orders from Cuba, Iran, Lebanon, Libya, Myanmar, North Korea, Somalia, Sudan, or Syria.

**Supported OS.** Ubuntu, Debian, CentOS, CentOS Stream, AlmaLinux, Rocky Linux, Fedora, openSUSE Leap, Arch Linux, Alpine Linux. Automated backups, instant snapshots, and SSH key authentication are all available on the cloud instance side.

## What third-party reviews say

This is where it's worth being careful, because the public review footprint for DMIT is thin and mixed.

Trustpilot shows a 2.6 TrustScore across only 4 reviews — a sample size too small to mean much on its own. An independent review on a personal blog (akr.moe) describes the hardware as powerful, bandwidth as plentiful, and the service as good for stable site hosting, while noting that pricing sits on the higher side and that PayPal, Alipay, and credit cards are supported. A longer write-up on a GitHub-hosted review page characterizes DMIT as having earned a solid reputation in a specific niche — users who need reliable, low-latency connections between overseas infrastructure and mainland China — which lines up with how DMIT markets themselves.

The pattern across what's publicly available is consistent: DMIT is seen as a premium-priced provider whose value proposition is China-optimized routing and modern EPYC hardware, not as a budget host. If you don't care about China routing, that value proposition weakens considerably and there are cheaper dedicated options elsewhere.

## Promotions and discount codes

DMIT releases discount codes periodically. Their TOS states these codes only apply to new customers, and that using a code issued to a specific existing user will get your service suspended with no refund.

From third-party coupon aggregators (which I can't fully verify against DMIT's current promotions), codes referenced include things like 20% recurring discounts on LAX Tier 1 annual plans, 10% recurring on LAX T1, and various seasonal sales. Because promo codes rotate and I can't confirm any specific code is live and applicable to your chosen plan right now, I'm not going to paste one into the article — the safe move is to check the order page for whatever's currently active. 👉 [You can view current promotions on DMIT's site here](https://bit.ly/DmiT).

## Choosing between bare metal and cloud instances here

If you've read this far, the practical question is whether you should be quoting a DMIT bare metal server or just buying one of the published cloud instance plans above. The decision comes down to a few real factors:

Go with **bare metal** if you need the whole physical machine — no hypervisor layer at all, custom hardware specs that don't fit a VPS SKU (large GPU, multi-TB NVMe RAID, 512GB+ RAM), or strict isolation for compliance. Expect to talk to their team and get a quote.

Go with a **cloud instance** if your workload fits one of the published configurations, you want instant self-service deployment, and you're fine with a KVM VM running on dedicated cores. You'll save money and time, and for a lot of workloads the "dedicated vCores, no oversubscription" guarantee is enough — you just won't have the box to yourself in the literal sense.

A reasonable middle path: start on a cloud instance plan to validate your workload and routing, then move to a bare metal quote once you know exactly what hardware footprint you actually need. DMIT's network series and locations stay consistent across both product lines, so the routing behavior you test on a VPS will be representative of what you get on bare metal in the same location.

## Final notes before you order

A few things worth double-checking on the order page rather than trusting this article for:

- Whether the plan you want is on AN5, AN4, or AS3, since the platform affects both performance and price
- Current transfer allowances and port speeds, since DMIT reserves the right to adjust plan resources
- Whether your target region needs Premium, Eyeball, or Tier 1 routing — this is the single biggest price lever
- IP replacement terms for your chosen network series, especially if you're serving China

If you want to dig into configurations or pull a bare metal quote, 👉 [head to DMIT's site through this link](https://bit.ly/DmiT). The bare metal page has a contact form for custom builds, and the cloud instance configurator lets you pick location and network series to see the matching published plans with live pricing.
