# IEPL dedicated server: choosing the right cross-border line, reading real pricing, and avoiding compliance pitfalls

If you've typed "IEPL dedicated server" into a search engine, you are almost certainly running into one of three real situations: you need a stable China→Hong Kong path for a cross-border team or store; you want a physical machine, not a noisy shared VPS, but you still want it to ride a private line instead of the open internet; or you've been quoted three different prices for what looks like the same product and you want to know why. This guide walks through all three, with facts pulled from current provider documentation (Mkcloud in particular, since that's who this piece is built around) and from the broader cross-border hosting market.

## What "IEPL dedicated server" actually means

Strictly speaking, IEPL is short for **International Ethernet Private Line**, a Layer 2 point-to-point Ethernet service over MPLS-style transports. The bandwidth you buy is dedicated, the route is predetermined, and the line does not share capacity with the public internet. In China, the term gets stretched: many local providers bundle their VPS and dedicated-server products into IEPL-priced SKUs because the *network* is IEPL even if the *machine* on top of it is still virtualized. When you read a price page, pay attention to which of these three you are getting:

- **IEPL on a virtualized cloud server.** Looks like a dedicated line, but the hardware is still a VM. Usually cheaper, and most providers (Mkcloud included) ship this as their default.
- **IEPL on a dedicated physical server.** A real bare-metal box sitting behind the IEPL port. Higher cost, but you are the only tenant on the CPU/RAM/SSD, which matters for sustained CPU-bound tasks.
- **IEPL as a transport you connect to yourself.** A raw Layer 2 or MPLS handoff from the carrier into your office. This is enterprise pricing — typically starts at several hundred USD per month — and is not what Mkcloud or similar VPS-tier sellers offer.

The search term you typed sits in the first two buckets. The third is a different product category that usually shows up under V-IEPL or "international leased circuit" search results from carriers like China Telecom Global or Telin, not from VPS providers.

On this side of the market, you are bundling three decisions: line (which direction), machine type (cloud or physical), and billing mode (traffic-based or dedicated bandwidth). Picking the wrong combination is the most common reason people end up overpaying or with the wrong latency profile.

## Who actually buys an IEPL line today

Cross-border e-commerce is the headline use case, but it isn't the only one.

- **Cross-border e-commerce teams** with stores, accounts, or operations in Hong Kong, Japan, or the US still need a stable "always out" path so internal browsers and API clients don't get flagged by the platforms.
- **Content and creative studios** that upload large video assets to international platforms daily.
- **Marketplace ops and affiliate teams** that manage many logins from a single physical location.
- **Internal tooling vendors** that need a fixed egress IP associated with a Chinese ASN, so their backend clients can whitelist it.

For all of these, the deciding factor is rarely the absolute number of megabits. It's about **how predictable the route is from one week to the next**, which is exactly what makes IEPL interesting relative to vanilla CN2 or BGP transit.

## What separates a serious IEPL-dedicated-server provider from a reseller

A few things worth checking before you pay any deposit.

1. **Real network ingress IP per instance.** On most decent setups, each VPS gets its own ingress IP and its own egress IP, not a shared pool. This is what makes per-browser / per-account isolation possible. Mkcloud's product documentation explicitly states that the shared and dedicated tiers both ship with one independent ingress IP and one independent egress IP per machine — a useful baseline to compare other providers against.
2. **Compliance posture.** In China, the sale of "tunnel" products that route personal internet through a foreign endpoint is restricted. Any provider offering IEPL-dedicated-server products in this segment should clearly state that the service is intended for legitimate outbound business use, requires identity verification, and is not for personal VPN/parental-control-style tunneling. Mkcloud's checkout pages spell this out, which is a good sign.
3. **Pricing transparency at the line, plan, and traffic-tier level.** A single "from ¥X/month" headline price is fine for hero text, but the real decision lives in the per-tier breakdown. If you cannot find a published table of traffic tiers with prices — say 1TB / 2TB / 4TB / 6TB / 10TB — there is usually a reason.
4. **Refund and SLA terms.** A simple "质量问题退款" (refunds on quality issues only) policy with no SLA on uptime is the industry norm on this segment. Anyone promising 99.99% uptime and full SLA without an enterprise contract is either rebranded residential transit or being optimistic.
5. **Honest descriptions of the route.** "End-to-end 1–2ms" between Guangdong and Hong Kong is realistic. But applying that same number to a full journey from your office in Shanghai, through the provider's nine hops out, and into a US endpoint is not. Any provider that splits these two — line latency vs. full path latency — is more trustworthy than one that doesn't.

## The Mkcloud cross-border lineup at a glance

Mkcloud has built a relatively focused portfolio around China's outbound scenarios. The lines are categorized into IEPL, IPLC, IX (cloud peering), and DDoS-protected variants. All products ship as cloud servers, with a dedicated physical server option available on key lines.

| Line | Direction | Type | End-to-end latency (provider reference) | Entry price (verified) | Available form |
| --- | --- | --- | --- | --- | --- |
| 广港 IEPL | Guangdong → Hong Kong | IEPL | 1–2 ms | ¥358 / month, from | Cloud server / dedicated physical |
| 深港 IX | Shenzhen → Hong Kong | IX (cloud peering) | 1–2 ms | ¥158 / month, from | Cloud server / dedicated |
| 沪港 IPLC (shared) | Shanghai → Hong Kong | IPLC, shared bandwidth | ~21 ms | ¥288 / month | Cloud |
| 沪港 IPLC (dedicated) | Shanghai → Hong Kong | IPLC, dedicated bandwidth | ~21 ms | ¥388 / month, from | Cloud |
| 沪日 IPLC / 沪日 IX | Shanghai → Japan | IPLC / IX | 25–28 ms | ¥358 / month, from | Cloud |
| 沪美专线, 1 TB | Shanghai → US | IPLC | 124–134 ms | ¥428 / month | Cloud |
| 沪美专线, 2 TB | Shanghai → US | IPLC | 124–134 ms | ¥698 / month | Cloud |
| 沪美专线, 4 TB | Shanghai → US | IPLC | 124–134 ms | ¥1,258 / month | Cloud |
| 沪美专线, 6 TB | Shanghai → US | IPLC | 124–134 ms | ¥1,758 / month | Cloud |
| 沪美专线, 10 TB | Shanghai → US | IPLC | 124–134 ms | ¥2,888 / month | Cloud |
| 厦港 / 泉港 高防 | Fujian → Hong Kong | High-defense IPLC | 1–2 ms | Per configuration, DDoS-grade | Cloud |
| 上海 CN2 | Domestic only | China Telecom CN2 | — | Configuration-based | Cloud |

A few things stand out. First, the **Guangdong → Hong Kong IEPL** lane is the headline product, and the **Shenzhen → Hong Kong IX** lane is the budget alternative for users who already have a supported cloud VM sitting in Shenzhen to act as the ingress front. Second, the **Shanghai → US** tier is clearly traffic-metered and the price curve is steep: doubling from 4 TB to 6 TB adds roughly ¥500, and 10 TB is double the 6 TB price. Third, the Fujian → Hong Kong high-defense line exists for teams that need DDoS tolerance on top of a private route — useful but not a default pick unless you specifically need it. The prices above are documented on Mkcloud's homepage and store pages, along with the per-traffic-tier table for the Shanghai→US line.

If your workload only needs Hong Kong direction and you're connecting from South China, the right starting point is closer to ¥228/month for the shared entry on the Guangdong IEPL line, climbing upward based on how much monthly traffic and how much peak bandwidth you want. If you must run from a Shanghai egress into Hong Kong, the entry is around ¥288/month on the Shanghai IPLC shared tier.

## Reading the Mkcloud price table without getting burned

Most confusion around IEPL-dedicated-server pricing comes from the gap between an entry-tier SKU and a usable SKU. Here are the patterns worth knowing.

The entry prices tend to be **shared-bandwidth** packages with tight traffic caps. On the Guangdong IEPL line, the "from ¥358/month" headline is the cloud-server/dedicated-server sleep price with a 200 Mbps peak and a relatively small monthly traffic allotment. On the Shanghai→US IPLC line, you can see the cap very clearly: 1 TB at ¥428/month is fine for low-volume sync tasks, but anything that pushes video or large image batches across the Pacific breaks through that ceiling fast.

The **shared vs dedicated bandwidth** distinction is about rate consistency, not about IP exclusivity. Both tiers get their own ingress and egress IPs on Mkcloud. Shared bandwidth swings between idle and the burst ceiling; dedicated bandwidth gives you a contracted minimum rate. Continuous uploader workloads usually belong on dedicated bandwidth; intermittent tasks on shared.

**Traffic measurement is bidirectional** on the tier where it applies. Both upload and download count toward the cap. Once you blow through the cap, service is suspended until you add-on via self-service or ticket. There is no automatic prorated mode once you cross the line.

**Quarterly and annual billing change the math.** On the Shanghai IPLC shared tier, for example, the published quarterly equivalent is ¥864 and the annual equivalent is ¥3,456, both of which come out lower per month than the monthly run-rate. If your workload is real and ongoing, buying quarterly or annually is the cheapest honest path; if you're testing, monthly keeps the exit cheap.

There is no permanent coupon on the price page at the time of writing — past events (双旦, 618, 新春 666) shipped discount codes such as MK-8.8 for traffic plans or MK-NEW for new-customer machines, but they tied to specific windows and should not be relied on as a standing discount today.

## Compliance and identity, plainly

IEPL-dedicated-server products in this segment run inside Chinese regulatory boundaries. On Mkcloud in particular, the checkout flow makes the rules explicit. You will need to provide a Chinese identity document for individual buyers, or a business license for corporate buyers. The service is positioned for legitimate cross-border commerce, content operations and similar outbound business workloads.

There are a few practical consequences:

- The egress is **outgoing only**. You cannot run a public website, accept email, or take payment callbacks on the line's egress. If inbound is your requirement, this is the wrong product category.
- The line is not a personal-use tunnel. The provider actively rejects orders linked to personal VPN / "return to China" use cases. This is policy, not just marketing.
- Plan changes are ticket-based. Upgrades and downgrades both go through support, and downgrades to a cheaper plan don't return the difference. Plan changes can also mean migration to a new machine and new IP.

These aren't quirks to apologize for; they are the industry's current shape and worth weighing in.

## How to pick the right line for your workload

A short decision flow that maps cleanly to the table above.

**Target is Hong Kong and you connect from South China.** Start with the Guangdong→HK IEPL line. If you already maintain a supported cloud VM in Shenzhen (Alibaba, Tencent, Baidu, Volcano or Huawei South China), compare the Shenzhen→HK IX line — the entry price is lower, but you must factor the cloud front machine into your cost and management overhead.

**Target is Hong Kong and you connect from East China.** The Shanghai IPLC options are the natural fit. The shared tier at ~¥288/month is fine for low-volume browser/account workflows; the dedicated tier at ~¥388/month is the better pick if your days involve continuous outbound traffic.

**Target is Japan.** The Shanghai→Japan IPLC lane sits at 25–28 ms end-to-end. The Shanghai→Japan IX variant fits users who already have a supported Shanghai cloud VM as the ingress.

**Target is the US.** The Shanghai→US IPLC lane is the only path, and it is bandwidth-bounded by traffic caps rather than peak rate. Budget in the actual TB usage; the price ladder is steep enough that you'll notice tier-jumps.

**You need DDoS tolerance.** The Fujian→Hong Kong high-defense lines on Mkcloud are the only options in this lineup that publish a high-defense posture, with the published SKU including up to 300 Gbps DDoS protection on certain dedicated configurations. **Confirm protection scope and trigger rules with a pre-sales ticket before you commit.**

**You need inbound.** Stop here. None of these products support inbound. Look at a different category.

## Before you pay: a short checklist

A few things to confirm after you've narrowed the line down, but before you hit checkout.

- **Ingress IP will be tied to a region.** On direct-connect SKUs, the ingress can be bound to a single province. The binding can be changed, but it is not dynamic. Make sure you can connect from inside that province on the network you're planning to use.
- **Peak versus sustained bandwidth.** On shared tiers, peak is a ceiling, not a floor. If you need a sustained 50 Mbps or more, plan to move up to the dedicated tier.
- **Traffic cap behavior.** Check whether add-on traffic is sold per-pack or by tier upgrade, and confirm whether adding traffic resets the cycle or extends it.
- **Test before paying for a year.** Most providers offer monthly billing on entry plans, and even better, a short testing window before a refund. Use it.
- **Spread the OS installation and reboot behavior.** Asking whether snapshots and backups are provided is fair — on this category they're often not bundled, so plan your rollback yourself.

## FAQ

**IEPL vs IPLC — which one do I want?** They are both dedicated private lines. IEPL is delivered as Ethernet (Layer 2), IPLC as traditional leased circuit. In practice on the VPS side, you mostly see IEPL marketed for the South-China routes and IPLC for the longer Shanghai-based routes. Pick based on the line and route, not based on the acronym.

**Can I install my own OS image?** Yes, the package supports re-installation of common Linux and Windows images per their documentation. Custom kernel-level changes are usually fine; virtualization layers (running a hypervisor inside a VM) are usually not.

**Can I upgrade later if I outgrow the entry tier?** Upgrades go through support tickets. Whether your IP and data carry over depends on the engineering decision for that specific move, not on automation. Downgrades to a cheaper plan don't refund the difference, so plan ahead rather than gambling on a smaller plan and stepping back up.

**Refund policy?** The provider's published position is refund for quality issues only, contingent on ticket, test screenshots and description. Once opened, you cannot migrate to another region. This is standard for the segment.

**Setup time?** Most open-market SKUs (cloud-server plans) are listed as auto-provisioned within roughly a minute after payment clears. Custom physical dedicated servers will take longer and explicitly require separate confirmation.

## Where to go from here

The full pricing tables, line rules and login are on the Mkcloud store, with product pages per line direction. If you want a direct review of the Guangdong→HK IEPL line on its headline pricing, the cheapest entry tier is already documented at the store, with → [👉 Guangdong→HK IEPL 套餐与价格](https://bit.ly/MKCLoud) sitting on the line-summary page. For head-to-head comparison of the Shanghai→US tier (e.g., 1 TB / 2 TB / 4 TB), the full traffic-tier table is published and the same → [👉 Mkcloud 全线套餐选购入口](https://bit.ly/MKCLoud) gets you there without retyping.

Before you click purchase, re-confirm the line direction matches your access region, the bandwidth/traffic plan matches your monthly usage, and you are not buying an outbound-only product for something that needs inbound. With those three aligned, you should not need anyone to walk you through the rest.
