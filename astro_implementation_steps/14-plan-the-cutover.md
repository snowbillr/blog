### 2. Plan the Cutover

*   **Action:** Once the Astro site is confirmed to be working correctly, create a plan to make it the live site.
*   **Steps (to be performed in a separate PR):**
    1.  Remove all old Jekyll files and directories (`_posts`, `_layouts`, `Gemfile`, etc.).
    2.  Move the contents of the `astro_blog` directory to the project root.
    3.  Update the `.gitignore` file to remove the `astro_blog/` prefix from the paths.
    4.  Update the `deploy.yml` workflow to remove the `working-directory` property.