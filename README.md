# tiktok residential proxy: which IPs actually keep accounts alive, how to set up sticky sessions, and what it costs

TikTok is not hard to reach. It's hard to reach *repeatedly* from the same address without something noticing.

Most people searching for a TikTok residential proxy have already run into one of three walls: a login loop that keeps demanding phone verification, a cluster of accounts that all died the same week, or a scraper that returns empty responses after two hundred requests. None of those are content problems. They're IP problems, and they show up before you ever post anything.

This piece walks through what TikTok actually evaluates at the network layer, what residential IPs do and don't fix, how to configure sticky sessions so an account keeps a consistent identity, and what 9Proxy's current plans cost if you go that route.

---

## TikTok scores the connection before it scores the account

TikTok doesn't see "a user." It sees a set of signals it weighs against each other — IP reputation, IP history, device fingerprint, timezone, language, and how often any of those change. An account looks suspicious when those signals disagree, not when one of them is unusual.

Two failure modes account for most of the trouble:

**Datacenter IPs.** Ranges owned by AWS, DigitalOcean, or any hosting provider are catalogued and sold as proxy infrastructure. A request from those ranges is easy to identify before it hits the application layer. Free proxy lists are almost entirely datacenter IPs, and because they're free, thousands of people push the same addresses through them. Reusing one gets you flagged by association, not by behavior.

**Location mismatch.** If your browser profile says New York, your timezone says EST, your content preferences are US, and your IP resolves to Frankfurt, you've described an impossible user. TikTok resolves that inconsistency the same way an anti-fraud system resolves any inconsistency: with friction.

Then there's the loop. When an IP gets tagged for excessive registration or a location mismatch, TikTok starts asking for verification on every login. The account isn't banned. It's just unusable, and no amount of retrying fixes an IP problem.

## Residential, mobile, and datacenter IPs on TikTok

| IP type | How TikTok tends to treat it | Where it works | Where it breaks |
| --- | --- | --- | --- |
| Datacenter | Identified as hosting infrastructure; frequently pre-flagged | Internal testing, non-social automation | Account creation, logins, anything authenticated |
| Residential | Reads as a real home connection; trust depends on how clean the IP is | Multi-account management, scraping, geo-verification, browser-based workflows | Can be slower than datacenter IPs; quality varies a lot by provider |
| Mobile (carrier) | Highest trust — carriers use CGNAT, so hundreds of real users share one IP and blocking it hits legitimate customers | App-level activity, high-risk account creation | Cost, and availability through a normal proxy subscription |

Residential is the practical middle. Mobile is stronger on paper, but it's a different budget and a different supply chain — worth knowing if you're weighing a provider whose product line stops at residential, which is the case for 9Proxy.

> The honest summary: residential IPs solve the "obviously a server" problem. They do not solve the "twelve accounts on one address" problem. That one is on you.

## The rule that matters more than the proxy type: one account, one IP

Ask anyone who has run TikTok accounts at any volume and you get the same answer — one static or long-lived residential IP per account, not two or three.

The reasoning isn't technical limits, it's risk concentration. If five accounts share an exit IP, TikTok groups them as one source. When one gets actioned, the others inherit the suspicion. Multi-account setups that survive are the ones where each account has its own IP, its own fingerprint environment, and a stable location that never jumps.

That's what a sticky session is for. Rotating IPs are correct for scraping, where every request should look unrelated. They're wrong for logins, where the same identity needs the same address across hours of activity. A good residential provider lets you choose per endpoint: sticky, rotating, or sticky with a per-instance session ID so you can pull several different fixed IPs from one configuration.

The practical version of this on 9Proxy is a structured username rather than a separate control panel. Country, state, city, ISP, session duration, and session ID are all embedded in the credential string:


subaccount-country-us-st-ohio-sst-15-ssid-device1


`sst-15` holds the IP for 15 minutes; a different `ssid` value returns a different IP with the same geographic settings. Both rotating and sticky modes run over HTTP/HTTPS and SOCKS5, which is what antidetect browsers and scripted automation actually need.

