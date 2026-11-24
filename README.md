# MT4 VPS: How much RAM per terminal, which plan is enough, and when a free broker VPS beats paying (all 11 Evoxt plans compared)

Most MT4 setups need one VPS and it is not the expensive one. On Evoxt's current price list, **VM-1 at $5.99/month** (1 core, 2 GB RAM, 20 GB storage) runs one or two terminals with a few EAs each. Step up to **VM-2 at $11.99** (2 cores, 4 GB, 30 GB) when you add accounts or a trade copier. Everything below shows how those two numbers come out of the RAM-per-terminal maths, walks through all eleven plans, covers the city question that decides your latency, and explains when a broker's free VPS is genuinely enough.

> **Quick pick**
> - One or two MT4 accounts with ordinary EAs: **VM-1, $5.99/mo** (1 core, 2 GB RAM, 20 GB).
> - Three to six accounts, a copier, indicator-heavy templates: **VM-2, $11.99/mo** (2 cores, 4 GB, 30 GB). Take VM-4 at $23.99 only if you want 8 GB in hand.
> - Skip VM-0.5 and VM-0.75 (512 MB and 1 GB). Windows plus a terminal leaves nothing spare.
>
> 👉 [See all 11 Evoxt plans and current prices](https://bit.ly/EvoXt)

## All Evoxt plans, with an MT4 terminal estimate

Evoxt prices in USD and bills monthly up to three years, with account credit you can top up for future invoices. Every plan includes weekly offsite backup at no extra cost, and you can add RAM, cores or storage to a running VM without moving to a different plan.

| Plan | CPU / RAM / Storage | MT4 terminals (estimate) | Price | Order |
| --- | --- | --- | --- | --- |
| VM-0.5 | 1 core / 512 MB / 5 GB | Not for MT4 | $2.99/mo | [Deploy VM-0.5](https://bit.ly/EvoXt) |
| VM-0.75 | 1 core / 1 GB / 10 GB | Not for MT4 | $4.99/mo | [Deploy VM-0.75](https://bit.ly/EvoXt) |
| VM-1 | 1 core / 2 GB / 20 GB | 1–2 | $5.99/mo | [Deploy VM-1 at $5.99](https://bit.ly/EvoXt) |
| VM-1.5 | 2 cores / 2 GB / 20 GB | 1–2, more CPU headroom | $6.95/mo | [Deploy VM-1.5](https://bit.ly/EvoXt) |
| VM-2 | 2 cores / 4 GB / 30 GB | 3–6 | $11.99/mo | [Deploy VM-2 at $11.99](https://bit.ly/EvoXt) |
| VM-3 | 4 cores / 4 GB / 30 GB | 3–6, more CPU headroom | $14.99/mo | [Deploy VM-3](https://bit.ly/EvoXt) |
| VM-4 | 4 cores / 8 GB / 60 GB | 8–12 | $23.99/mo | [Deploy VM-4 at $23.99](https://bit.ly/EvoXt) |
| VM-6 | 8 cores / 8 GB / 60 GB | 8–12, more CPU headroom | $29.99/mo | [Deploy VM-6](https://bit.ly/EvoXt) |
| VM-8 | 8 cores / 16 GB / 80 GB | 16–20 | $47.99/mo | [Deploy VM-8](https://bit.ly/EvoXt) |
| VM-12 | 16 cores / 16 GB / 80 GB | 16–20, more CPU headroom | $60.95/mo | [Deploy VM-12](https://bit.ly/EvoXt) |
| VM-16 | 16 cores / 32 GB / 100 GB | 30+ | $95.99/mo | [Deploy VM-16](https://bit.ly/EvoXt) |

Monthly transfer runs from 250 GB on the two smallest plans up to 5,000 GB on VM-16, and it scales with the plan rather than with terminal count, so it rarely decides anything for a trading box. The terminal estimates are mine, worked out from the rule in the next section, not something Evoxt publishes.

## How much RAM does one MT4 terminal actually need?

Plan on **0.5 to 1 GB of RAM per terminal**. A terminal with four or five charts and an EA or two settles at a few hundred megabytes; indicator-heavy templates, long chart histories and tick-level EAs push it towards 1 GB. MT4 is a 32-bit application, and each chart's code runs on a single thread, so a live terminal spends most of its time waiting for ticks rather than burning CPU. Memory, not cores, is the resource you run out of first.

That has three consequences for plan choice:

- **512 MB and 1 GB are too small.** Windows itself plus Remote Desktop consumes most of a 1 GB box before MT4 starts, and Evoxt sells those two plans for small Linux jobs. VM-1 at 2 GB is the practical floor for a trading terminal.
- **Extra cores are cheap insurance, not a requirement.** VM-1.5 costs $0.96/month more than VM-1 for the same 2 GB and a second core. Worth it if you plan to run two terminals side by side, because each terminal is a separate process and can use a second core. The RAM still caps you at two terminals.
- **Storage is almost never the constraint.** A terminal with history is small next to 20 GB. VM-4's 60 GB matters if you keep years of tick data or several copies of the platform for testing.

For reference, the same ladder exists at other hosts at very different prices: MassiveGRID's 1 vCPU / 2 GB entry tier starts at $2.87/month and its 4 vCPU / 8 GB tier is the one it recommends for three to eight terminals, while ForexVPS starts at $28.80/month across 21 locations. Those are different resource tiers with different support and location coverage, so the headline numbers are not a like-for-like comparison. A dedicated-core tier with a Windows Server licence included, such as MassiveGRID's Forex VPS Pro from $9.73/month, is the next step up if a neighbour's backtest disturbing your fills is a real risk for your strategy.

## Which city should the VPS sit in?

Pick the city where your broker's trade servers sit, not the city nearest to you. Your order travels from the VPS to the broker's server, so the distance that matters is that one.

Broker server locations published by the brokers themselves, checked in October 2026: IC Markets, FP Markets, Axi and Global Prime name Equinix NY4; FxPro, Admirals, Darwinex and BlackBull name LD4 or LD5 in London; FXTM names Frankfurt. Brokers move servers and some run different entities in different cities, so confirm before ordering. The quickest check is inside MT4 itself: the login dialog shows the ping next to each server name.

Evoxt's pricing page separates Standard regions from a Premium Network that lists Hong Kong and Japan (Osaka). Whatever region you choose, check the monthly transfer figure shown for that region on the plan you pick, because the same plan name can carry a different allowance depending on where the VM sits.

Treat latency numbers from any hosting provider as self-reported. Evoxt does not publish a latency table for MT4, and the figures you see elsewhere (under 1 ms inside the same datacenter campus, 0–10 ms London to LD4) come from vendor pages with different measurement methods. QuantVPS itself notes that its 0–10 ms figure is network latency and that fills and slippage still depend on the broker, venue, order type and market conditions. Do not rank providers on those numbers.

## Is a free broker VPS enough?

Sometimes, and the conditions are narrower than the marketing suggests. tastyfx offers a VPS hosted by Beeks in London with 30 GB disk and 2,560 MB RAM, free while you hold a $2,000 account balance and $50/month below that. IG's version, also Beeks in London, gives 25 GB and 1,300 MB, free at a £5,000 balance and £50/month otherwise. Both pages are undated. Other brokers gate the free VPS behind monthly trading volume instead of balance, usually on a small shared instance in one location.

Three things decide it:

Free tiers are tied to one broker. If you run accounts at two or three brokers, or mirror trades with a copier, you need a box that logs into all of them.
Free tiers give you little control. You take the location you are given, you typically do not get full administrator rights, and the instance can be withdrawn when your volume drops.
The fallback charge is steep. $50 or £50 a month is a lot next to a $5.99 VM, so the free VPS is only free while you keep the balance or the volume.

If your only account is at the broker offering the VPS and your EAs are light, take the free one. If you want the city your broker actually sits in, or more than one broker on the same machine, a paid VM is cheaper than the non-qualifying monthly charge.

👉 [Check the VM-1 price and region list on Evoxt](https://bit.ly/EvoXt)

## Setup mistakes that quietly stop an EA

These cost people money far more often than a slow server does.

1. **Disconnect, do not sign out.** Closing the Remote Desktop window leaves the Windows session and MT4 running. Choosing Sign out from the Start menu ends the session and closes your terminals.
2. **Make terminals survive a reboot.** Put a shortcut to each terminal in the Windows startup folder and tick "Keep personal settings and data at startup" inside MT4, so every terminal comes back logged in after maintenance or a host-side restart.
3. **One folder per account.** A terminal logs into one account only, so three accounts means three terminals. Install each in its own folder, or launch a copy with the `/portable` switch so its data stays beside the executable.
4. **Download MT4 from your broker.** MetaQuotes no longer distributes MT4 from its own site; the installer you need is the broker-branded build that carries that broker's server list.
5. **Leave Windows on UTC.** The clock in MT4 is your broker's server time, not the VPS clock, so a New York VM still shows a GMT+2 or GMT+3 chart correctly. Changing the Windows timezone gains nothing and confuses your logs.
6. **Do not buy cores for backtesting.** The MT4 Strategy Tester runs a single backtest on one thread, so extra vCPUs do not make one test finish sooner. They let you run several tests or terminals at once. Heavy optimisation belongs on MT5, which uses every core, or on a separate machine, because a backtest competes with your live terminals for CPU.

## Prepaying, uptime claims and refunds

Evoxt lets you prepay from one month up to three years and accepts cards, PayPal, Bitcoin and USDt Tron. For a first deployment, buy a month. One month at $5.99 tells you more about ping, reboot behaviour and how your EA handles a fresh session than any review will, and you keep the option to move.

Uptime promises come in two flavours and they are not equivalent. Some hosts advertise "100% uptime" as a slogan; others publish a number plus a compensation table. QuantVPS, for example, advertises a 99.999% monthly SLA with a 10% service credit for any covered outage over one minute, and a 14-day money-back window, while MassiveGRID advertises a 100% uptime SLA with a 15-day money-back guarantee. Read the credit terms, not the headline percentage, and read the refund clause before you prepay for a year. Evoxt's own refund terms are the ones that apply to your order, so check them on the site before paying.

👉 [Start with a single month on Evoxt and test the ping yourself](https://bit.ly/EvoXt)

## FAQ

**Can I run MT4 on a Mac with a VPS?**
Yes. The terminal runs on the Windows server and you connect with the Microsoft Remote Desktop client for macOS, so every indicator and EA works as it would on a PC. Your phone can watch the same account through the MT4 mobile app while the EAs keep running on the server.

**How many MT4 terminals can one VPS run?**
As many as your RAM allows at 0.5–1 GB each, plus Windows overhead. On Evoxt's line-up that means one or two on VM-1's 2 GB, three to six on VM-2's 4 GB, and eight to twelve on VM-4's 8 GB. CPU becomes the limit only when several terminals and a backtest run simultaneously.

**Will more vCPUs speed up my backtest?**
No. One MT4 backtest uses one thread. More cores let you run more tests in parallel or keep more terminals responsive.

**What time does MT4 show on a VPS?**
Your broker's server time. The Windows clock is irrelevant to the charts, which is why leaving it on UTC is the least confusing option.

**Is Windows included?**
Evoxt lists Windows RDP among its available operating systems. Confirm the Windows line item at checkout so the monthly figure you budget for matches what you are actually billed.

## What to order

Run one account with a couple of EAs and you are done at **VM-1, $5.99/month**. Two terminals that need to stay responsive, or a second core for comfort, make **VM-1.5 at $6.95** the better buy. Three to six accounts, a copier, or templates loaded with indicators take you to **VM-2 at $11.99**, and a desk running eight or more terminals belongs on **VM-4 at $23.99** or VM-8 at $47.99.

Two things to confirm before you pay: that Evoxt has a region in or near your broker's server city (check the ping in the MT4 login dialog first), and what the Windows option adds to the monthly total. Those two answers decide whether the cheapest suitable plan is actually the cheapest suitable plan for your setup.

👉 [Compare the Evoxt plans and pick your region](https://bit.ly/EvoXt)
