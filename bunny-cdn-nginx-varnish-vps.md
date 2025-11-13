## 🚀 Bunny CDN Integration with NGINX/Varnish VPS (Showcase)

This guide documents the integration of an external Content Delivery Network (CDN) with a high-performance, self-managed VPS hosting a WordPress application. This strategy maintains DNS control at the registrar while utilizing the CDN for global asset distribution and speed optimization.

---

### 1. ⚙️ CDN Pull Zone Configuration

This step establishes the necessary infrastructure on the CDN platform, instructing it where to source the website's content.

1.  **Pull Zone Setup:** Created a new Pull Zone within the CDN dashboard.
2.  **Origin Definition:** Set the **Origin URL** to the secured primary domain: `https://example.com`.
3.  **Endpoint Retrieval:** Retrieved the unique CNAME hostname provided by the CDN (e.g., `[unique-id].b-cdn.net`).

### 2. 🌐 DNS Configuration (External Registrar)

The necessary CNAME record is created on the external DNS provider (e.g., Namecheap) to route static asset requests through the CDN's global network.

| Record Type | Host | Value/Target | Purpose |
| :--- | :--- | :--- | :--- |
| **CNAME Record** | `cdn` | `[unique-id].b-cdn.net` | Links the designated asset subdomain to the CDN service endpoint. |

### 3. 🧩 WordPress & Server Optimization Integration

This phase connects the WordPress application and the server's caching layer to the new CDN endpoint for full integration.

1.  **Application Rewriting:** Installed and configured a WordPress CDN plugin (e.g., CDN Enabler or WP Rocket) to automatically rewrite all static asset URLs (images, CSS, JS) to use the new CDN subdomain: `https://cdn.example.com/`.
2.  **Server Cache Purge:** Executed a full cache purge on the server's dedicated caching layer (Varnish Cache) via the CloudPanel interface to ensure the CDN immediately begins pulling the freshest content.

### 4. ✨ Performance Benefits

* **Decoupled Services:** DNS and CDN management remain separate from the hosting environment, improving portability and control.
* **Global Speed:** Static assets are served from the closest global point-of-presence (POP) to the user, dramatically reducing load times worldwide.
* **Origin Offload:** Reduces bandwidth and resource load on the VPS, as static asset requests are handled by the CDN network instead of the NGINX/Varnish stack.
