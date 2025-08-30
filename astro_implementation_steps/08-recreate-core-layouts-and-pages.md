### 1. Recreate Core Layouts & Pages

*   **Action:** Manually recreate the functionality of Jekyll layouts and includes as new Astro components. The following is a list of Jekyll files to be migrated and their proposed Astro counterparts.

| Jekyll File | Astro Component | Purpose |
| :--- | :--- | :--- |
| `_layouts/base.html` | `src/layouts/BaseLayout.astro` | The main layout for all pages. |
| `_layouts/post.html` | `src/layouts/PostLayout.astro` | The layout for individual blog posts. |
| `_layouts/page.html` | `src/layouts/PageLayout.astro` | The layout for regular pages. |
| `_layouts/minimal.html` | `src/layouts/MinimalLayout.astro` | A minimal layout for special pages. |
| `_includes/head.html` | `src/components/Head.astro` | The `<head>` section with meta tags, etc. |
| `_includes/nav.html` | `src/components/Nav.astro` | The main navigation component. |
| `_includes/header.html` | `src/components/Header.astro` | The page header component. |
| `_includes/footer.html` | `src/components/Footer.astro` | The main footer component. |
| `_includes/footer-minimal.html` | `src/components/FooterMinimal.astro` | A minimal footer component. |
| `_includes/social-share.html` | `src/components/SocialShare.tsx` | A React component for social sharing. |
| `_includes/disqus.html` | `src/components/Disqus.tsx` | A React component for Disqus comments. |
| Analytics Includes | `src/components/Analytics.astro` | A single component to handle all analytics. |

*   **Steps:**
    1.  For each Jekyll file listed above, create the corresponding Astro component in the `astro_blog` directory.
    2.  Translate the Liquid templating syntax to Astro's syntax. For example, `{{ content }}` becomes `<slot />`, and `{% include file.html %}` becomes `<Component />`.
    3.  Re-implement any logic from the Jekyll includes in the new Astro components. For example, the social sharing and Disqus comments will be rebuilt as React components.
