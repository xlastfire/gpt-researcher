# Image Extraction Procedures

Extracting relevant images (like featured images or content illustrations) while ignoring UI elements (like icons or ads) is crucial for a deep research agent.

## 1. Finding All Image Candidates
Start by identifying all `<img>` tags that have a `src` attribute.

## 2. Resolving URLs
Images often use relative paths. Use `urllib.parse.urljoin` to convert these into absolute URLs using the base URL of the page.

## 3. Scoring and Filtering
Not all images are equally important. Implement a scoring system to prioritize relevant images:

- **Class/ID Analysis:** Give higher scores to images with "hero", "featured", "main", or "content" in their class or ID attributes.
- **Dimension Analysis:** Larger images are more likely to be relevant content than small icons.
    - Large (e.g., > 1600x800): High priority.
    - Medium (e.g., > 800x500): Medium priority.
    - Small (e.g., < 500x300): Low priority or ignore.
- **Filtering:** Skip very small images (potential icons) and ensure only `http/https` protocols are included.

## 4. Deduplication
Use hashing (like MD5) on the image filename or URL to ensure you don't collect the same image multiple times if it appears in different parts of the page.

## 5. Limiting Results
To avoid overwhelming the agent, limit the number of images returned (e.g., top 10 highest-scoring images).

Example Scoring Logic:
```python
def get_relevant_images(soup, url):
    image_urls = []
    all_images = soup.find_all('img', src=True)

    for img in all_images:
        img_src = urljoin(url, img['src'])
        score = 0

        # Priority by class
        if any(cls in img.get('class', []) for cls in ['header', 'featured', 'main', 'content']):
            score = 4

        # Priority by size
        width = parse_dimension(img.get('width', '0'))
        height = parse_dimension(img.get('height', '0'))
        if width > 800 and height > 500:
            score += 2

        image_urls.append({'url': img_src, 'score': score})

    return sorted(image_urls, key=lambda x: x['score'], reverse=True)[:10]
```