## Country-level targeting is not enough anymore

For TikTok specifically, city and ISP targeting matter more than they do on most platforms, for one reason: content and monetization are region-gated.

TikTok Shop affiliate programs, Creator Rewards, and even the shape of the For You feed behave differently per market. If you're validating what a US audience sees, a US IP from a random state is a weak approximation — your locale, timezone, and language signals have to line up with the same city the IP resolves to. Providers that filter down to city, ZIP, and ISP let you build a coherent identity instead of just a plausible one.

Cost-wise, more filtering narrows the pool. Targeting by country alone gives you the fastest pool and the most IPs to draw from. Adding state, city, and ISP simultaneously shrinks what's available. Worth knowing before you blame the provider for slow responses on a triple-filtered endpoint.

## What to check before you buy a TikTok residential proxy

- **Pool size and refresh rate.** A large pool is only useful if the addresses are cleaned. Ask how the provider handles IPs that get flagged.
- **Targeting depth.** Country only is not enough for TikTok Shop or Creator Rewards work. City, state, ZIP, and ISP filtering is the real bar.
- **Sticky session length.** Minutes are fine for logins. If you need a single identity alive for hours, check the maximum session duration before you pay.
- **Session IDs for parallel use.** Without them, one configuration gives one IP, and parallel accounts collide.
- **Bandwidth model.** Per-GB billing is cheap for light requests, expensive for video-heavy work. Per-IP with unlimited traffic is cheaper for sustained sessions and unpredictable data use.
- **Protocol support.** SOCKS5 for antidetect browsers, HTTP/HTTPS for scripts.
- **Refund terms.** Read them. This is where the complaints usually live.

## Where 9Proxy fits

9Proxy is a residential-only network: 20M+ IPs across 90+ countries, with targeting down to country, state, city, ZIP, and ISP level. No datacenter, ISP, or mobile product line, so if you specifically need carrier IPs, this isn't the provider — that's a real limitation, not a footnote.

What it does offer is a pricing model that suits account work better than straight per-GB billing. The IP-based plans charge per address with unlimited bandwidth on each one, which means a TikTok session can run as long as you like without a data meter ticking in the background. The GB-based plans flip that around and charge for traffic, with no fixed IP lifetime — better for rotation-heavy scraping and geo-checking where each request is small.

A few details that are genuinely useful for this use case, rather than generic features:

- **Today List.** Proxies used in the last 24 hours are reusable at no extra cost. For recurring TikTok tasks where you want the same geography twice, that's a direct cost reduction.
- **Sub-users.** Each sub-user has its own credentials and optionally its own traffic allocation. Useful when several people or several scripts share one balance.
- **Public API and Proxy Generator.** Programmatic endpoint generation with targeting baked in — the practical way to assign one fixed IP per account at scale.
- **60-second refund policy.** Any IP that fails to connect within the first minute is credited back. Narrow, but it covers the worst case of a dead address.
- **Crypto payment bonus.** Paying in crypto adds a 5% bonus on IPs.
- **Vendor performance figures.** 9Proxy publishes roughly 99.95% uptime and around 0.6s average response time; independent reviews report success rates in the mid-90s on harder targets. Treat the headline numbers as vendor claims and your target list as the real test.

Worth flagging: 9Proxy's acceptable use policy restricts media streaming on IP-based plans, and the refund window is genuinely narrow. Neither affects TikTok account work, but both are the kind of thing you want to know before checkout rather than after.

