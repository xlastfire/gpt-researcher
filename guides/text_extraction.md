# Text Extraction Procedures

This guide outlines the procedures for extracting high-quality text content from web pages, based on the techniques used in GPT Researcher.

## 1. Fetching the HTML
The first step is to obtain the raw HTML content of the target URL. This can be done using:
- **Static Request:** Using libraries like `requests` for simple, fast fetching.
- **Dynamic Rendering:** Using tools like `Selenium` or `nodriver` for pages that require JavaScript to load content.

## 2. Cleaning the HTML (Soup Preparation)
Raw HTML often contains "noise" that isn't relevant to the main content and can confuse LLMs.
- **Remove Unwanted Tags:** Decompose elements like `<script>`, `<style>`, `<nav>`, `<footer>`, `<header>`, `<svg>`, `<menu>`, and `<sidebar>`.
- **Remove by Class/ID:** Target elements with class names or IDs that suggest non-content areas (e.g., `class="nav"`, `class="footer"`).

Example implementation:
```python
def clean_soup(soup):
    for tag in soup.find_all(["script", "style", "footer", "header", "nav", "menu", "sidebar", "svg"]):
        tag.decompose()
    return soup
```

## 3. Extracting Text
Once the soup is cleaned, extract the text while maintaining some structure.
- **Structural Separation:** Use separators (like `\n`) when getting text to prevent words from different blocks (like a header and a paragraph) from merging.
- **Whitespace Normalization:** Use regular expressions to collapse multiple spaces or newlines into single ones for a cleaner output.

Example implementation:
```python
import re

def get_text_from_soup(soup):
    text = soup.get_text(strip=True, separator="\n")
    # Remove excess whitespace
    text = re.sub(r"\s{2,}", " ", text)
    return text
```

## 4. Extracting Metadata
Don't forget to capture essential metadata like the page title, which provides context for the extracted text.
```python
def extract_title(soup):
    return soup.title.string if soup.title else ""
```
