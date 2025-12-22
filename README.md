# mh-sheepu.github.io

Personal GitHub Pages site with a terminal-inspired aesthetic, link hub, featured projects, and tech stack.

## Pages
- [index.html](index.html): Home hub with links, projects, tech, and contact.
- [links.html](links.html): Focused links page with the same visual style.

## Editing
- Update link cards directly in the HTML files; each card is a simple anchor with title and description.
- Projects are displayed as cards in a responsive grid. Duplicate an existing card to add more.
- Colors and layout live in the `<style>` block at the top of each page.

## Local Preview
Open the `index.html` file directly in your browser, or use a simple server:

```bash
python3 -m http.server 8080
# then visit http://localhost:8080/
```

## Deploy
Push changes to the `main` branch. GitHub Pages will update automatically for this repository.

## Notes
- Matrix rain effect uses a lightweight canvas animation.
- Navigation is fixed at the top and is mobile-friendly.