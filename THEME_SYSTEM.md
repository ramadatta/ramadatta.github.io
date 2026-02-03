# Theme System Documentation

This document explains how the theme system works and how to switch between themes.

## Overview

The site supports multiple themes that can be easily switched. Currently, two themes are available:

1. **Visual Cinnamon** (currently active) - Minimal, clean theme inspired by visualcinnamon.com
2. **Warm Sunset** (preserved) - Friendly, inviting theme with warm orange/amber tones

## Theme Files Location

All theme-related files are located in the `_sass/` directory:

```
_sass/
├── _themes.scss                    # Theme switcher (imports active theme)
├── _themes-visual-cinnamon.scss    # Visual Cinnamon theme colors
├── _themes-warm-sunset.scss        # Warm Sunset theme (preserved)
├── _base.scss                      # Base styles (uses theme variables)
├── _components.scss                # Component styles (uses theme variables)
└── _layout.scss                    # Layout styles (uses theme variables)
```

## How Theme Switching Works

The theme system uses a simple import mechanism:

1. `_themes.scss` is a wrapper file that imports the active theme
2. `_base.scss` imports `_themes.scss` to get theme variables
3. All other SCSS files import `_themes.scss` or `_base.scss` to access theme variables

## Switching Themes

To switch between themes, edit `_sass/_themes.scss`:

### Activate Visual Cinnamon Theme:
```scss
// Active theme: visual-cinnamon
@import 'themes-visual-cinnamon';

// To switch back to warm sunset, comment above and uncomment below:
// @import 'themes-warm-sunset';
```

### Activate Warm Sunset Theme:
```scss
// Active theme: warm-sunset
// @import 'themes-visual-cinnamon';

// To switch back to warm sunset, comment above and uncomment below:
@import 'themes-warm-sunset';
```

After changing the theme, rebuild Jekyll to see the changes.

## Theme Details

### Visual Cinnamon Theme
- **Style**: Minimal, clean, professional
- **Colors**:
  - Background: `#ffffff` (pure white)
  - Text: `#222222` (primary), `#555555` (secondary)
  - Links: `#0066cc` (blue), `#004080` (hover), `#551a8b` (visited)
  - Borders: `#e0e0e0` (light gray)
  - Code: `#f8f8f8` (light gray background)
- **Fonts**: System fonts (Apple, Segoe UI, Roboto, etc.) for body/headings
- **Typography**: 18px base font, 1.65 line-height
- **Container**: 800px max-width, 1.5rem padding

### Warm Sunset Theme
- **Style**: Friendly, inviting, warm
- **Colors**:
  - Background: `#fffbf5` (warm white/cream)
  - Primary: `#e85d04` (burnt orange)
  - Accent: `#6a040f` (deep burgundy)
  - Text: `#1a1a1a` (near black), `#4a4a4a` (dark gray)
- **Fonts**: JetBrains Mono (monospace) for all text
- **Typography**: Smaller base font, 1.8 line-height
- **Container**: 1100px max-width

## Theme Variables

Each theme defines the following SCSS variables:

- `$color-primary` - Primary brand color
- `$color-accent` - Accent color
- `$bg-dark` - Main background color
- `$bg-dark-elevated` - Elevated/card background
- `$bg-dark-surface` - Surface background
- `$text-dark-primary` - Primary text color
- `$text-dark-secondary` - Secondary text color
- `$text-dark-muted` - Muted text color
- `$border-dark` - Border color
- `$code-bg` - Code block background
- `$code-text` - Code block text color
- `$gradient-primary` - Primary gradient
- `$shadow-sm`, `$shadow-md`, `$shadow-lg` - Shadow styles

These variables are used throughout `_base.scss`, `_components.scss`, and `_layout.scss`.

## Adding a New Theme

To add a new theme:

1. Create a new file: `_sass/_themes-[theme-name].scss`
2. Define all theme variables (see existing themes for reference)
3. Update `_sass/_themes.scss` to import the new theme
4. Test the theme by rebuilding Jekyll

## History

- **2026-02-03**: Created theme system with Visual Cinnamon and Warm Sunset themes
- Visual Cinnamon theme implemented based on visualcinnamon.com style
- Warm Sunset theme preserved from original site design

## Notes

- Both themes are preserved and can be switched at any time
- Theme switching only requires editing one file (`_themes.scss`)
- All theme files are version controlled in Git
- Font changes are handled in `_base.scss` and `_layouts/default.html`
