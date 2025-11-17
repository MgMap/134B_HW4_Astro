# Tech Learning Hub - Astro Static Site

A comprehensive web development learning resource built with **Astro**, showcasing modern static site generation with reusable components and layouts.

## Project Overview

This project demonstrates Astro's capabilities by creating a content-rich educational blog with:
- **Zero JavaScript by default** for optimal performance
- **Component-based architecture** for code reusability
- **View Transitions** for smooth, SPA-like navigation (Extra Credit)
- **6 comprehensive blog posts** covering web development topics

##  Project Structure

```
/
├── public/
│   └── favicon.svg
├── src/
│   ├── components/          # 4 Reusable Components
│   │   ├── AuthorBio.astro     # Author information display
│   │   ├── Card.astro          # Reusable content card
│   │   ├── Footer.astro        # Site footer with links
│   │   └── Navigation.astro    # Responsive navigation bar
│   ├── layouts/             # 2 Layout Templates
│   │   ├── BaseLayout.astro    # Main site wrapper with View Transitions
│   │   └── BlogPostLayout.astro # Blog post template with metadata
│   ├── pages/              # Routes
│   │   ├── index.astro         # Homepage
│   │   ├── blog.astro          # Blog listing page
│   │   ├── about.astro         # About page
│   │   └── blog/               # Individual blog posts (6 articles)
│   │       ├── getting-started-with-astro.md
│   │       ├── modern-css-layout.md
│   │       ├── javascript-es2024.md
│   │       ├── understanding-git.md
│   │       ├── working-with-rest-apis.md
│   │       └── web-performance-optimization.md
│   └── styles/
│       └── global.css          # Global styles with CSS variables
└── package.json
```

## ✨ Key Features

### Component Reuse
- **Navigation** component used across all pages via BaseLayout
- **Footer** component used across all pages via BaseLayout
- **Card** component used on homepage (3×) and blog listing page (6×)
- **AuthorBio** component used on all blog posts (6×)

### Layout Hierarchy
- **BaseLayout**: Provides site structure, navigation, footer, and View Transitions
- **BlogPostLayout**: Extends functionality for blog posts with metadata, tags, and author info

### View Transitions (Extra Credit )
Implemented Astro's View Transitions API in `BaseLayout.astro`:
```astro
import { ViewTransitions } from 'astro:transitions';
// ... in <head>
<ViewTransitions />
```

This provides smooth, SPA-like page transitions without the JavaScript overhead of a traditional SPA. Navigate between pages to see the smooth transitions in action!

### Content Quality
6 comprehensive blog posts covering:
1. **Getting Started with Astro** - Framework introduction and features
2. **Modern CSS Layout** - Grid, Flexbox, and modern CSS techniques
3. **JavaScript ES2024** - Latest JavaScript features and best practices
4. **Understanding Git** - Version control fundamentals and workflows
5. **Working with REST APIs** - API consumption and best practices
6. **Web Performance Optimization** - Techniques for faster websites

Each post includes:
- Detailed content with code examples
- Author information (via AuthorBio component)
- Publication date
- Topic tags
- Proper metadata for SEO

## Design Features

- **Responsive design** with mobile-first approach
- **Modern color scheme** using CSS custom properties
- **Dark mode support** via `prefers-color-scheme`
- **Gradient hero sections** for visual appeal
- **Smooth animations** and hover effects
- **Accessible** with semantic HTML and proper ARIA labels

## Commands

```bash
# Install dependencies
npm install

# Start development server
npm run dev

# Build for production
npm run build

# Preview production build
npm run preview
```

##  Learning Outcomes

This project demonstrates:
1. **Correct component reuse** - 4 components used across multiple pages
2. **Meaningful layouts** - 2 layouts creating a consistent site structure
3. **Large-scale content** - 6 detailed blog posts
4. **Minimal JS delivery** - Astro's zero-JS-by-default approach
5. **Extra Credit** - View Transitions implementation for enhanced UX

## 🌟 Extra Credit Implementation

The View Transitions feature is clearly implemented in:
- **File**: `src/layouts/BaseLayout.astro:3`
- **Usage**: Imported and added to `<head>` section
- **Effect**: Smooth page transitions throughout the site
- **Documentation**: This README and the About page both highlight this feature

Visit the **About page** in the built site to see a clear indication of the View Transitions implementation!

---

**Built with ❤️ using Astro**
