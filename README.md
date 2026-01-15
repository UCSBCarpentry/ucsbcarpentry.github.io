# UCSB Library Carpentry Workshops

This repository is the source for the GitHub Pages site:

[https://ucsbcarpentry.github.io](https://ucsbcarpentry.github.io)

## Built with Hugo

This site is built using the [Hugo](https://gohugo.io/) static site generator.

### Local Development

1. [Install Hugo](https://gohugo.io/installation/)
2. Run the development server:
   ```bash
   hugo server
   ```
3. Visit http://localhost:1313

### Adding Content

- **Workshops**: Add markdown files to `content/workshop/`
- **Meetings**: Add markdown files to `content/meeting/`
- **Static pages**: Add to `content/` or `content/community/`

### Deployment

The site automatically deploys to GitHub Pages via GitHub Actions when changes are pushed to the `main` branch.
