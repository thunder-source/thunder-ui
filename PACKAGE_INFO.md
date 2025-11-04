# Swift UI Component Library - Package Information

A comprehensive React component library built for modern web applications, featuring a rich collection of UI primitives, custom components, and layouts powered by Radix UI, Tailwind CSS, and TypeScript.

## Table of Contents

- [Package Overview](#package-overview)
- [Package Structure](#package-structure)
- [Installation](#installation)
- [Build System](#build-system)
- [Component Architecture](#component-architecture)
- [Development Workflow](#development-workflow)
- [Dependencies](#dependencies)
- [Usage Examples](#usage-examples)
- [TypeScript Support](#typescript-support)
- [Best Practices](#best-practices)

## Package Overview

**Package Name**: `@hrms/components` (or your configured package name)  
**Version**: See `package.json`  
**License**: MIT  
**Build Tool**: Rollup + TypeScript + Tailwind CSS    
**Module Formats**: ESM and CommonJS  
**Framework**: React 18+

### Key Features

- 🎨 **80+ Production-Ready Components** - From basic UI primitives to complex business components
- 📦 **Tree-Shakeable** - Import only what you need, reducing bundle size
- 🎭 **TypeScript First** - Full type safety with comprehensive type definitions
- 🎯 **Accessible** - Built on Radix UI primitives with ARIA compliance
- 🎨 **Customizable** - Tailwind CSS for easy theming and styling
- 📖 **Well Documented** - Comprehensive Storybook documentation
- ⚡ **Performance Optimized** - Minimal runtime overhead
- 🔧 **Developer Friendly** - Excellent DX with IntelliSense support

## Package Structure

```
swift-ui/
├── src/                          # Source files
│   ├── components/               # All component source files
│   │   ├── base/                # Base application components
│   │   │   ├── announcementCard/
│   │   │   ├── avatarUploadImage/
│   │   │   ├── calenderPlus/
│   │   │   ├── checkboxField/
│   │   │   ├── inputField/
│   │   │   ├── notification/
│   │   │   ├── statCard/
│   │   │   └── ... (15+ components)
│   │   ├── ui/                  # UI primitives (Radix-based)
│   │   │   ├── alert/
│   │   │   ├── avatar/
│   │   │   ├── button/
│   │   │   ├── calendar/
│   │   │   ├── card/
│   │   │   ├── dialog/
│   │   │   ├── dropdown-menu/
│   │   │   ├── form/
│   │   │   ├── input/
│   │   │   ├── select/
│   │   │   ├── table/
│   │   │   └── ... (30+ primitives)
│   │   ├── custom/              # Custom business components
│   │   │   ├── actionModal/
│   │   │   ├── barGraphCard/
│   │   │   ├── filter/
│   │   │   ├── formModal/
│   │   │   ├── header/
│   │   │   ├── sidebar/
│   │   │   ├── tableCard/
│   │   │   └── ... (15+ components)
│   │   ├── layout/              # Layout components
│   │   │   ├── appLayout.tsx
│   │   │   └── rootLayout.tsx
│   │   ├── common/              # Common utilities
│   │   │   ├── roleGuard.tsx
│   │   │   └── withRole.tsx
│   │   └── dev/                 # Development tools
│   │       └── roleSwitcher.tsx
│   ├── assets/                  # Static assets
│   │   └── svg/                 # SVG icons and images
│   ├── lib/                     # Utility functions
│   │   ├── index.ts
│   │   └── utils.ts             # cn() and other helpers
│   ├── constants/               # Constants and configuration
│   │   ├── base/
│   │   ├── custom/
│   │   ├── filter.ts
│   │   ├── inputField.ts
│   │   └── regex.ts
│   ├── types/                   # TypeScript type definitions
│   │   ├── api.ts
│   │   ├── filter.ts
│   │   ├── props.ts
│   │   ├── roles.ts
│   │   ├── states.ts
│   │   └── table.ts
│   ├── utils/                   # Utility functions
│   │   ├── dateFormatter.ts
│   │   ├── form.ts
│   │   ├── roleUtils.ts
│   │   └── typeUtils.ts
│   ├── hooks/                   # React hooks
│   │   ├── use-mobile.ts
│   │   └── useErrorBoundary.tsx
│   ├── styles/                  # Styling
│   │   ├── globals.css          # Global styles
│   │   ├── app.css              # Application styles
│   │   └── index.css            # Style entry point
│   └── index.ts                 # Main entry point
│
├── dist/                         # Build output (generated, gitignored)
│   ├── index.js                 # CommonJS bundle
│   ├── index.esm.js             # ESM bundle
│   ├── index.d.ts               # Root TypeScript definitions
│   ├── styles.css               # Compiled CSS
│   ├── components/              # Type definitions mirror source
│   ├── types/
│   ├── utils/
│   └── ... (all .d.ts files)
│
├── public/                       # Public assets (dev only)
│   ├── icons/
│   └── images/
│
├── scripts/                      # Build and utility scripts
│   ├── fix-git-rename.sh
│   └── rename-to-lowercase.js
│
├── Configuration Files
├── package.json                 # Package manifest
├── tsconfig.json                # TypeScript base config
├── tsconfig.build.json          # Build-specific TS config
├── tsconfig.app.json            # App-specific TS config
├── tsconfig.node.json           # Node-specific TS config
├── rollup.config.js             # Rollup bundler configuration
├── vite.config.ts               # Vite configuration
├── tailwind.config.js           # Tailwind CSS configuration
├── postcss.config.cjs           # PostCSS configuration
├── biome.json                   # Biome linter/formatter config
├── eslint.config.js             # ESLint configuration
│
├── Documentation
├── README.md                    # Main documentation
├── PACKAGE_INFO.md              # This file
├── PUBLISHING.md                # Publishing guide
│
└── .npmignore                   # npm publish exclusions
```

## Installation

### For End Users

Install the package in your React application:

```bash
# Using npm
npm install @hrms/components

# Using pnpm
pnpm add @hrms/components

# Using yarn
yarn add @hrms/components
```

### Peer Dependencies

Ensure your project has the required peer dependencies:

```bash
npm install react@^18.0.0 react-dom@^18.0.0
```

### Import Styles

Add the compiled styles to your application entry point:

```tsx
// In your main App.tsx or index.tsx
import '@hrms/components/dist/styles.css';
```

## Build System

The package uses a modern build pipeline optimized for both development and production.

### Build Pipeline Overview

```
Source (src/) → TypeScript Compiler → Type Definitions (.d.ts)
                     ↓
              Rollup Bundler → ESM Bundle (index.esm.js)
                     ↓
                          → CommonJS Bundle (index.js)
                     ↓
              Tailwind CSS → Compiled Styles (styles.css)
                     ↓
              Output (dist/)
```

### Build Process Details

1. **TypeScript Compilation**
   - Compiles all `.ts` and `.tsx` files
   - Generates comprehensive `.d.ts` type definition files
   - Maintains source directory structure in `dist/`
   - Enables autocomplete and IntelliSense in consuming applications

2. **Bundle Creation (Rollup)**
   - **ESM Bundle** (`index.esm.js`) - For modern bundlers (Webpack 5+, Vite, etc.)
   - **CommonJS Bundle** (`index.js`) - For Node.js and older build systems
   - Tree-shaking enabled for optimal bundle sizes
   - Source maps included for debugging

3. **CSS Compilation**
   - Tailwind CSS processes all component styles
   - Compiles to a single `styles.css` file
   - Includes all necessary utilities and custom styles
   - PostCSS optimizations applied

### Available Build Commands

```bash
# Full build (recommended before publishing)
pnpm run build

# Individual build steps
pnpm run build:types      # TypeScript declarations only
pnpm run build:bundle     # JavaScript bundles only  
pnpm run build:css        # CSS compilation only

# Development
pnpm run dev              # Development mode with watch
pnpm run storybook        # Launch Storybook

# Quality checks
pnpm run lint             # Run ESLint and Biome
pnpm run format           # Format code with Biome
pnpm run type-check       # TypeScript type checking
```

### Build Output Verification

After building, verify the `dist/` folder contains:

```
dist/
├── index.js              ✓ CommonJS bundle
├── index.esm.js          ✓ ESM bundle
├── index.js.map          ✓ Source map (CJS)
├── index.esm.js.map      ✓ Source map (ESM)
├── index.d.ts            ✓ Root type definitions
├── styles.css            ✓ Compiled styles
├── components/           ✓ Component type definitions
├── types/                ✓ Type definitions
└── utils/                ✓ Utility type definitions
```

## Component Architecture

### Component Categories

#### 1. **Base Components** (`src/components/base/`)

Application-specific components designed for HRMS and business applications:

- `AnnouncementCard` - Display announcements and updates
- `AvatarUploadImage` - Avatar upload with preview
- `CalenderPlus` - Extended calendar functionality
- `CheckboxField` - Form checkbox with validation
- `FileImageViewer` - File and image preview
- `InputField` - Enhanced input with validation
- `Notification` - Notification display
- `StatCard` - Statistical data cards
- `Stepper` - Multi-step form stepper
- `UserProfile` - User profile display
- And more...

#### 2. **UI Components** (`src/components/ui/`)

Foundational UI primitives built on Radix UI:

- **Form Controls**: Button, Input, Select, Checkbox, Switch, Slider
- **Data Display**: Table, Card, Badge, Avatar, Tooltip
- **Overlays**: Dialog, Modal, Popover, Sheet, Dropdown Menu
- **Feedback**: Alert, Toast, Progress, Skeleton
- **Navigation**: Tabs, Sidebar, Pagination
- **Data Visualization**: Chart, BarGraph
- **Date/Time**: Calendar, Date Picker
- And more...

#### 3. **Custom Components** (`src/components/custom/`)

Complex business components combining multiple primitives:

- `ActionModal` - Action confirmation modals
- `BarGraphCard` - Data visualization cards
- `Filter` - Advanced filtering system
- `FormComponent` - Dynamic form builder
- `FormModal` - Modal with form handling
- `Header` - Application header
- `Sidebar` - Navigation sidebar
- `TableCard` - Data tables with actions
- `TabsHeader` - Tabbed navigation
- `UploadInput` - File upload with validation
- And more...

#### 4. **Layout Components** (`src/components/layout/`)

Structural components for application layout:

- `AppLayout` - Main application layout wrapper
- `RootLayout` - Root-level layout provider

#### 5. **Common Utilities** (`src/components/common/`)

Reusable utilities and HOCs:

- `RoleGuard` - Role-based component protection
- `withRole` - HOC for role-based rendering

#### 6. **Development Tools** (`src/components/dev/`)

Development-only helpers:

- `RoleSwitcher` - Switch user roles in development

## Development Workflow

### Setting Up Development Environment

```bash
# Clone the repository
git clone <repository-url>
cd swift-ui

# Install dependencies (using pnpm)
pnpm install

# Start Storybook for component development
pnpm run storybook
```

### Storybook Development

Storybook provides an isolated environment for component development:

```bash
pnpm run storybook
```

Access at: `http://localhost:6006`

**Benefits**:
- Develop components in isolation
- Test different component states
- Interactive documentation
- Visual regression testing
- Accessibility testing

### Adding New Components

1. **Create component directory**:
   ```bash
   src/components/[category]/[componentName]/
   ├── [componentName].tsx       # Component implementation
   ├── [componentName].stories.tsx  # Storybook stories
   ├── types.ts                  # Component-specific types
   └── index.ts                  # Exports
   ```

2. **Export from category index**:
   ```tsx
   // src/components/[category]/index.ts
   export * from './[componentName]';
   ```

3. **Export from main index**:
   ```tsx
   // src/index.ts
   export * from './components/[category]';
   ```

### Code Quality

```bash
# Linting
pnpm run lint          # Check for issues
pnpm run lint:fix      # Auto-fix issues

# Formatting
pnpm run format        # Format all files

# Type checking
pnpm run type-check    # Verify TypeScript types
```

## Dependencies

### Peer Dependencies (Required in consuming app)

```json
{
  "react": "^18.0.0 || ^19.0.0",
  "react-dom": "^18.0.0 || ^19.0.0"
}
```

⚠️ **Important**: Consuming applications must install these dependencies.

### Core Dependencies (Bundled)

#### UI Framework & Components
- `@radix-ui/*` - Accessible UI primitives (Dialog, Dropdown, Select, etc.)
- `lucide-react` - Icon library
- `recharts` - Chart and data visualization

#### Styling
- `tailwindcss` - Utility-first CSS framework
- `tailwind-merge` - Merge Tailwind classes
- `clsx` - Conditional class names
- `class-variance-authority` - Variant-based styling

#### Forms & Validation
- `react-hook-form` - Form state management
- `@hookform/resolvers` - Validation resolvers
- `zod` - Schema validation

#### Date & Time
- `date-fns` - Date utility library
- `react-day-picker` - Date picker component

#### Data Tables
- `@tanstack/react-table` - Powerful table library

### Development Dependencies

- TypeScript, Rollup, Vite - Build tools
- Storybook - Component development
- Biome, ESLint - Code quality
- And more...

## Usage Examples

### Basic Import

```tsx
import { Button, Card, Input } from '@hrms/components';
import '@hrms/components/dist/styles.css';

function App() {
  return (
    <Card>
      <Input placeholder="Enter text" />
      <Button>Submit</Button>
    </Card>
  );
}
```

### Tree-Shaking (Recommended)

Modern bundlers automatically tree-shake unused components:

```tsx
// Only Button and Card will be included in your bundle
import { Button, Card } from '@hrms/components';
```

### Component Categories

```tsx
// Base components
import { InputField, StatCard, Notification } from '@hrms/components';

// UI primitives
import { Button, Dialog, Select } from '@hrms/components';

// Custom components
import { TableCard, FormModal, Sidebar } from '@hrms/components';

// Layouts
import { AppLayout, RootLayout } from '@hrms/components';

// Utilities
import { cn } from '@hrms/components';
```

### Type Imports

```tsx
import type { 
  ButtonProps, 
  TableColumn, 
  FilterConfig,
  UserRole 
} from '@hrms/components';
```

### Using with TypeScript

Full type safety and IntelliSense support:

```tsx
import { Button } from '@hrms/components';
import type { ButtonProps } from '@hrms/components';

const MyButton: React.FC<ButtonProps> = (props) => {
  return <Button variant="default" size="lg" {...props} />;
};
```

## TypeScript Support

### Type Definitions

Complete type definitions are provided for:
- All component props
- Utility functions
- Hooks
- Constants and enums
- API types

### Path Aliases

Internal path alias `@/` is resolved during build:

```tsx
// In source (src/)
import { cn } from '@/lib/utils';

// Becomes in dist/
import { cn } from './lib/utils';
```

### Strict Mode

The package is built with TypeScript strict mode enabled for maximum type safety.

## Best Practices

### For Package Maintainers

1. **Always build before publishing**
   ```bash
   pnpm run build
   ```

2. **Test components in Storybook**
   ```bash
   pnpm run storybook
   ```

3. **Run quality checks**
   ```bash
   pnpm run lint
   pnpm run type-check
   ```

4. **Follow semantic versioning**
   - Major: Breaking changes
   - Minor: New features (backward compatible)
   - Patch: Bug fixes

5. **Document new components** - Add Storybook stories

### For Package Consumers

1. **Import styles once** - In your app entry point
2. **Use tree-shaking** - Import only what you need
3. **Leverage TypeScript** - Use provided type definitions
4. **Follow accessibility guidelines** - Components are accessible by default
5. **Customize with Tailwind** - Use Tailwind utilities for styling

## Troubleshooting

### Build Issues

**TypeScript errors during build**:
- Run `pnpm run type-check` to identify issues
- Check `tsconfig.build.json` configuration

**Missing dependencies**:
- Run `pnpm install` to ensure all dependencies are installed
- Check for peer dependency warnings

**CSS not applying**:
- Ensure you're importing `@hrms/components/dist/styles.css`
- Check Tailwind configuration in consuming app

### Import Issues

**Module not found**:
- Verify the component is exported in `src/index.ts`
- Check the component path in your import statement
- Ensure the package is built (`dist/` folder exists)

**Type definitions not working**:
- Check that `dist/index.d.ts` exists
- Verify TypeScript can resolve the package
- Restart your TypeScript server/IDE

## Additional Resources

- **README.md** - Getting started guide
- **PUBLISHING.md** - Publishing and release guide
- **Storybook** - Interactive component documentation
- **package.json** - Package configuration and scripts

## Support

For issues, questions, or contributions, please contact the package maintainers or refer to the project repository.
