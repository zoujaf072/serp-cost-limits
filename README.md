# serp scraper: What Pulling Google Results Actually Costs, Which Plan Fits Your Volume, and When a Free Open-Source Tool Makes More Sense

Most people searching for a serp scraper want the same thing: clean, structured search results from Google or Bing at a predictable price, without writing and maintaining their own proxy rotation, CAPTCHA solving, and HTML parsing. There are two realistic paths in 2026. One is a managed SERP API like ScraperAPI, where a single GET request returns parsed JSON and someone else handles the blocking problems. The other is a self-hosted open-source scraper, which costs nothing in subscription fees but puts all the maintenance on you.

This article covers both, but it goes deeper on the paid side with numbers that most comparison posts skip: ScraperAPI charges **25 credits per Google or Bing request**, which means the $49 Hobby plan's 100,000 credits only cover about 4,000 Google searches per month. If you don't know that before signing up, you'll budget wrong.

> **Quick take:** For rank tracking, SEO monitoring, or feeding search results into an app or AI pipeline at modest volume, ScraperAPI's Hobby plan at $49/month (100,000 credits, 20 concurrent threads) is the usual starting point — and a 7-day trial with 5,000 credits costs nothing and requires no credit card. 👉 [Check ScraperAPI's current plans and prices](https://www.scraperapi.com/pricing/?fp_ref=coupons)

If your volume is tiny and you're comfortable running code yourself, free open-source options exist — they're covered further down, and they genuinely make sense for some readers.

## Every ScraperAPI plan, as currently listed on the pricing page

All paid plans include JavaScript rendering, premium residential and mobile IPs, automatic CAPTCHA handling, JSON auto-parsing, the structured data API, DataPipeline access, and a 99.9% uptime guarantee. Annual billing drops each price by 10%. ScraperAPI marks Scaling as its "most popular" plan, which tells you roughly where its typical customer lands — somewhere above the solo-developer tier.

| Plan | Monthly | Annual (per month) | API credits | Threads | Link |
| --- | --- | --- | --- | --- | --- |
| Free | $0 | — | 1,000 | 5 | [Sign up for the free plan](https://www.scraperapi.com/signup?fp_ref=coupons) |
| Hobby | $49 | $44.10 | 100,000 | 20 | [See Hobby plan details](https://www.scraperapi.com/pricing/?fp_ref=coupons) |
| Startup | $149 | $134.10 | 1,000,000 | 50 | [See Startup plan details](https://www.scraperapi.com/pricing/?fp_ref=coupons) |
| Business | $299 | $269.10 | 3,000,000 | 100 | [See Business plan details](https://www.scraperapi.com/pricing/?fp_ref=coupons) |
| Scaling | $475 | $427.50 | 5,000,000 | 200 | [See Scaling plan details](https://www.scraperapi.com/pricing/?fp_ref=coupons) |
| Professional | $975 | $877.50 | 10,500,000 | 300 | [See Professional plan details](https://www.scraperapi.com/pricing/?fp_ref=coupons) |
| Advanced | $1,975 | $1,777.50 | 21,500,000 | 500 | [See Advanced plan details](https://www.scraperapi.com/pricing/?fp_ref=coupons) |
| Enterprise | Custom | — | 22,000,000+ | 500+ | [Contact sales for Enterprise](https://www.scraperapi.com/contact-sales/?fp_ref=coupons) |

Two plan-level details worth flagging now: Hobby and Startup can only geotarget to **US and EU regions**, while Business and above unlock country-level targeting across 50+ countries. And Hobby/Startup analytics dashboards only keep the last 30 days of data.

## Why scraping Google SERPs is harder than scraping a normal page

A serp scraper has three jobs that a plain HTTP client can't do alone.

First, Google aggressively flags automated traffic. You need a rotating pool of residential and mobile IPs — ScraperAPI advertises more than 40 million IPs across residential, datacenter, and mobile networks in over 50 countries — plus automatic CAPTCHA handling. When a target sits behind Cloudflare, DataDome, PerimeterX, or Akamai, ScraperAPI's bypass mechanisms kick in automatically (at an extra credit cost, covered below).

Second, modern result pages are JavaScript-heavy. ScraperAPI's `render=true` parameter spins up a headless browser so you get the fully loaded page, not an empty shell.

Third, raw HTML is useless until it's parsed. This is where a managed service earns its keep: ScraperAPI has structured data endpoints for Google Search and Google Shopping that return parsed JSON directly, so you don't maintain a parser that breaks every time Google tweaks its markup. Amazon product and search pages and Walmart work the same way.

In practice, one request looks like this: you send a GET with your API key and a target URL, optionally adding parameters. Several parameters cost nothing extra — `country_code`, `session_number`, `device_type`, `output_format`, `autoparse`, `keep_headers`, and `wait_for_selector` are all free. That means geotargeting your searches to a specific country doesn't change the price at all.

## The credit math most comparison posts get wrong

Here's the part that trips people up, and it's documented but buried: **ScraperAPI doesn't charge 1 credit per page.** Credit cost depends on the domain you hit.

| Target type | Credits per request |
| --- | --- |
| Normal website | 1 |
| E-commerce (Amazon, Walmart, eBay) | 5 |
| SERP (Google, Bing, all subdomains) | 25 |
| LinkedIn | 30 |

Google and Bing are deliberately priced at 25 credits because they're the hardest targets to scrape reliably. It's not a hidden fee — the pricing page states it plainly — but plenty of buyers see "100,000 credits" and assume 100,000 pages. For a serp scraper, the real number is 4,000 searches on Hobby, 40,000 on Startup, and 120,000 on Business.

Add-ons stack on top:

- `render=true`: +10 credits
- `premium=true`: +10 credits
- `ultra_premium=true`: +30 credits (paid plans only)
- Any anti-bot bypass (Cloudflare, DataDome, PerimeterX, Akamai): +10 credits, triggered automatically
- `premium=true` + `render=true` together: +25 credits — note that's not 10+10; combined parameters have their own rates
- `ultra_premium=true` + `render=true`: +75 credits

Two tools keep this under control. The dashboard's Domain Cost Estimator shows the exact credit cost of any URL before you run a job, and you can set a `max_cost` per scrape so a single request can't blow past your limit. There's also a programmatic endpoint that returns the cost of any URL with any parameter combination, and every API response includes a `sa-credit-cost` header telling you what that request actually consumed.

Translated into money, a Google SERP request costs roughly this per thousand requests, depending on plan:

| Plan | SERP (25 credits) per 1,000 requests |
| --- | --- |
| Hobby | $12.25 |
| Startup | $3.73 |
| Business | $2.49 |
| Scaling | $2.38 |

One caveat for heavier use: the no-code DataPipeline tool uses its own credit table, and SERP requests there cost 30 credits instead of 25. Basic normal requests jump from 1 to 6 credits in DataPipeline — a 6× markup that's documented but easy to miss. If you're writing your own code against the standard API, this doesn't apply.

👉 [See the full plan list and current prices](https://www.scraperapi.com/pricing/?fp_ref=coupons)

## Limits you'll only find in the fine print

Four rules shape what happens after you sign up, and none of them appear on the headline pricing cards.

**Credits expire every billing cycle.** Unused credits don't roll over. If your scraping is spiky — say you run rank checks on the 1st of the month and nothing else — you're paying for capacity you may not use evenly.

**Running out of credits pauses you by default — but there are two opt-in ways around it, and they apply to different plans.** Auto-upgrade is available on Hobby, Startup, Business, Scaling, and Professional: when credits hit zero, your account automatically moves to the next plan (opt-in, so turn it on before you need it). Pay-as-you-go is only available from Scaling upward — there, you keep scraping past your limit at a fixed per-credit rate, up to a monthly spending cap you set. On Hobby through Business, there is no pay-as-you-go option: once credits run out and auto-upgrade is off, scraping pauses until renewal or an upgrade.

**Concurrency is hard-capped.** Exceeding your thread limit returns a 429 error. Requests are refused, not queued, and you're not charged for them — but a script that ignores this will drop results silently.

**Billing only counts successes, mostly.** You're charged for requests returning 200 or 404, and for requests you cancel before giving the API 70 seconds to finish. Failed requests — the ones where ScraperAPI retried through different IPs and eventually gave up — don't cost you credits. For a serp scraper, that's meaningful protection, since Google failure rates are exactly what you're paying someone else to absorb.

One more practical limit: sticky sessions via `session_number` last around an hour, and you can't pin a specific IP or ASN. For rank tracking and result collection this doesn't matter. For long-running logged-in workflows or ad verification, it's a dealbreaker, and a dedicated residential or mobile proxy provider would fit better.

## How to actually get started

Setup is genuinely short, which is one thing users consistently praise — Capterra gives it a 4.9/5 on ease of use, and a Latenode community review put it as "Super easy to set up. You can start scraping in minutes."

1. Sign up. The free plan gives 1,000 credits with 5 concurrent connections, no credit card needed.
2. Grab your API key from the dashboard.
3. Test your target URLs in the API Playground first — it shows both the results and the exact credit cost per request before you commit to a plan size.
4. For SERP data specifically, point requests at Google with `autoparse=true` and optionally `country_code` to target a specific market.
5. Check the Domain Cost Estimator, size your plan against real per-request costs (25 credits for Google, remember), then subscribe.

The 7-day trial is the safest way to do this: 5,000 credits, no card required, enough to run a real sample of your actual workload rather than trusting anyone's math — including this article's. No refund policy is confirmed in the published materials, so it's worth completing your cost validation within the trial window before committing to a paid plan. 👉 [Start the 7-day free trial with 5,000 credits](https://www.scraperapi.com/signup?fp_ref=coupons)

## Free and paid alternatives, for context

A serp scraper doesn't have to be a paid API, and depending on your situation these alternatives are worth knowing about. None of them are linked here — this is just the lay of the land.

- **Self-hosted open-source scrapers** (projects like Sift or MCP-based SERP extractors on GitHub) pull Google and Bing results with organic listings, ads, featured snippets, and people-also-ask data, at zero subscription cost. The trade: you run and maintain them, and Google changes its markup often enough that parsing is a recurring chore. Best for developers with time and low volume.
- **Apify** runs a Google SERP Scraper as a hosted actor on its platform — a similar managed model with per-actor pricing I couldn't verify precisely, so I won't quote numbers.
- **Scrappa** is a newer entrant offering 500 free credits monthly, pay-as-you-go credit packs, and 80+ structured endpoints across Google, YouTube, LinkedIn, and Trustpilot.
- **Infatica** sells a SERP scraper API positioned on speed and reliability, again as a commercial alternative.

Where ScraperAPI clearly leads is the structured data layer: Google Search, Google Shopping, Amazon, and Walmart endpoints return parsed JSON maintained by the vendor. Open-source scrapers give you HTML you parse yourself. That maintenance difference is the real product here.

## What actual users say

Feedback clusters into two camps, and both are worth hearing before you pick a plan.

On the positive side, users on G2 and Capterra report it "works great for Amazon/Google" — exactly the well-supported targets where the structured endpoints do the heavy lifting. Support gets called responsive, with multiple reports of the team crediting back requests that were billed in error.

The complaints concentrate on pricing surprises. A Capterra review from February 2025 (John S., Founder) reads: "Breakdown of credit costs can be confusing." A Reddit user reported being quoted $3,600 for 60 million credits, assuming Amazon requests at 1 credit each — then billed at the 5-credit e-commerce rate, meaning the plan actually covered only about 12 million requests, an 80% shortfall, with no upfront disclosure. Another recurring theme: at scale, some users found building their own infrastructure more cost-effective than a managed API.

None of this contradicts the published credit table. It just means you should do the multiplier math before buying, not after. That's what the Domain Cost Estimator and the trial are for.

## Quick answers to common questions

**How many Google searches does each plan actually cover?** Divide credits by 25. Hobby: 4,000/month. Startup: 40,000. Business: 120,000. Scaling: 200,000. If your rank tracker checks 500 keywords daily across one search engine, Hobby covers it with headroom.

**Does the free trial require a credit card?** No. It's 5,000 credits over 7 days, and you can also register for the standing free plan (1,000 credits, 5 concurrent connections) without one.

**Can I cancel anytime?** Yes — cancel from your dashboard or by contacting support, and you won't be charged for cancelling. If you're unsure about long-term fit, stay on the free trial as long as possible before subscribing.

**Does geotargeting cost extra?** No. `country_code` is one of the zero-cost parameters. Only Hobby and Startup are limited to US/EU regions, though — country-level targeting worldwide starts at the Business plan.

**Do failed scrapes cost credits?** Requests that end in failure after ScraperAPI's automatic retries don't count. You pay for 200s, 404s, and requests you cancel before the 70-second window closes.

## Picking a plan, concretely

Match your situation to a plan and don't overbuy:

- **Under ~4,000 Google requests a month, one or two countries, want zero maintenance:** Hobby at $49/month, or $44.10 on annual billing.
- **Rank tracking or monitoring at 10,000+ searches monthly, or need country-level targeting beyond US/EU:** Business at $299/month — the per-1,000 SERP cost drops to $2.49 versus Hobby's $12.25, so the jump pays for itself if you'll use the volume.
- **Heavy jobs across many targets with premium rendering:** test the real per-request cost first; at 75 credits for ultra-premium plus rendering, the effective price per thousand pages climbs fast, and a bandwidth-billed residential proxy may undercut it.
- **Very low volume and happy to maintain code yourself:** a free open-source scraper costs nothing and may be all you need.

Before you subscribe, run one number through the calculator: your expected monthly SERP requests × 25 credits, plus any render or bypass add-ons you'll actually use. If that fits comfortably inside a plan's allowance, you're set. 👉 [Pick your ScraperAPI plan and start scraping](https://www.scraperapi.com/pricing/?fp_ref=coupons)
