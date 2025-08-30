### 1. Migrate Markdown Posts

*   **Action:** Copy all existing Markdown blog posts from the Jekyll `_posts` directory to the Astro `src/content/blog` directory.
*   **Command:**
    ```bash
    cp -r _posts/* astro_blog/src/content/blog/
    ```