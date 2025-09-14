# 📚 Professional Documentation Website Template

A modern, responsive documentation website template perfect for technical projects, APIs, and system documentation.

## 🌟 Features

- **Modern Design**: Clean, professional interface with responsive layout
- **GitHub Pages Ready**: Automatic deployment with GitHub Actions
- **Markdown Support**: Dynamic markdown rendering with syntax highlighting
- **Architecture Documentation**: Built-in support for ADRs, C4 diagrams, and technical specs
- **Navigation System**: Structured documentation with breadcrumbs and categories
- **Mobile Responsive**: Works perfectly on all devices
- **Professional Styling**: Corporate-ready design with customizable branding

## 🚀 Quick Start

### 1. Use This Template
1. Click "Use this template" on GitHub
2. Create your new repository (public for GitHub Pages)
3. Clone to your local machine

### 2. Enable GitHub Pages
1. Go to Repository Settings → Pages
2. Source: "GitHub Actions"
3. The site will deploy automatically on every push

### 3. Customize Content
- Edit `index.html` for the main page
- Add your markdown files in organized folders
- Update `_config.yml` with your project details
- Customize styling in the `<style>` section of `index.html`

## 📁 Structure

```
├── index.html          # Main documentation page
├── md.html            # Markdown renderer page
├── _config.yml        # Jekyll/GitHub Pages config
├── api/               # API documentation
├── architecture/      # System architecture docs
├── design/            # Design decisions and ADRs
├── guides/            # User guides and tutorials
├── operations/        # Operational documentation
└── tools/             # Development tools and configs
```

## 🎨 Customization

### Branding
- Update title and description in `index.html`
- Modify the gradient colors in the CSS
- Replace the 🤖 emoji with your project icon
- Update author meta tag

### Content
- Replace placeholder content with your project details
- Update links to point to your repositories
- Modify the service/feature cards
- Add your own documentation sections

### Colors & Styling
```css
/* Main brand colors */
--primary-color: #3498db;
--secondary-color: #2c3e50;
--accent-color: #e74c3c;
```

## 📖 Adding Documentation

### Markdown Files
1. Create `.md` files in appropriate folders
2. Link to them using: `md.html?path=folder/filename.md`
3. Use standard markdown syntax

### ADR (Architecture Decision Records)
- Add new ADRs in `design/adr/`
- Follow the naming convention: `XXXX-title.md`
- Link from the main ADR index

### API Documentation
- Document endpoints in `api/api-reference.md`
- Include request/response examples
- Add authentication details

## 🔧 GitHub Actions Workflow

The template includes automatic GitHub Pages deployment:

```yaml
# .github/workflows/pages.yml
name: Deploy to GitHub Pages
on:
  push:
    branches: [ main ]
jobs:
  deploy:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - uses: actions/configure-pages@v3
      - uses: actions/upload-pages-artifact@v2
        with:
          path: '.'
      - uses: actions/deploy-pages@v2
```

## 🌐 Live Demo

Perfect for open source projects, API documentation, technical guides, system documentation, and developer resources.

## 🤝 Contributing

1. Fork the repository
2. Create a feature branch
3. Make your improvements
4. Submit a pull request

---

**Perfect for**: Open source projects, API documentation, technical guides, system documentation, developer resources

**Ready to deploy**: Just enable GitHub Pages and start writing!