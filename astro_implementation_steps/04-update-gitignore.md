### 4. Update `.gitignore`

*   **Action:** Add entries to the root `.gitignore` file to prevent Astro-specific generated files and dependencies from being committed to the repository.
*   **Content to add to `.gitignore`:**
    ```
    # Astro
    astro_blog/node_modules/
    astro_blog/dist/
    astro_blog/.astro/
    ```