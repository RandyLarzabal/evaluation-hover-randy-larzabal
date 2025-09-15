# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This is **Dawn**, Shopify's reference theme that represents a HTML-first, JavaScript-only-as-needed approach to theme development. Dawn is built for Shopify's Online Store 2.0 platform using the Liquid templating language and follows web-native principles for performance and accessibility.

## Development Commands

### Theme Development with Shopify CLI
```bash
# Install Shopify CLI first, then:
shopify theme dev                    # Start local development server
shopify theme push                   # Push theme to Shopify store
shopify theme pull                   # Pull theme from Shopify store
shopify theme check                  # Run Theme Check linting
```

### Code Quality & Validation
```bash
shopify theme check                  # Lint Liquid, CSS, and JS files
```

### Git Workflow
```bash
git remote add upstream https://github.com/Shopify/dawn.git
git fetch upstream
git pull upstream main              # Pull latest Dawn changes when forking
```

## Architecture & File Structure

### Core Theme Architecture
Dawn follows Shopify's standard theme structure with Online Store 2.0 features:

- **`layout/`** - Base templates (`theme.liquid`, `password.liquid`)
- **`templates/`** - Page templates (`.json` files for OS 2.0, `.liquid` for legacy)
- **`sections/`** - Reusable theme sections (Liquid files)
- **`snippets/`** - Reusable code components (Liquid files) 
- **`assets/`** - CSS, JavaScript, images, and other static files
- **`config/`** - Theme settings (`settings_schema.json`, `settings_data.json`)
- **`locales/`** - Translation files for internationalization

### Template System (Online Store 2.0)
Templates are JSON files that define:
- Which sections to include
- Section settings and configuration
- Block order within sections

Example: `templates/index.json` defines the homepage layout using sections like `image-banner`, `featured-collection`, etc.

### Component Architecture
Dawn uses a component-based approach:

1. **Sections** - Main building blocks (e.g., `main-product.liquid`, `header.liquid`)
2. **Snippets** - Reusable components (e.g., `card-product.liquid`, `price.liquid`)
3. **Assets** - Styling and functionality (component-specific CSS and JS)

### JavaScript Architecture
- **Web Components** - Custom elements for interactive components (e.g., `CartItems`, `ProductInfo`)
- **ES6 Classes** - Modern JavaScript patterns for component logic
- **Progressive Enhancement** - JavaScript enhances HTML functionality
- **Event-Driven** - Uses DOM events for component communication

Key utilities in `assets/global.js`:
- `SectionId` - Helper for section ID parsing
- `HTMLUpdateUtility` - DOM manipulation utilities
- `getFocusableElements` - Accessibility helper

### CSS Organization
Component-specific stylesheets following BEM-like methodology:
- `component-*.css` - Individual component styles
- `base.css` - Base styles and design tokens
- Section-specific styles included inline or via dedicated CSS files

## Key Development Concepts

### Liquid Integration
- Use `{{ }}` for output, `{% %}` for logic
- Leverage Shopify objects: `product`, `collection`, `cart`, etc.
- Include translation support: `{{ 'key' | t }}`

### Performance Philosophy
- HTML-first rendering (server-side via Liquid)
- JavaScript only for enhancement, not core functionality  
- Progressive enhancement for older browsers
- Minimal JavaScript footprint

### Theme Check Configuration
Configured in `.theme-check.yml`:
- `MatchingTranslations: disabled` - Allows missing translations
- `TemplateLength: disabled` - No template size limits

### Code Formatting
Prettier configuration in `.prettierrc.json`:
- 120 character line width
- Single quotes for JS/CSS
- Double quotes for Liquid files

## Development Best Practices

### Performance Guidelines
- Lazy load images using `loading="lazy"`
- Use `preload` for critical resources
- Minimize JavaScript bundle size
- Optimize image formats and sizes

### Accessibility Standards
- Semantic HTML structure
- ARIA attributes where needed
- Keyboard navigation support
- Screen reader compatibility
- Focus management for modals/drawers

### Shopify-Specific Patterns
- Use `{% liquid %}` for complex logic blocks
- Leverage section groups for flexible layouts
- Implement proper cart/checkout integration
- Handle variant selection and inventory states
- Support multiple currencies and languages

### Component Development
When creating new components:
1. Start with semantic HTML structure in Liquid
2. Add component-specific CSS file
3. Enhance with JavaScript Web Component if interactive
4. Include proper documentation comments
5. Test across different screen sizes and browsers

### Translation & Localization
- Add translations to `locales/en.default.json`
- Use `{{ 'key' | t }}` in templates
- Include schema translations for settings
- Test with different languages and RTL layouts

## Testing & Quality Assurance

The CI pipeline runs:
- **Lighthouse CI** - Performance, accessibility, and SEO audits
- **Theme Check** - Liquid, CSS, and JavaScript linting

Always test changes across:
- Different viewport sizes (mobile, tablet, desktop)
- Various product configurations (variants, inventory states)
- Multiple browsers (Chrome, Firefox, Safari, Edge)
- Different theme settings combinations

## Common Patterns

### Product Cards
Use `snippets/card-product.liquid` with parameters:
- `card_product` - Product object
- `show_secondary_image` - Hover state
- `show_vendor` - Display brand
- `lazy_load` - Performance optimization

### Section Integration
Sections support settings defined in their schema and can include multiple blocks for flexible content management.

### Cart Functionality
Cart operations use AJAX for seamless updates without page reloads. Key components:
- `CartItems` Web Component for cart management
- `cart-drawer.js` for slide-out cart functionality
- Cart notification system for user feedback