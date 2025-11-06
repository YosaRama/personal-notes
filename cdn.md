---
description: This page will tell a story about how CDN work and when we going to use that
---

# CDN

## Problem

Websites with global users often experience **slow loading times** because:

* Assets (images, scripts, CSS) are served from a single origin server.
* Users far from that server experience high **latency** and **network congestion**.
* Heavy traffic can overload the origin server, reducing performance or causing downtime.

This affects:

* SEO (search engines penalize slow pages)
* User experience (higher bounce rates)
* Server costs (increased load, bandwidth usage)



## Approach

Use a **Content Delivery Network (CDN)** to distribute content geographically.\
A CDN works by:

1. **Caching static content** (images, videos, CSS, JS, etc.) on multiple edge servers around the world.
2. When a user requests a file, it is served from the **nearest CDN node (edge location)** — reducing latency.
3. **Dynamic content** (e.g. PHP or API responses) can also be optimized using CDN routing rules, caching headers, or reverse proxy setups.
4. Common CDNs include **Cloudflare**, **AWS CloudFront**, **Akamai**, **Fastly**, and **BunnyCDN**.



## Solutions

**Example Setup (WordPress + Apache)**

1. **Configure CDN provider:**
   * Point your CDN to your origin domain (e.g., `yourdomain.com`).
   * Enable caching for `/wp-content/uploads/`, CSS, JS, and images.
2. **Rewrite URLs:**
   *   Use plugin or manual SQL replace to change static URLs:

       ```
       https://yourdomain.com/wp-content/uploads/
       → https://cdn.yourdomain.com/wp-content/uploads/
       ```
3.  **Set caching headers:**

    ```apache
    <IfModule mod_headers.c>
      <FilesMatch "\.(jpg|jpeg|png|gif|css|js|ico|webp)$">
        Header set Cache-Control "max-age=31536000, public"
      </FilesMatch>
    </IfModule>
    ```
4. **Clear Cache when needed:**
   * Use your CDN’s purge API or panel.
   * Clear both **CDN cache** and **WordPress plugin cache** (e.g. W3 Total Cache, WP Rocket).

**Benefits:**

* Faster load times globally
* Reduced bandwidth on origin server
* Improved scalability and reliability

\
**Example Setup (React.js Application)**

**1. Build your static assets**

Generate production assets:

```bash
npm run build
```

This creates a `build/` or `dist/` folder containing all static files (HTML, CSS, JS, images).

***

**2. Upload build files to CDN**

Use one of these setups:

* **Cloudflare / AWS CloudFront / Netlify / Vercel / BunnyCDN**
* Serve your React build folder through your CDN origin or S3 bucket.

**Example:**\
If using AWS:

```bash
aws s3 sync build/ s3://your-bucket-name --delete
aws cloudfront create-invalidation --distribution-id <DIST_ID> --paths "/*"
```

This uploads and refreshes your CDN cache automatically after deployment.

***

**3. Set caching headers**

Make sure your CDN uses long-lived cache headers for static assets:

```bash
Cache-Control: public, max-age=31536000, immutable
```

and shorter cache headers for `index.html` (since it changes frequently):

```bash
Cache-Control: no-cache
```

If you use **Vite** or **Next.js**, they handle cache-busting automatically by including unique hashes in filenames (e.g. `main.8c1f3.js`).

***

**4. Use CDN URLs in your app**

If your assets are hosted on a CDN domain (e.g., `https://cdn.yourdomain.com`), set it as your **public path**:

**For Create React App:**\
In `.env`:

```
PUBLIC_URL=https://cdn.yourdomain.com
```

**For Vite:**\
In `vite.config.js`:

```js
export default defineConfig({
  base: 'https://cdn.yourdomain.com/',
});
```

**For Next.js:**\
In `next.config.js`:

```js
module.exports = {
  assetPrefix: 'https://cdn.yourdomain.com/',
};
```

***

**5. Cache Invalidation**

When deploying a new build:

* Trigger CDN cache purge (e.g., via AWS CLI or API call).
* Many CDNs auto-invalidate when file hashes change (best practice).

***

**🚀 Benefits**

* Drastically reduced asset loading time globally
* Lower server bandwidth usage
* Better SEO and Core Web Vitals
* Seamless scalability for large traffic spikes
