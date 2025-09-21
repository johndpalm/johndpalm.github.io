# GitHub Copilot Instructions

## Project Overview

This is a Jekyll-based personal blog using **GitHub Actions for CI/CD**:
- **Source content** stored directly in this GitHub repository
- **Static site generation** via GitHub Actions using Jekyll
- **Hosting** on GitHub Pages at `johndpalm.github.io`

## Architecture & Key Insight

**Critical Understanding**: This repo contains both Jekyll source files AND serves as the deployment target. GitHub Actions builds the Jekyll site and deploys it automatically on every push to master.

### Repository Structure
```
├── _posts/            # Jekyll blog posts (markdown source)
├── _config.yml        # Jekyll configuration
├── Gemfile            # Ruby dependencies
├── index.md           # Homepage (Jekyll source)
├── about.md           # About page (Jekyll source)
├── privacy.md         # Privacy page (Jekyll source)
├── assets/            # CSS, images, and static files
├── _site/             # Generated output (ignored in git)
└── .github/workflows/ # GitHub Actions CI/CD
```

## CI/CD Workflow (GitHub Actions)

The deployment process in `.github/workflows/jekyll.yml` follows this pattern:

1. **Trigger**: Commits to `master` branch or manual workflow dispatch
2. **Build Environment**: Ubuntu with Ruby 3.1+
3. **Jekyll Build**: 
   - Checks out repository
   - Installs Ruby and runs `bundle install`
   - Runs `bundle exec jekyll build --baseurl`
4. **GitHub Pages Deployment**:
   - Uploads build artifact to GitHub Pages
   - Automatically deploys to live site

### Environment Variables
- All configuration handled via `_config.yml`
- No external secrets required

## Development Guidelines

### Content Creation
- **Blog posts**: Create markdown files in `_posts/` with `YYYY-MM-DD-title.md` naming
- **Pages**: Create markdown files in root with YAML front matter
- **Assets**: Add CSS, images, and static files to `assets/` directory

### File Patterns
- **Posts**: Use Jekyll front matter with layout, title, date, categories
- **Pages**: Use `permalink` in front matter for custom URLs
- **Styling**: Minima theme with custom CSS in `assets/main.scss`

### Local Development
```bash
bundle install          # Install dependencies
bundle exec jekyll serve # Run local server on http://localhost:4000
bundle exec jekyll build # Build static site to _site/
```

## Common Tasks

### Adding New Posts
1. Create markdown file in `_posts/` with format: `YYYY-MM-DD-title.md`
2. Add Jekyll front matter (layout, title, date, categories)
3. Commit and push to master - GitHub Actions will build and deploy

### Updating Site Structure
- Modify `_config.yml` for site-wide settings
- Update `Gemfile` for Jekyll plugins and dependencies
- Create custom layouts in `_layouts/` directory
- Override theme styles in `assets/main.scss`

### Debugging Builds
- Check GitHub Actions logs in the Actions tab
- Verify Jekyll build locally with `bundle exec jekyll build`
- Common issues: missing dependencies, invalid YAML front matter

## Integration Points

- **GitHub Pages**: Automatic deployment on push to master
- **GitHub Actions**: Build and deployment pipeline
- **Jekyll SEO Plugin**: Automatic meta tags and structured data
- **RSS Feed**: Auto-generated at `/feed.xml`

## Important Constraints

- This is a **static site only** - no server-side processing
- All dynamic content must be generated at build time
- External integrations limited to client-side JavaScript
- GitHub Pages has specific Jekyll plugin limitations