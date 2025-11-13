## 📢 Self-Hosted WordPress Deployment Guide (VPS/CloudPanel)

This document serves as a standardized, secure guide detailing the necessary steps to configure a Virtual Private Server (VPS) running **CloudPanel** (NGINX/PHP-FPM) and deploy a securely hosted WordPress instance.

**Note:** All sensitive credentials, IP addresses, and unique domain names are replaced with generalized placeholders to ensure security.

### 1. 🌐 DNS Configuration and Security Strategy

The core strategy for this deployment is using the registrar's DNS (Namecheap) for management and employing a security-through-obscurity approach for the control panel.

| Record Type | Host | Points to | Purpose |
| :--- | :--- | :--- | :--- |
| **A Record** | `@` | `YOUR_VPS_IP_ADDRESS` | Main domain access. |
| **A Record** | `www` | `YOUR_VPS_IP_ADDRESS` | `www` subdomain access. |
| **A Record** | `[random-subdomain]` | `YOUR_VPS_IP_ADDRESS` | **Secure Control Panel Access.** (e.g., `popcorn.example.com`) |

**Security Note:** Using a non-standard subdomain (e.g., `weirdo` instead of `cp` or `admin`) for the CloudPanel login mitigates automated brute-force scanning attempts.

### 2. 🛡️ CloudPanel Setup & Website Provisioning

Once DNS propagation is complete, the VPS services are configured via the CloudPanel interface.

1.  **Access CloudPanel:** Log in to the control panel using the IP address and port (e.g., `https://YOUR_VPS_IP_ADDRESS:8443/`) until the secure domain is validated.
2.  **Secure the Panel Interface:**
    * Navigate to **Settings** > **CloudPanel Custom Domain**.
    * Set the domain name to the secure, random subdomain: `https://[random-subdomain].example.com`.
3.  **Issue Panel SSL:** Attempt to issue the **Let's Encrypt Certificate** for the panel's secure domain. This confirms DNS propagation is complete.
4.  **Add Main Website:**
    * Navigate to **Sites** and click **Add New Website**.
    * Select **WordPress** as the application template.
    * Enter the **Domain Name**: `example.com`.
    * This step creates the NGINX Virtual Host configuration required to serve the domain.
5.  **Optional: Basic Auth:** Enable **Basic Authentication** via the site's **Security** tab if the site will be in a staging/development state for a prolonged period.

### 3. 🔐 Final SSL/HTTPS Configuration

The last step is ensuring the main website is secured with a trusted certificate, replacing the initial Self-Signed certificate.

1.  Navigate to the site's **SSL/TLS** tab.
2.  Select **Actions** > **New Let's Encrypt Certificate**.
3.  Issue the certificate for both the naked domain (`example.com`) and the `www` version.
4.  **Verification:** Confirm that traffic is automatically redirected to `https://` and the browser shows a lock icon.

### 4. 💻 Infrastructure & Backup Strategy

This deployment uses a self-managed strategy to reduce recurring hosting costs.

* **Backup Method:** Utilize the command-line utility **`rclone`** to securely connect the VPS to an external cloud storage provider (e.g., Google Drive, Dropbox, S3).
* **Automation:** A scheduled **`cron` job** executes a backup script (`backup.sh`) daily/weekly to dump the MySQL database and archive the website files before transferring them via `rclone copy`.

This public document showcases competence in Linux VPS management, secure configuration, DNS, and application deployment (LAMP/LEMP stack).
