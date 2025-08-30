### 1. Asset Optimization

*   **Action:** Consider moving images from the `public` directory to be co-located with the blog posts that use them.
*   **Benefit:** This improves organization and makes it easier to manage assets on a per-post basis.
*   **How:**
    1.  Create a folder for each post in `src/content/blog/`.
    2.  Move the post's Markdown file and its images into that folder.
    3.  Update the image paths in the Markdown file to be relative.