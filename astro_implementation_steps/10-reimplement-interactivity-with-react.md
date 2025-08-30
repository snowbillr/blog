### 3. Reimplement Interactivity with React

*   **Action:** Replace all functionality that previously used jQuery or Bootstrap JS with new, purpose-built React components.
*   **Process:**
    1.  **Identify interactive elements:** Look for things like mobile navigation toggles, social share buttons, or any other client-side dynamic features.
    2.  **Create React components:** For each piece of functionality, create a new `.tsx` file in `astro_blog/src/components/`.
    3.  **Integrate into Astro:** Use the React components in your `.astro` files with the `client:load` directive to enable client-side rendering.
    *   **Example:**
        ```astro
        ---
        import MyInteractiveComponent from '../components/MyInteractiveComponent.tsx';
        ---
        <MyInteractiveComponent client:load />
        ```
*   **Specifics:** Based on the current implementation, the following interactive elements need to be recreated in React:
    *   **Dynamic Navbar (`js/main.js`):**
        *   A component that shrinks the navbar and fades out the avatar on scroll. This can be achieved with a `useEffect` hook that adds a scroll event listener.
        *   The mobile navigation toggle (hamburger button) will be part of the navbar component, managing its open/closed state.
        *   The multi-level dropdown functionality for mobile needs to be rebuilt in React, managing the visibility of sub-menus with component state.
    *   **Random Header Image (`js/main.js`, `_includes/header.html`):**
        *   A component that cycles through a list of background images for the page header. The image URLs can be passed as props. A `useEffect` hook with a `setTimeout` can handle the cycling.
    *   **Social Share Buttons (`_includes/social-share.html`):**
        *   Create a `SocialShare` component that takes the page title and URL as props and renders the share links. While the original implementation is just HTML, a React component is cleaner.
    *   **Disqus Comments (`_includes/disqus.html`):**
        *   Create a `DisqusComments` component. To optimize performance, this component should use the Intersection Observer API to only load the Disqus embed script when the component is scrolled into view.