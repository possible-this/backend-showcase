# Bot Blocking Strategy for Possible This

**Status:** Operational
**Implementation Level:** Server-Side (CloudPanel / NGINX)
**Objective:** Prevent AI model training and reduce server load while maintaining SEO visibility and social sharing capabilities.

---

## I. The "Kill List" (Block in CloudPanel)

Navigate to **CloudPanel > Security > Bots Blocking**. Add the following User-Agents one by one. These are strict blocks at the NGINX level.

### A. AI Model Trainers (The Data Vampires)
*Blocks these entities from scraping content to train future models (e.g., GPT-5, Claude 4) without providing traffic.*

| User-Agent String | Owner | Reason for Blocking |
| :--- | :--- | :--- |
| `GPTBot` | OpenAI | Trains models. Does not drive traffic. (Distinct from SearchBot). |
| `ClaudeBot` | Anthropic | Trains Claude models. |
| `anthropic-ai` | Anthropic | Secondary agent for Anthropic fetching. |
| `CCBot` | Common Crawl | The primary dataset used by almost all AI companies. Blocking this cuts off the main pipeline. |
| `FacebookBot` | Meta | Used for crawling to train LLaMA models. |
| `Amazonbot` | Amazon | Scrapes for Alexa and Amazon Q. High resource cost. |
| `Applebot-Extended` | Apple | Allows Apple to use data for AI training (Apple Intelligence), but keeps standard Applebot search active. |

### B. Aggressive Scrapers & Junk Traffic (The Performance Killers)
*Blocks these entities to save CPU/RAM. These provide no SEO value or commercial benefit.*

| User-Agent String | Owner | Reason for Blocking |
| :--- | :--- | :--- |
| `Bytespider` | ByteDance (TikTok) | Extremely aggressive. Scrapes for Chinese LLMs. High server load. |
| `MJ12bot` | Majestic | SEO crawler. Useless unless you pay for Majestic. |
| `Dotbot` | Moz | SEO crawler. Useless unless you pay for Moz. |
| `PetalBot` | Huawei | Search engine crawler for mobile users in China. |
| `SeekportBot` | Seekport | High-volume crawler with negligible traffic value. |

---

## II. The "Do Not Block" List (Critical Whitelist)

**WARNING:** Do not add the following to your block list. Doing so will break your site's functionality, SEO, or social sharing.

### A. Traffic & Search Engines
* **`OAI-SearchBot`**: Required for SearchGPT and ChatGPT Search visibility.
* **`ChatGPT-User`**: Required for live browsing by ChatGPT users.
* **`Googlebot`**: Required for Google Search indexing.
* **`Bingbot`**: Required for Bing/Microsoft Copilot.

### B. Social Media Previews (The "Amplification Loop")
* **`Twitterbot`**: Required for link previews (cards) on X.
* **`facebookexternalhit`**: Required for link previews on Facebook, Messenger, Instagram, and Threads.
* **`Pinterestbot`**: Required for pinning images to Pinterest.
* **`LinkedInBot`**: Required for link previews on LinkedIn.

---

## III. Implementation Notes

1.  **CloudPanel is Case-Insensitive:** Usually, but sticking to the official casing (e.g., `GPTBot`) is best practice.
2.  **Verification:** After blocking, you can test a block using `curl` in your terminal:
    ```bash
    curl -A "GPTBot" -I [https://possiblethis.com](https://possiblethis.com)
    ```
    * *Result should be:* `HTTP/1.1 403 Forbidden` (or 444 No Response depending on NGINX config).
    * *Test a valid one:* `curl -A "Googlebot" -I https://possiblethis.com` -> Should return `200 OK`.
