# Web Scraping Best Practices

To build a reliable and ethical deep research agent, follow these best practices for web scraping.

## 1. Respect `robots.txt`
Always check the `robots.txt` file of a website (e.g., `https://example.com/robots.txt`) to see which parts of the site are off-limits for crawlers.

## 2. Use Realistic User Agents
Websites often block requests that use default library user agents (like `python-requests/2.x`). Use a modern browser user agent string to appear as a regular user.

```python
headers = {
    "User-Agent": "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/119.0.0.0 Safari/537.36"
}
```

## 3. Handle Timeouts and Retries
Network requests can fail or hang. Always set a `timeout` and implement retry logic for transient errors.

## 4. Throttle Your Requests
Don't overwhelm a server with hundreds of requests per second. Implement delays or use a worker pool with a concurrency limit.

## 5. Handle Cookies and Sessions
Maintain a session (`requests.Session()`) to handle cookies across multiple requests to the same site. This is often necessary for sites that use cookies for bot protection or session management.

## 6. Ethical Data Usage
- Only scrape what you need.
- Don't scrape personal or sensitive data.
- Credit the source when using scraped content in your agent's output.

## 7. Error Handling
Wrap your scraping logic in try-except blocks to prevent a single failing URL from crashing your entire research process. Log the errors for debugging.