👉 [Check 9Proxy's residential IP plans and current pricing](https://bit.ly/9-Proxy)

## Every 9Proxy plan, as currently published

9Proxy splits its catalogue into four product lines. Prices below reflect the published structure after the pricing update that raised IP-based and bundle rates while leaving GB-based plans unchanged.

**Residential proxy by IP** — fixed number of IPs, unlimited bandwidth per IP, IPs don't expire:

| Package | Price | Effective rate | Purchase |
| --- | --- | --- | --- |
| 100 IPs | $24 | $0.24/IP | [Get 100 IPs](https://bit.ly/9-Proxy) |
| 500 IPs | $72 | $0.144/IP | [Get 500 IPs](https://bit.ly/9-Proxy) |
| 1,000 + 500 bonus IPs | $126 | $0.084/IP | [Get 1,500 IPs](https://bit.ly/9-Proxy) |
| 2,500 IPs | $210 | $0.084/IP | [Get 2,500 IPs](https://bit.ly/9-Proxy) |
| 5,000 IPs | $360 | $0.072/IP | [Get 5,000 IPs](https://bit.ly/9-Proxy) |
| 15,000 IPs | $720 | $0.048/IP | [Get 15,000 IPs](https://bit.ly/9-Proxy) |
| 25,000 IPs | $863 | $0.035/IP | [Get 25,000 IPs](https://bit.ly/9-Proxy) |
| 50,000 IPs | $1,438 | $0.029/IP | [Get 50,000 IPs](https://bit.ly/9-Proxy) |

**Business IP packages** — same residential IP quality, volume pricing for large operations:

| Package | Price | Effective rate | Purchase |
| --- | --- | --- | --- |
| 100,000 IPs | $2,300 | $0.023/IP | [Get 100,000 IPs](https://bit.ly/9-Proxy) |
| 200,000 IPs | $4,140 | $0.021/IP | [Get 200,000 IPs](https://bit.ly/9-Proxy) |
| 500,000 IPs | $8,625 | $0.018/IP | [Get 500,000 IPs](https://bit.ly/9-Proxy) |

**Residential proxy by GB** — pay per traffic, rotating or sticky, 180-day validity, unlimited endpoints:

| Package | Price | Effective rate | Validity | Purchase |
| --- | --- | --- | --- | --- |
| 5 GB | $15 | $3.00/GB | 180 days | [Get 5 GB](https://bit.ly/9-Proxy) |
| 50 + 5 bonus GB | $105 | $2.10/GB | 180 days | [Get 55 GB](https://bit.ly/9-Proxy) |
| 100 GB | $150 | $1.50/GB | 180 days | [Get 100 GB](https://bit.ly/9-Proxy) |
| 200 GB | $200 | $1.00/GB | 180 days | [Get 200 GB](https://bit.ly/9-Proxy) |
| 1,000 GB | $800 | $0.80/GB | 180 days | [Get 1,000 GB](https://bit.ly/9-Proxy) |
| 2,000 GB | $1,500 | $0.75/GB | 180 days | [Get 2,000 GB](https://bit.ly/9-Proxy) |

**Enterprise GB packages** — no expiry, team mode with one owner and up to five members, per-member traffic controls:

| Package | Price | Effective rate | Validity | Purchase |
| --- | --- | --- | --- | --- |
| 3,000 GB | $2,160 | $0.72/GB | Unlimited | [Get 3,000 GB Enterprise](https://bit.ly/9-Proxy) |
| 6,000 GB | $4,200 | $0.70/GB | Unlimited | [Get 6,000 GB Enterprise](https://bit.ly/9-Proxy) |
| 10,000 GB | $6,800 | $0.68/GB | Unlimited | [Get 10,000 GB Enterprise](https://bit.ly/9-Proxy) |

**Bundle packages** — IPs plus bandwidth for mixed workflows:

| Package | Price | Contents | Purchase |
| --- | --- | --- | --- |
| Starter | $30 | 100 IPs + 5 GB | [Get the Starter bundle](https://bit.ly/9-Proxy) |
| Popular | $180 | 1,500 IPs + 50 GB | [Get the Popular bundle](https://bit.ly/9-Proxy) |
| Pro | $720 | 5,000 IPs + 500 GB | [Get the Pro bundle](https://bit.ly/9-Proxy) |

Discount codes are issued to your account rather than published as public codes — they appear under My Coupons in the dashboard after qualifying orders, and partner channels occasionally distribute their own percentage codes. If you pay in crypto, the 5% IP bonus applies automatically.

## Which package makes sense for which TikTok workflow

**A handful of accounts, run manually.** The 100-IP package at $24 is the obvious entry point. Ten accounts on ten fixed IPs, unlimited traffic, and IPs that don't expire. You are not paying for bandwidth you won't use.

**Twenty to fifty accounts with an antidetect browser.** 500 IPs at $72, or the Starter bundle at $30 if you also want traffic for verification checks and light scraping. Pair each browser profile with one IP and one consistent city.

**Scraping TikTok data or verifying ads across markets.** GB-based. Requests are small and you want rotation, not persistence. Start at 5 GB and measure real consumption before scaling — TikTok responses are JSON-heavy, and a 5 GB package goes further than people expect if you're not pulling video files.

**Agencies and resellers.** Business IP tiers or the Enterprise GB packages. Enterprise is the only tier with team seats, unlimited validity, and per-member controls, which matters when several operators share one balance.

## Setup: from purchase to a working TikTok session

1. **Create the account and buy a package.** IP-based if you need fixed identities, GB-based if you need rotation.
2. **Open the Proxy Generator.** Choose authentication — username/password with sub-users, or IP whitelisting if you'd rather skip credentials entirely.
3. **Pick the location.** Country first, then state, city, and ISP if the market is narrow. Don't over-filter unless you have to; each additional filter shrinks the available pool.
4. **Choose the session mode.** Sticky with an `sst` value for accounts, rotating for scraping. Add a distinct `ssid` per account so parallel sessions don't collide.
5. **Test one endpoint before scaling.** Check the IP with a lookup service, then confirm the timezone and language in your browser profile match the IP's geography. Mismatches here are the most common reason a technically working proxy still gets accounts flagged.
6. **Assign, don't reassign.** Once an account is bound to an IP, keep the pairing stable. Swapping IPs mid-life is the fastest way to make a clean setup look automated.

👉 [Create a 9Proxy account and set up your first TikTok endpoint](https://bit.ly/9-Proxy)

## Problems that persist even with good IPs

**Verification loops on new accounts.** Usually IP reuse rather than IP type. Pull a fresh sticky IP in the same city and slow down account creation — fill the profile over minutes, not seconds.

**"Suspicious activity detected."** Often behavioral rather than network. High follow rates, immediate posting after signup, or identical timing across accounts will trigger it on perfect IPs.

**Slow loading.** Residential routing goes through real home connections, so latency is inherently higher than datacenter. If a single IP is slow, request another in the same city before concluding the provider is the issue.

**Accounts still linked after switching to residential.** IP is one layer. Fingerprint, cookies, and timezone are others. This is why residential proxies get paired with antidetect browsers — 9Proxy documents integrations with several, and supports SOCKS5 with user/pass or whitelisted-IP auth.

## Straight answers to the questions people actually ask

**Do I need mobile proxies for TikTok instead?** Mobile IPs carry more trust because carrier NAT means blocking them affects real subscribers. Residential works for browser-based account management and data work. If app-level activity is the core of your workflow, mobile is the stronger choice — and 9Proxy doesn't sell it.

**How many TikTok accounts can one residential IP handle?** One, if you care about the accounts. Two or three may survive short term; the link between them doesn't go away.

**Is rotating better for TikTok?** No, for anything authenticated. Rotate for scraping and geo-checks, hold sessions for accounts.

**Can I avoid the per-GB math?** On IP-based plans, yes — unlimited bandwidth per IP means bandwidth stops being a variable. That's the main argument for that model on long TikTok sessions.

**Does any of this conflict with TikTok's terms?** TikTok's terms restrict automated access and coordinated multi-account activity. That's a risk you're taking on deliberately, and it's worth being clear-eyed about rather than assuming a proxy makes it compliant.

The short version: buy per-IP if you're running accounts, buy per-GB if you're collecting data, keep one IP per account, and match your city targeting to your fingerprint. 9Proxy's pricing structure lines up well with the first two of those, and its 20M-IP residential pool covers the third. What it won't do is fix a setup where twelve profiles share one address.
