# GitHub Copilot Instructions

## Project Overview

This is a Jekyll-based personal blog that uses a **unique hybrid CI/CD approach**:
- **Source content** (drafts) stored in private Azure Repos
- **Static site generation** via Azure Pipelines using Jekyll
- **Hosting** on GitHub Pages at `johndpalm.github.io`

## Architecture & Key Insight

**Critical Understanding**: This repo contains **generated output**, not source files. The actual Jekyll source (markdown posts, layouts, configs) lives in a private Azure Repos repository. This repo only contains the compiled HTML/CSS/XML that Jekyll produces.

### Repository Structure
```
├── index.html          # Generated Jekyll homepage
├── feed.xml           # Generated RSS feed
├── 404.html           # Generated 404 page
├── YYYY/MM/DD/        # Generated blog post pages (date-based URLs)
├── about/             # Generated about page
├── privacy/           # Generated privacy page
├── assets/            # Generated CSS and static assets
└── azure-pipeline.yml # CI/CD configuration (only real source file)
```

## CI/CD Workflow (Azure Pipelines → GitHub Pages)

The deployment process in `azure-pipeline.yml` follows this pattern:

1. **Trigger**: Commits to `master` branch in private Azure Repos
2. **Build Environment**: Ubuntu with Ruby 2.5+
3. **Jekyll Build**: 
   - Copies source to `/tmp/source`
   - Installs bundler and dependencies
   - Runs `bundle exec jekyll build -d /tmp/build`
4. **GitHub Deployment**:
   - Clones this GitHub repo to staging
   - Replaces all content (except `.git/`) with Jekyll output
   - Commits and pushes to trigger GitHub Pages rebuild

### Required Variables
- `GitHubUserName`, `GitHubRepoName`, `GitHubPAT`, `GitHubEmail`
- `TenantId`, `ProjectName` (for Azure Repos integration)

## Development Guidelines

### Content Creation
- **Never edit HTML files directly** - they're generated and will be overwritten
- **Blog posts** should be created in the private Azure Repos as markdown files
- **Static pages** (about, privacy) follow Jekyll page conventions

### File Patterns
- **Posts**: Follow Jekyll's `YYYY/MM/DD/post-title.html` URL structure
- **Assets**: CSS uses Minima theme conventions (`main.css`, social icons SVG)
- **Metadata**: All pages include Jekyll SEO tag and structured data

### Styling Approach
- Uses **Minima theme** CSS patterns
- Responsive design with mobile-first approach
- Social media integration (GitHub, Twitter, LinkedIn)
- Custom 404 page styling

## Common Tasks

### Adding New Posts
1. Create markdown file in private Azure Repos (not here)
2. Azure Pipeline will automatically build and deploy
3. Verify deployment via GitHub Pages build status

### Updating Site Structure
- Modify layouts/includes in private Azure Repos source
- CSS changes go in source `assets/` directory
- Navigation updates in source `_includes/header.html`

### Debugging Builds
- Check Azure Pipeline logs for Jekyll build errors
- Verify GitHub Pages build status (pipeline includes API check)
- Common issues: missing dependencies, invalid YAML frontmatter

## Integration Points

- **GitHub Pages**: Automatic deployment on push to master
- **Azure DevOps**: Source repository and build pipeline
- **Jekyll SEO Plugin**: Automatic meta tags and structured data
- **RSS Feed**: Auto-generated at `/feed.xml`

## Important Constraints

- This is a **static site only** - no server-side processing
- All dynamic content must be generated at build time
- External integrations limited to client-side JavaScript
- GitHub Pages has specific Jekyll plugin limitations