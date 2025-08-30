### 2. Define Workflow

*   **Action:** Add the following YAML configuration to `deploy.yml`. This workflow will build the Astro site and prepare it for deployment.
*   **Content for `deploy.yml`:**
    ```yaml
    name: Deploy Astro Site

    on:
      push:
        branches:
          - main

    jobs:
      build:
        runs-on: ubuntu-latest
        steps:
          - name: Checkout code
            uses: actions/checkout@v3

          - name: Setup Node.js
            uses: actions/setup-node@v3
            with:
              node-version: '18'

          - name: Install dependencies
            run: npm install
            working-directory: ./astro_blog

          - name: Build site
            run: npm run build
            working-directory: ./astro_blog
    ```