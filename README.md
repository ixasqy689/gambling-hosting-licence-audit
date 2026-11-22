# Gambling hosting: what a licence audit checks, what match-day traffic breaks, and when a €5 KVM VPS is genuinely enough (plan table + five-host comparison)

Four things decide which gambling hosting you buy: where the data physically sits, who can certify the RNG running on it, how the network behaves in the ninety minutes around kick-off, and whether card data ever lands on your disks. AvaHost answers a different set of questions, and answers them cheaply. **KVM VPS from €5.00/mo, bare metal from €85.00/mo, DDoS mitigation included on every plan, billed in EUR, payable in 20+ cryptocurrencies.** That covers affiliate estates, staging builds, back-office systems and pre-licence development. It does not cover an MGA- or UKGC-licensed casino, and where that line falls is the whole point of this page.

> **Quick pick:** pre-licence build, affiliate/SEO estate, bonus engine or back office: the KVM VPS line, €5.00–€40.00/mo. Production platform whose *sustained baseline* (not match-day spikes) has pinned the VPS ceiling: bare metal, €85.00–€149.00/mo, and budget engineer time on top because it ships unmanaged. If your licence names Malta, the UK or a US state, the missing piece in a Moldovan single-region deployment will not be price.
>
> 👉 [Compare AvaHost's VPS and dedicated plan ranges](https://my.ava.hosting/aff.php?aff=15562)

## The plan line-up, and what each one is actually for

AvaHost has run out of Chișinău, Moldova since 2002 and sells three infrastructure layers from one client area: shared web hosting, KVM-backed VPS, and bare-metal dedicated servers. Only the last two matter for a gambling workload.

| Plan line | Core configuration | What it suits in a gambling stack | Price | Order |
| --- | --- | --- | --- | --- |
| KVM VPS | Fixed vCPU and RAM allocation per instance, NVMe on every plan, dedicated IPv4, root access, OS reinstall from the panel, DDoS mitigation | Staging and pre-launch builds, affiliate and SEO estates, bonus/CRM engines, reporting and back office | €5.00–€40.00/mo | [Pick a VPS plan](https://my.ava.hosting/aff.php?aff=15562) |
| Dedicated servers | Whole physical machine, no hypervisor layer, unmanaged by default, managed support optional | Production platform once baseline load saturates a VPS; database or RNG-tier boxes you tune yourself | €85.00–€149.00/mo | [See dedicated server pricing](https://my.ava.hosting/aff.php?aff=15562) |
| Managed support add-on | Engineer time on OS-level configuration and incident response, charged hourly | Teams with no sysadmin on call | €20.00/hour | [Ask about managed support](https://my.ava.hosting/aff.php?aff=15562) |

Those figures are the published spread across plans. Per-plan CPU, RAM and disk sit on the order page, and that is the only place worth checking, because a range tells you nothing about whether the top VPS has enough RAM for a player database.

Billing is in EUR with the period chosen at checkout. Payment goes through Visa, Mastercard, PayPal, bank transfer, WebMoney and 20+ cryptocurrencies including BTC, ETH and USDT. Weekly backup is an option on VPS, and the team will handle a cPanel-to-KVM migration including database transfer, DNS TTL pre-staging and mail record validation.

Anyone billing in USD should note the currency exposure: a EUR 149 server moves with the exchange rate every month, and infrastructure is the one line item you cannot renegotiate mid-contract.

## What a licence audit asks, and where AvaHost's answers stop

An auditor or licensing consultant does not open with CPU. The list is short and unforgiving: which jurisdiction holds the player data, which standard the facility is certified to, who tested the RNG, and can you produce a log for a specific hand three months ago.

AvaHost's pages make none of the usual certification claims. No ISO 27001, no SOC 2, no PCI DSS, no named testing lab. Treat a missing claim as a missing certificate until somebody emails you the PDF, and ask for it before you sign, because retrofitting compliance onto a live platform means migrating it.

The competitors publish the other half of the picture. Gambitec states ISO 27001 and PCI DSS on its infrastructure and ships servers with **hardware random number generators** so operators can pass GLI and iTechLabs certification. MassiveGRID claims MGA and UKGC technical compliance, PCI DSS Level 1, ISO 27001 and 9001, GDPR, and integration with eCOGRA, GLI and BMM for game-integrity certification. NovoServe notes that ISO 27001 and SOC 2 in its Dutch and US facilities are often a precondition for KSA or MGA audits. Leaseweb certifies data centres to ISO 27001, SOC type 1 and PCI DSS. Internet Vikings holds ISO 27001.

On residency, AvaHost's footprint is a single hardened facility in Moldova. Where a licence requires player data to stay inside the licensing jurisdiction (Malta for MGA, a named state for US operators), a single-region Moldovan deployment does not meet it, and no amount of network capacity changes that. Where your licence is issued elsewhere and you simply want the infrastructure outside a particular country, the calculus is different.

RNG is the question people get wrong. On a KVM guest with root, whatever generator you run is software you installed, and a lab certifies the generator in the environment where it executes. Ask which lab writes the report and whether the host's stack supports that lab's test procedure. Hardware RNG in the chassis is a shortcut Gambitec sells; AvaHost sells you the root access to install your own.

## The €20/hour line that sets your real monthly cost

A €85.00 dedicated server is unmanaged. Managed support runs €20.00/hour. Twenty hours of engineer time in a month is €400, which turns an €85 commitment into roughly €485. Ten hours is €200, still more than double the hardware.

That math decides more purchases than any benchmark. If nobody on your team can tune a kernel, write firewall rules and ship logs to a collector, price the hours before you price the server. If you have a DevOps engineer who wants root and no ticket queue, the unmanaged model is the reason the price is what it is.

The SLA has the same shape. AvaHost publishes 99.9% uptime covering core power and network, and states plainly that the scope does not extend to downtime caused by customer-side software or misconfiguration. That is the right way to write an SLA, and it also means the number does not protect you from the failure mode you will actually hit: your own deploy at 21:40 on a Saturday.

## Noisy neighbours are a compliance problem, not just a speed problem

Shared hosting on a gambling workload fails twice. The first failure is latency: a co-tenant's traffic spike adds jitter to your session handling and players abandon bets. The second is quieter and worse. On shared infrastructure, log integrity is not yours to control, and a compliance record you cannot vouch for is not a record.

KVM gives each instance a fixed allocation rather than a burst pool, and NVMe cuts fsync latency on exactly the write-heavy paths a gambling platform lives on: transaction logs, session persistence, audit trails. A dedicated IPv4 per VPS keeps SSL binding and geo-restriction rules clean, which matters when you're enforcing country blocks by IP. Root means you set log rotation, retention and permissions yourself.

Owning that also means owning the consequence. A box you can reinstall from a control panel in thirty seconds is a box whose local audit trail can disappear in thirty seconds. Ship syslog and application logs to a collector outside the instance, or the retention policy you wrote is decorative.

## Match day: why generic DDoS protection isn't the same product

Sporting events produce a traffic shape that ordinary mitigation handles badly: odds updates in the thousands per second, sub-millisecond delivery expectations to the front end, and a flood of both legitimate players and attackers arriving in the same minute.

A filter tuned for web traffic will drop or delay legitimate packets, and delayed packets during in-play betting look identical to a broken platform. NovoServe's answer is "game-aware" edge filtering that it says drops malicious traffic without dropping player packets or adding jitter. Gambitec says its multi-level mitigation rules are tuned specifically to iGaming traffic patterns. MassiveGRID quotes 10+ Tbps of mitigation with iGaming-specific profiling for exactly the major-event window. Internet Vikings runs Cloudflare's advanced protection in front of its estate.

AvaHost includes DDoS mitigation on every plan. Every host's capacity figure is self-declared, so put the question in writing: what scrubbing capacity sits in front of my IP, what triggers it, and what is the process when a flood exceeds it. There is also no multi-region failover here. If a regulator expects an active-active pair in two jurisdictions, you are designing that yourself on top of whatever you rent.

## What an hour of downtime costs, according to one vendor's sales page

MassiveGRID estimates that iGaming platform downtime costs **$11,200 per minute on average, $672,000 per hour**, that a major-event outage runs past **$8M**, and that **20–30% of players** leave after an incident, with $8.1M+ a year saved at 100% uptime. Those are the hosting company's own numbers on its own product page, with no methodology attached. Use them as an order of magnitude and nothing more. Leaseweb, for its part, only says that every second of downtime means thousands in lost revenue.

The useful takeaway is structural rather than numerical. Regulated operators pay several times the AvaHost price for a reason that has little to do with hardware: it buys an SLA with a failover topology behind it, data sitting in an approved jurisdiction, and an environment a lab has already certified.

## Five specialist hosts, side by side

None of these five publishes prices on the pages above, so the comparison is about capability and shape, not cost.

| Host | Compliance claims on its own site | Footprint | Deployment options | Worth knowing |
| --- | --- | --- | --- | --- |
| Internet Vikings | ISO 27001; MGA-compliant Malta hosting with a usage-billed replication server | 24 US states, Canada, Europe (a 2023-dated page; New Jersey via Atlantic City, Connecticut via nearby facilities) | Cloud, bare metal, bare-metal GPU, colocation from 1U, disaster recovery | Colocation from a single rack unit is unusual at this tier |
| NovoServe | ISO 27001 and SOC 2 in Dutch and US facilities | Amsterdam, Rotterdam, Copenhagen (large deployments only), New York | Bare metal, private VLAN tagging for tier separation, IPMI/KVM on every server, own on-site engineers 24/7 | Accepts BTC, ETH, USDT, SEPA, wire and cards |
| Leaseweb | ISO 27001, SOC type 1, PCI DSS in its data centres | Four continents | Dedicated, cloud, CDN, cyber security, backup, combinable into hybrid | Publishes a 99.999% core uptime figure |
| Gambitec | ISO 27001, PCI DSS; hardware RNG for GLI/iTechLabs certification | US, Europe, Canada, LatAm, Arab countries | RNGCloud/VPS, bare metal, colocation in Tier III, disaster recovery | Splits hosting by component: jackpot servers, databases, SEO/affiliate networks |
| MassiveGRID | MGA, UKGC, Gibraltar, Isle of Man technical compliance; PCI DSS Level 1; ISO 27001/9001; GDPR | 85+ metro locations | Managed active-active HA clusters, payment vault, AML tooling | Claims a 100% uptime SLA on HA clusters with zero RPO |

## Pick by what you're actually running

**Affiliate or SEO network.** You need many IPs, clean isolation between sites, and a firewall you control. The low end of the VPS range is the match; a specialist iGaming host is overkill and priced accordingly.

**Pre-licence build, staging, or a white-label demo.** Same answer. Get the architecture right on a €5–€40 box and move the production database later, when you know its real I/O profile.

**Back office, CRM, bonus engine, reporting.** These sit beside the regulated stack rather than inside it, and the compliance requirement is lighter than for the game logic. VPS, or bare metal if the reporting queries get heavy.

**MGA-licensed casino.** You need Malta-local capacity, a replication node in a second Maltese facility, and a hardware RNG path the lab will certify. Internet Vikings and Gambitec both sell pieces of that; AvaHost doesn't.

**US multi-state sportsbook.** Per-state licensed facilities, full stop. Internet Vikings builds that model state by state; a Moldovan deployment cannot substitute for it, whatever the latency graph says.

**Production platform outgrowing its VPS.** Move to bare metal on the same account once sustained baseline utilisation sits near the ceiling. Spikes are a network problem; baseline is a hardware problem, and only the second one justifies the upgrade.

## Three things to settle before you pay

Ask whether your licensing jurisdiction is one the provider will serve at all. Ask who issues the RNG certificate and whether the environment supports that lab's procedure, because the answer determines whether your certification survives contact with production. Ask how traffic and resources above the plan allowance are billed, in writing, since that is where a €5 invoice becomes a €90 one during a tournament.

If your requirement list reads "isolated, fast local disk, own IP, root, five to forty euros a month, payable in crypto", AvaHost is a serious shortlist entry and the KVM VPS line is where to start.

👉 [Start with an AvaHost KVM VPS from €5.00/mo](https://my.ava.hosting/aff.php?aff=15562)
