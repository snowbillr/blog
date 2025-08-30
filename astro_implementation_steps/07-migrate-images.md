### 3. Migrate Images

*   **Action:** Copy the entire `img` directory from the Jekyll project root to the `public` directory in the new Astro project.
*   **Command:**
    ```bash
    cp -r img/ astro_blog/public/
    ```
*   **Note:** This makes all images available as static assets. Image paths in Markdown posts will not need to be updated, as the `/img/` directory will be served from the root of the new site.