# Web Scraping Techniques: Static vs. Dynamic

A robust research agent needs to handle different types of websites. This guide compares the two main techniques used in GPT Researcher.

## 1. Static Scraping (BeautifulSoup)
Best for pages where the content is present in the initial HTML source code.

- **Pros:** Extremely fast, low resource usage, easy to implement.
- **Cons:** Cannot execute JavaScript, fails on Single Page Applications (SPAs) or content loaded via AJAX.
- **Tools:** `requests`, `BeautifulSoup4`.

## 2. Dynamic Scraping (Selenium / nodriver)
Required for modern websites that load content dynamically via JavaScript.

- **Pros:** Can interact with the page (click, scroll, wait), handles SPAs, can bypass some bot detections.
- **Cons:** Slower, resource-intensive (requires a browser engine), more complex setup.
- **Techniques:**
    - **Scrolling:** Many sites load content as you scroll. A dynamic scraper can automate this.
    - **Waiting:** Use `WebDriverWait` to wait for specific elements to appear before scraping.
    - **Cookie Handling:** Some sites require cookies or "visiting" a home page first.

## 3. Advanced Extraction Services
For high-scale or difficult-to-scrape sites, consider specialized extraction APIs:
- **Tavily Extract:** Optimized for AI agents.
- **Firecrawl:** Handles complex web-to-markdown conversion.

## How to Choose?
1. **Try Static First:** It's faster and cheaper.
2. **Check for "Empty" Responses:** If a static request returns a page with no content, it's likely JS-heavy.
3. **Use Dynamic for Interactive Sites:** If you need to click buttons or scroll to see content, use a browser-based scraper.
4. **Fallback Strategy:** Implement a system that tries a fast scraper first and falls back to a more robust one if the content length is too short.
