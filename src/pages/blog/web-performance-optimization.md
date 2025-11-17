---
layout: ../../layouts/BlogPostLayout.astro
title: 'Web Performance Optimization'
description: 'Practical techniques to make your websites faster, from image optimization to code splitting and lazy loading.'
publishDate: '2025-11-16'
author: 'Min Aung Paing'
authorRole: 'Software Engineer'
authorBio: 'Web client languages student, junior year'
tags: ['Performance', 'Optimization', 'Best Practices']
---

## Why Performance Matters

Website performance directly impacts user experience, SEO rankings, and conversion rates. Studies show that even a 100ms delay can reduce conversions by 7%. Let's explore practical techniques to make your sites blazing fast.

## Measuring Performance

### Core Web Vitals

Google's Core Web Vitals are key metrics for user experience:

1. **LCP (Largest Contentful Paint)** - Loading performance (should be < 2.5s)
2. **FID (First Input Delay)** - Interactivity (should be < 100ms)
3. **CLS (Cumulative Layout Shift)** - Visual stability (should be < 0.1)

### Tools for Measurement

```bash
# Lighthouse (built into Chrome DevTools)
# Run from DevTools > Lighthouse tab

# WebPageTest
# https://www.webpagetest.org/

# Chrome User Experience Report
# https://developers.google.com/web/tools/chrome-user-experience-report
```

## Image Optimization

### Modern Image Formats

```html
<!-- Use WebP with fallback -->
<picture>
  <source srcset="image.webp" type="image/webp">
  <source srcset="image.jpg" type="image/jpeg">
  <img src="image.jpg" alt="Description">
</picture>

<!-- Or use AVIF for even better compression -->
<picture>
  <source srcset="image.avif" type="image/avif">
  <source srcset="image.webp" type="image/webp">
  <img src="image.jpg" alt="Description">
</picture>
```

### Responsive Images

```html
<!-- Serve different sizes based on viewport -->
<img
  srcset="
    image-320w.jpg 320w,
    image-640w.jpg 640w,
    image-1280w.jpg 1280w
  "
  sizes="
    (max-width: 320px) 280px,
    (max-width: 640px) 600px,
    1200px
  "
  src="image-640w.jpg"
  alt="Description"
>
```

### Lazy Loading

```html
<!-- Native lazy loading -->
<img src="image.jpg" alt="Description" loading="lazy">

<!-- For background images, use Intersection Observer -->
<script>
const observer = new IntersectionObserver((entries) => {
  entries.forEach(entry => {
    if (entry.isIntersecting) {
      entry.target.style.backgroundImage =
        `url(${entry.target.dataset.bg})`;
      observer.unobserve(entry.target);
    }
  });
});

document.querySelectorAll('[data-bg]').forEach(el => {
  observer.observe(el);
});
</script>
```

## JavaScript Optimization

### Code Splitting

Split your JavaScript into smaller chunks:

```javascript
// Dynamic imports for route-based splitting
const HomePage = () => import('./pages/Home.js');
const AboutPage = () => import('./pages/About.js');

// Load component only when needed
button.addEventListener('click', async () => {
  const { default: Modal } = await import('./components/Modal.js');
  const modal = new Modal();
  modal.show();
});
```

### Tree Shaking

Remove unused code:

```javascript
// Instead of importing everything
import * as utils from './utils.js'; // ❌

// Import only what you need
import { formatDate, parseDate } from './utils.js'; // ✅
```

### Minification and Compression

```javascript
// Use build tools for production
// package.json
{
  "scripts": {
    "build": "esbuild src/index.js --bundle --minify --outfile=dist/bundle.js"
  }
}
```

## CSS Optimization

### Critical CSS

Inline critical CSS and defer non-critical:

```html
<head>
  <!-- Inline critical CSS -->
  <style>
    /* Above-the-fold styles */
    body { margin: 0; font-family: sans-serif; }
    .header { background: #333; color: white; }
  </style>

  <!-- Defer non-critical CSS -->
  <link rel="preload" href="styles.css" as="style" onload="this.onload=null;this.rel='stylesheet'">
  <noscript><link rel="stylesheet" href="styles.css"></noscript>
</head>
```

### Remove Unused CSS

