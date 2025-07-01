# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Development Commands

```bash
# Development workflow
npm run dev          # Start Vite dev server on localhost:5173
npm run build        # Build for production (outputs to docs/ directory)
npm run preview      # Preview production build locally
npm run lint         # Run ESLint across the codebase

# Package management
npm install          # Install all dependencies
```

## Architecture Overview

This is a React 19 application showcasing "Stargazers" - an interactive character gallery with modal interactions. The architecture follows a centralized state pattern with props drilling for data flow.

### Core State Management Pattern

**App.jsx** serves as the central state holder:
- `cast` state: Array of character data loaded from `public/cast.json`
- `memberInfo` state: Currently selected character for modal display
- Data flows down through props, events bubble up through callbacks

### Component Architecture

```
App (Central state + data fetching)
├── Nav (Theme toggle + character dropdown)
│   └── ToggleTheme (Three-state theme system: auto/light/dark)
├── Modals (Character detail popup with navigation)
└── ListCast (Character grid display)
```

### Key Technical Patterns

**Theme System**: Three-state theme management (auto/light/dark) with localStorage persistence and system preference detection via `matchMedia`. Themes are applied by setting `data-theme` attribute on document element.

**Data Loading**: Uses native fetch API to load character data from `cast.json`. Character images follow naming convention: `{character.slug}.svg` for full images, `{character.slug}_tn.svg` for thumbnails.

**Modal State**: Conditional rendering based on `memberInfo` state. Modal includes navigation logic to cycle through characters and proper cleanup on close.

**Styling Strategy**: Uses PicoCSS as base framework with CSS custom properties for theming. Component-specific styles are exported from `InterfaceStyles.jsx` as JavaScript objects.

### Build Configuration

**Vite Setup**: Configured with React Compiler plugin for automatic memoization. Builds to `docs/` directory with relative paths for GitHub Pages deployment. Base path set to `./` for subdirectory hosting.

**React Compiler**: Babel plugin `babel-plugin-react-compiler` automatically optimizes React components without manual memoization.

### Data Structure

Characters in `cast.json` follow this structure:
```javascript
{
  id: number,
  name: string,
  slug: string,    // Used for image file naming
  bio: string,
  origin: string,
  // Additional character attributes
}
```

### Common Development Patterns

- All components use function declarations (not arrow functions)
- Props are destructured at the parameter level
- Event handlers use inline arrow functions for simple interactions
- Conditional rendering uses `&&` operator and ternary expressions
- No external state management library - relies on React's built-in state