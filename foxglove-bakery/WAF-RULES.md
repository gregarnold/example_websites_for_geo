# Foxglove Bakery: Cloudflare WAF Rules

Domain: foxglovebakery.com
Purpose: Block AI crawlers and automated scanners so GeoScored produces a WAF-blocked scan report.
Cloudflare Plan: Free (5 custom WAF rules maximum)

## Rule 1: Block AI Crawler User Agents

- **Rule name:** Block AI Crawlers
- **When:** `(http.user_agent contains "GPTBot") or (http.user_agent contains "ChatGPT-User") or (http.user_agent contains "Claude-Web") or (http.user_agent contains "anthropic-ai") or (http.user_agent contains "Google-Extended") or (http.user_agent contains "PerplexityBot") or (http.user_agent contains "CCBot")`
- **Then:** Block
- **Priority:** 1

## Rule 2: Block Common Bot User Agents

- **Rule name:** Block Common Bots
- **When:** `(http.user_agent contains "Bytespider") or (http.user_agent contains "PetalBot") or (http.user_agent contains "Amazonbot") or (http.user_agent contains "Applebot-Extended") or (http.user_agent contains "Scrapy") or (http.user_agent contains "wget") or (http.user_agent contains "curl")`
- **Then:** Block
- **Priority:** 2

## Rule 3: Block Scanner-Like User Agents

- **Rule name:** Block Scanners
- **When:** `(http.user_agent contains "python-requests") or (http.user_agent contains "httpx") or (http.user_agent contains "aiohttp") or (http.user_agent contains "Go-http-client") or (http.user_agent contains "Java/") or (http.user_agent contains "libwww-perl")`
- **Then:** Block
- **Priority:** 3

## Rule 4: Challenge Empty or Missing User Agents

- **Rule name:** Challenge Empty UA
- **When:** `(http.user_agent eq "")`
- **Then:** Managed Challenge
- **Priority:** 4

## Rule 5: Block Known Headless Browsers

- **Rule name:** Block Headless Browsers
- **When:** `(http.user_agent contains "HeadlessChrome") or (http.user_agent contains "PhantomJS") or (http.user_agent contains "Selenium")`
- **Then:** Block
- **Priority:** 5

## Additional Cloudflare Settings

These are not custom rules but Cloudflare dashboard settings to enable:

- **Bot Fight Mode:** Enabled (Security > Bots)
- **Security Level:** High (Security > Settings)
- **Browser Integrity Check:** On (Security > Settings)

## Expected Behavior

When these rules are active:
- Normal browsers (Chrome, Firefox, Safari, Edge) can access the site without issue.
- GeoScored's scanner (which uses headless browsers and/or custom user agents) receives a 403 Forbidden or Cloudflare challenge page.
- The scan result will be classified as "WAF-blocked" in GeoScored's report system.
- AI crawlers (GPTBot, Claude-Web, etc.) are blocked outright.

## Setup Instructions

1. Log into Cloudflare dashboard for foxglovebakery.com
2. Navigate to Security > WAF > Custom rules
3. Create each rule using the expressions above
4. Enable Bot Fight Mode under Security > Bots
5. Set Security Level to High under Security > Settings
6. Verify by testing with curl: `curl -I https://foxglovebakery.com` (should be blocked by Rule 2 or 3)
7. Verify normal browser access works by visiting the site in Chrome

## Notes

- The free Cloudflare plan allows exactly 5 custom WAF rules, which is why we use all 5.
- Rules are evaluated in priority order. A request matching Rule 1 is blocked before Rules 2-5 are checked.
- The `robots.txt` file also disallows AI crawlers, but WAF rules enforce the block at the network level.
- This configuration is deliberately aggressive. A real small bakery might have similar settings if their hosting provider (or a well-meaning friend) enabled Cloudflare's security features without understanding the tradeoffs for AI visibility.
