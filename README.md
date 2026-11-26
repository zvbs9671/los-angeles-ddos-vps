# Los Angeles DDoS Protected VPS: What $200/Year Buys, How Much Mitigation Counts as Real, and Which Plan Fits a Game Server, API or VPN

SixtyNet's DDoS-protected Los Angeles line, sold under the LAX name, starts at **$200 per year** for 2 cores, 2 GB RAM, 40 GB NVMe and 5 TB of traffic on a 2 Gbps premium network, down 17% from a $240 list price. That works out to about $16.67 a month for a box that is supposed to stay online when someone points a flood at your IP instead of null-routing you and closing the ticket.

What the plan page does not publish is how many Gbps of attack traffic the filter absorbs, or whether scrubbing runs always-on. Those two numbers decide whether "DDoS protected" means anything on the day you actually get hit, and they are the first thing to nail down before paying. The rest of this covers the full LAX plan table, how to separate real mitigation from a marketing line, what Los Angeles latency actually looks like, and how SixtyNet's pricing sits against two other LA hosts that do publish their capacity.

## The short version

> Most readers searching this term have one server, one public IP, and a bad week behind them. Start with **LAX-STARTER**: 2 cores, 2 GB, 40 GB NVMe, 5 TB traffic at **$200/year**, about $16.67/month. A single small game server, a VPN endpoint, or an API serving a few hundred users fits in 2 GB. A Docker host running four services does not.
> 👉 See the LAX plan lineup and current annual pricing

## SixtyNet's Los Angeles DDoS-protected plans

The LAX series is the only line on SixtyNet's VPS page that carries the DDoS protection label. All four plans are quoted annually, and each one shows a struck-through list price next to a discounted rate labelled "Save 17%".

