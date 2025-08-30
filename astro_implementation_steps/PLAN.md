# Astro Conversion Plan (Revised v3)

This document outlines the plan to migrate the existing Jekyll blog to a modern Astro-based site. The new site will be built in a dedicated `astro_blog` subdirectory. All legacy dependencies like jQuery and Bootstrap will be removed and replaced with modern solutions, using React for interactive components.

## Phase 1: Project Setup and Initialization

1.  **Create Astro Project Directory:**
    *   Create a new directory named `astro_blog`.

2.  **Initialize New Astro Project:**
    *   Initialize a new, best-practice Astro project inside the `astro_blog` directory using the official blog template.
    *   Command: `npm create astro@latest astro_blog -- --template blog`

3.  **Add React Integration:**
    *   Add the official Astro integration for React to the project.
    *   Command: `cd astro_blog && npx astro add react -y`

4.  **Update `.gitignore`:**
    *   Add Astro-specific build and dependency folders to the root `.gitignore` file. Example entries: `astro_blog/node_modules/`, `astro_blog/dist/`, `astro_blog/.astro/`.

## Phase 2: Content & Asset Migration

*All source paths in this phase are relative to the original Jekyll project root.*

1.  **Migrate Markdown Posts:**
    *   Copy existing blog posts from `./_posts/` to `astro_blog/src/content/blog/`. This aligns with Astro's Content Collections feature.

2.  **Update Post Frontmatter:**
    *   Update the frontmatter in the markdown files to match Astro's conventions (`date` -> `pubDate`, remove `layout`, etc.).

3.  **Migrate Images:**
    *   Copy the image directory from `./img/` to `astro_blog/public/img/`. This is the initial strategy for simplicity.

## Phase 3: Rebuilding Layouts, Styles, and Components

1.  **Recreate Core Layouts & Pages:**
    *   Manually rebuild Jekyll layouts (`./_layouts/`) and includes (`./_includes/`) as Astro components (`.astro`) in `astro_blog/src/layouts/` and `astro_blog/src/components/`.

2.  **Rebuild Styling from Scratch:**
    *   The legacy stylesheets (Bootstrap, `main.css`, etc.) will **not** be migrated.
    *   Styling will be rebuilt using modern CSS. We will start with the styles from the Astro blog template and customize them as needed inside `astro_blog/src/styles/`.

3.  **Reimplement Interactivity with React:**
    *   Legacy JavaScript (`jQuery`, `bootstrap.js`) will **not** be migrated.
    *   Identify all functionality that previously relied on these scripts (e.g., social sharing, mobile navigation toggles, etc.).
    *   This functionality will be rebuilt from scratch as new, interactive React components inside `astro_blog/src/components/`.

## Phase 4: CI/CD with GitHub Actions

1.  **Create Workflow File:**
    *   Create a new GitHub Actions workflow file at `.github/workflows/deploy.yml`.

2.  **Define Workflow:**
    *   The workflow will trigger on a `push` to the `main` branch and will be configured to operate within the `astro_blog` directory for all build and deployment steps.

## Phase 5: Verification and Cutover

1.  **Local & Live Verification:**
    *   Thoroughly test the new site locally and after deployment.

2.  **Plan the Cutover:**
    *   Once the new Astro site is stable, a final cutover can be performed in a separate PR. This involves removing the old Jekyll files, moving the new Astro site to the root, and updating all relevant paths.

## Phase 6: Post-Migration Refinements

1.  **Asset Optimization:**
    *   Explore co-locating images by moving them from `public/img/posts/` into their respective `src/content/blog/` post folders for easier maintenance.

2.  **Component Refactoring:**
    *   Review the rebuilt components and layouts to identify opportunities for creating more reusable, smaller components.