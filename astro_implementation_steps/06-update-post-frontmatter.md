### 2. Update Post Frontmatter

*   **Action:** Manually or with a script, update the YAML frontmatter of each Markdown file in `astro_blog/src/content/blog/`.
*   **Changes to make:**
    *   Rename `date` to `pubDate`.
    *   Remove the `layout` property (Astro handles layouts differently).
    *   Ensure other properties like `title` and `description` are consistent with the Astro blog template's expectations.
*   **Example:**
    *   **Old (Jekyll):**
        ```yaml
        ---
        layout: post
        title: "My Awesome Post"
        date: 2023-10-27 10:00:00 -0400
        ---
        ```
    *   **New (Astro):**
        ```yaml
        ---
        title: "My Awesome Post"
        description: "A summary of my awesome post."
        pubDate: "Oct 27 2023"
        ---
        ```