| Plan | Cores / RAM | NVMe / Traffic | Network | Annual price | Order |
| --- | --- | --- | --- | --- | --- |
| LAX-STARTER | 2 / 2 GB | 40 GB / 5 TB | 2 Gbps | **$200/yr** (list $240) | [ Order LAX-STARTER](https://sixtynet.net/aff.php?aff=39) |
| LAX-MICRO | 4 / 4 GB | 80 GB / 10 TB | 4 Gbps | **$400/yr** (list $480) | [ Order LAX-MICRO](https://sixtynet.net/aff.php?aff=39) |
| LAX-MEDIUM | 8 / 8 GB | 160 GB / 20 TB | 10 Gbps | **$800/yr** (list $960) | [ Order LAX-MEDIUM](https://sixtynet.net/aff.php?aff=39) |
| LAX-LARGE | 8 / 16 GB | 320 GB / 40 TB | 10 Gbps | **$1,600/yr** (list $1,920) | [ Order LAX-LARGE](https://sixtynet.net/aff.php?aff=39) |

Two details worth pulling out of that table. First, storage is quoted with an IOPS figure on the product page (12,500 on the smaller two plans, 25,000 on the larger two), which matters more than raw gigabytes if you are running a database rather than a static file dump. Second, the jump from LAX-MEDIUM to LAX-LARGE doubles RAM and traffic for double the money while the core count stays at 8. That is a memory-and-bandwidth upgrade, not a CPU upgrade, and you should only take it if you are actually short of RAM.

The port speed in the Network column is a burst ceiling, not a sustained rate. Divide the traffic allowance by the seconds in a month and 5 TB on LAX-STARTER averages out to roughly **15 Mbps** if you use the whole allowance; 40 TB on LAX-LARGE averages about **123 Mbps**. A 10 Gbps port with a 20 TB cap will not move 10 Gbps for a month, and no provider's will.

### The other plans on the same page

SixtyNet also lists two non-LAX options. They are not the DDoS-protected line, and for most readers here they are the wrong purchase.

| Plan | What it is | Annual price | Order |
| --- | --- | --- | --- |
| Residential-LARGE | 16 cores / 16 GB / 320 GB NVMe / 40 TB, residential IP | $1,660/yr (list $1,980) | [ View current price](https://sixtynet.net/aff.php?aff=39) |
| Los Angeles China Unicom-Premium | AS9929 China Unicom routing, free BGP session setup, 100M base with 3x burst, 95th-percentile billing | $2,500/yr (list $3,000) | [ View current price](https://sixtynet.net/aff.php?aff=39) |

The residential plans carry a notice in plain text on the product page: **ports 80 and 443 are closed** because of IP risk control, and refunds on those plans go to account balance only. If you are hosting a website, that rules them out before price enters the conversation. The China Unicom plan is a bandwidth product for China-facing traffic with 95th-percentile billing, which is a different purchasing model from a flat monthly VPS allowance.

## How to tell whether "DDoS protected" is real

Capacity and filtering mode are the whole game. A provider advertising 10 Gbps of mitigation goes down under a 40 Gbps flood exactly like an unprotected server does; the number on the marketing page is a ceiling, and attacks above it get through or get black-holed. Ask what the scrubbing capacity is in Gbps and whether it is shared across the whole rack.

The second question is always-on versus on-demand. Always-on inline filtering inspects every packet from the moment your server boots. On-demand systems watch for anomalies and pull your traffic into a scrubbing path after detection, which leaves a window where the attack lands on your box. A page that says attacks are "stopped within a minute" is describing detection-then-mitigation, not an inline filter.

The third is layer coverage. Network-layer scrubbing handles UDP floods, SYN floods, and the grudge traffic that game servers and IRC attract. It does nothing for an HTTP request flood that exhausts your PHP or Node workers, because those requests are well-formed and arrive slowly. Application-layer attacks need a WAF or rate limiting in front of the app, and no amount of Gbps capacity substitutes for it.

SixtyNet's LAX page lists "DDOS Protection" as an included feature and stops there. No capacity figure, no statement about filtering mode. That is a gap you close with a presales ticket, not with an assumption.
👉 Open the LAX plan page and check whether the spec sheet now lists a mitigation capacity

## What Los Angeles is actually good for

Los Angeles is the main US landing point for trans-Pacific submarine cables, and one LA provider publishes the numbers that follow from that: **110–150 ms to Tokyo**, **160–180 ms to Singapore**, **5–30 ms across the western US**, and **60–80 ms to the east coast**. If your players, users or VPN customers are in Japan, Korea, Taiwan, Hong Kong or Southeast Asia, LA is the lowest-latency US location you can pick without moving to Asia itself.

If your users are all in Europe, the same geography works against you. A Los Angeles server adds well over 100 ms for a Frankfurt or London visitor, and no amount of DDoS capacity compensates for a game server that feels laggy. Pick a European location and spend the difference on capacity there.

For a US-only audience, LA is fine but not uniquely so. The 60–80 ms to the east coast is acceptable for APIs and web apps and mediocre for real-time anything.

## Market reference: what two other LA hosts publish

Neither of the hosts below is the recommendation here, and neither link belongs in this article. They are useful because both publish information SixtyNet does not, which tells you what a complete spec sheet looks like.

| Provider | Entry price | Published mitigation | Port | Limits worth knowing |
| --- | --- | --- | --- | --- |
| SixtyNet (LAX) | $200/yr, ~$16.67/mo | Not published | 2–10 Gbps | Residential plans close ports 80/443 |
| INIZ (Los Angeles) | £4.50/mo, £48.60/yr | Up to 100 Gbps L3/L4, on-site filtering, included | 2.5 Gbps | Extra IPv4 addresses cost extra |
| ExtraVM (Los Angeles) | $4.50/mo, monthly only | Global Secure Layer plus in-house eBPF/XDP filtering, no capacity figure | 1 Gbps on 1–2 GB plans, 5 Gbps from 3 GB | 5-day refund, fiat payments only; Windows allowed from 3 GB without a licence |

A few numbers in that table deserve arithmetic rather than a glance.

INIZ's annual price is exactly 90% of twelve monthly payments: £4.50 × 12 = £54 against £48.60 billed yearly, a £5.40 saving, or "pay for 10.8 months". On the 16 GB plan the same 10% is £76.80 a year. ExtraVM publishes no annual rate at all, so twelve months of its $4.50 plan is $54 with no discount available. And £4.50 is not $4.50. At any realistic exchange rate the INIZ entry plan costs meaningfully more in dollars than the ExtraVM one, so comparing the two digits directly produces a wrong answer.

The other thing that table shows is how much a published mitigation capacity is worth. INIZ states 100 Gbps of L3/L4 on-site filtering. ExtraVM names its upstream scrubbing vendor and its local filter technology but gives no Gbps figure. SixtyNet gives neither. You can still buy from any of them, but with SixtyNet you are buying an unquantified claim, and you should price that in.

Two smaller restrictions that catch people: ExtraVM's 5-day refund window applies to fiat payments only, so a crypto payment is final, and its Windows templates ship without a licence, which means you supply the key. SixtyNet's own page does not state a refund window for the LAX line at all. Third-party write-ups of the LAX series report a 3-day money-back window and that card payments have been paused for fraud screening; treat both as things to confirm at checkout rather than settled facts.

## Which LAX plan fits your workload

**One small game server, a VPN endpoint, a single API or a low-traffic site → LAX-STARTER at $200/year.** 2 GB is the binding constraint, not the 2 cores. Minecraft with a handful of plugins, WireGuard for a dozen clients, or a FastAPI service with a small database all fit. Add a memory-hungry JVM or a second database and you will be swapping.

**Several containers, a real database, or a game server with a web panel alongside it → LAX-MICRO at $400/year.** The 4 GB and 80 GB of NVMe are what you are paying for; the extra two cores are secondary. This is the plan most small communities should be on after their first out-of-memory kill.

**A public web application, a CI runner, or anything serving concurrent users → LAX-MEDIUM at $800/year.** 8 GB and 160 GB of NVMe with a 10 Gbps uplink covers a production LAMP or Node stack with room to spare. Confirm with sales that ports 80 and 443 are open on this line first. The residential plans explicitly close them and the LAX page does not carry that notice, which is not the same thing as a guarantee.

**LAX-LARGE at $1,600/year** is for workloads that are genuinely RAM-bound: a large MySQL instance, an Elasticsearch node, a game server with a big world in memory. If your CPU is the bottleneck rather than memory, the 8 cores here will not help you and you should be looking at a dedicated box.

## FAQ

**Is there a refund window?** Not stated on the LAX product page. Third-party reviews of the line report 3 days, which is short compared with the 30 days some hosts offer. Ask before you pay, and pay with a method you can reverse if the answer is vague.

**Can I run Windows?** Not documented for the LAX line. The plans are quoted with NVMe storage and no OS list on the plan cards, so confirm the template library and licensing situation with sales. Hosts that do offer Windows commonly ship it unlicensed, meaning you bring the key.

**Can I upgrade later?** Not stated for the LAX plans. Upgrading between tiers on the same host is normally a rebuild or a resize with a prorated difference, but "normally" is not a policy. Get it in writing if you expect to grow from STARTER to MICRO within a year.

**How much IPv6 do I get?** Not listed on the plan cards. Ask. Several LA hosts hand out a /64 by default and up to 100 addresses on request, but SixtyNet's page does not say, and guessing an address family you depend on is a bad way to start.

**Which payment methods work?** Third-party write-ups report card payments suspended for fraud screening. Check the checkout page before committing to an annual term, because a 12-month prepay on a payment rail that might not be there at renewal is a real risk.

**Does the DDoS protection apply to every SixtyNet location?** SixtyNet lists Los Angeles as its datacenter, so the question is moot for now. It matters for any multi-location host: protection is usually tied to specific facilities, and the same brand's other regions may have none.

## Before you order

Two things to confirm in writing, in this order.

**One: the mitigation capacity and whether it is always-on, for the specific plan you are buying.** "DDoS protected" on a feature list covers everything from a 10 Gbps filter that folds under a real flood to a 100 Gbps inline scrubber. SixtyNet does not publish either number, so a presales ticket is the only way to know what you are buying. Ask for the Gbps figure and for confirmation that filtering is inline rather than triggered after detection.

**Two: whether $200 is the renewal rate or a first-term discount.** The page shows $240 struck through next to $200 with "Save 17%", and it does not say what happens at renewal. On an annual plan that difference is $40 a year, and it decides whether the true cost of ownership over three years is $600 or $720.

If both answers come back reasonable, LAX-STARTER at $200/year is a defensible entry point for a small LA-based game server or VPN endpoint, and the 17% annual discount is real arithmetic rather than a marketing label. If the capacity answer is vague, that tells you something too.
👉 Check the LAX plans and current annual pricing before you commit