```bash
# Use PurgeCSS to remove unused styles
npm install -D @fullhuman/postcss-purgecss

# In postcss.config.js
module.exports = {
  plugins: [
    require('@fullhuman/postcss-purgecss')({
      content: ['./src/**/*.html', './src/**/*.js']
    })
  ]
}
```

## Resource Loading Strategies

### Preloading Critical Resources

```html
<!-- Preload critical fonts -->
<link rel="preload" href="font.woff2" as="font" type="font/woff2" crossorigin>

<!-- Preload critical images -->
<link rel="preload" href="hero.jpg" as="image">

<!-- Preconnect to external domains -->
<link rel="preconnect" href="https://api.example.com">
<link rel="dns-prefetch" href="https://api.example.com">
```

### Defer Non-Critical JavaScript

```html
<!-- Defer script execution -->
<script src="analytics.js" defer></script>

<!-- Async for independent scripts -->
<script src="tracking.js" async></script>
```

## Caching Strategies

### HTTP Caching Headers

```javascript
// Express.js example
app.use(express.static('public', {
  maxAge: '1y', // Cache for 1 year
  immutable: true
}));

// Set cache headers
res.setHeader('Cache-Control', 'public, max-age=31536000, immutable');
```

### Service Workers

```javascript
// service-worker.js
const CACHE_NAME = 'v1';
const ASSETS = ['/styles.css', '/script.js', '/logo.png'];

// Install and cache assets
self.addEventListener('install', event => {
  event.waitUntil(
    caches.open(CACHE_NAME)
      .then(cache => cache.addAll(ASSETS))
  );
});

// Serve from cache, fallback to network
self.addEventListener('fetch', event => {
  event.respondWith(
    caches.match(event.request)
      .then(response => response || fetch(event.request))
  );
});
```

## Database and API Optimization

### Reduce API Calls

```javascript
// Bad: Multiple sequential calls
const user = await fetch('/api/user/123');
const posts = await fetch('/api/user/123/posts');
const comments = await fetch('/api/user/123/comments');

// Good: Single call with all data
const userData = await fetch('/api/user/123?include=posts,comments');
```

### Implement Pagination

```javascript
// Server-side
app.get('/api/posts', (req, res) => {
  const page = parseInt(req.query.page) || 1;
  const limit = parseInt(req.query.limit) || 20;
  const skip = (page - 1) * limit;

  const posts = db.posts.find().skip(skip).limit(limit);
  res.json({ posts, page, hasMore: posts.length === limit });
});
```

### Use Compression

```javascript
// Express.js
const compression = require('compression');
app.use(compression());

// This compresses responses with gzip/brotli
```

## Performance Budget

Set and enforce performance budgets:

```javascript
// lighthouse-budget.json
{
  "resourceSizes": [
    { "resourceType": "script", "budget": 300 },
    { "resourceType": "image", "budget": 500 },
    { "resourceType": "stylesheet", "budget": 100 }
  ],
  "resourceCounts": [
    { "resourceType": "third-party", "budget": 10 }
  ]
}
```

## Monitoring

### Real User Monitoring (RUM)

```javascript
// Track actual user performance
const observer = new PerformanceObserver((list) => {
  for (const entry of list.getEntries()) {
    // Send to analytics
    analytics.track('web-vital', {
      name: entry.name,
      value: entry.value,
      rating: entry.rating
    });
  }
});

observer.observe({ entryTypes: ['largest-contentful-paint', 'first-input', 'layout-shift'] });
```

## Quick Wins Checklist

- [ ] Enable compression (gzip/brotli)
- [ ] Optimize and compress images
- [ ] Minify CSS and JavaScript
- [ ] Implement lazy loading for images
- [ ] Use CDN for static assets
- [ ] Set proper cache headers
- [ ] Remove unused CSS and JS
- [ ] Defer non-critical JavaScript
- [ ] Optimize web fonts
- [ ] Reduce server response time

## Conclusion

Web performance optimization is an ongoing process, not a one-time fix. Start with measuring your current performance, identify bottlenecks, and systematically address them using the techniques outlined here.

Remember:
- **Measure first** - Don't optimize blindly
- **Focus on user experience** - Core Web Vitals matter
- **Test on real devices** - Desktop performance ≠ mobile performance
- **Monitor continuously** - Performance degrades over time
- **Set budgets** - Prevent performance regression

By implementing these optimization techniques, you'll create faster, more efficient websites that users love!
