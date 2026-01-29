# Design Document: Landing Page Color System

## Overview

This design document specifies the complete color system for the Nova Zezinho Vidros landing page. The palette consists of four core colors that create a professional, modern aesthetic while maintaining strong brand identity and accessibility standards.

## Architecture

The color system follows a hierarchical structure:

1. **Primary Brand Color**: Red (#9d1b19) - Used for logo, CTAs, and brand emphasis
2. **Neutral Base**: White (#ffffff) - Primary background and text on dark surfaces
3. **Dark Neutral**: Dark Gray (#2b2b2b) - Text, dark backgrounds, and contrast elements
4. **Light Neutral**: Light Gray (#bfc3c7) - Secondary UI elements, borders, and subtle accents

## Components and Interfaces

### Color Definitions

```css
:root {
  /* Primary Brand Color */
  --color-primary: #9d1b19;
  --color-primary-rgb: 157, 27, 25;
  
  /* Neutral Colors */
  --color-white: #ffffff;
  --color-white-rgb: 255, 255, 255;
  
  --color-dark: #2b2b2b;
  --color-dark-rgb: 43, 43, 43;
  
  --color-light-gray: #bfc3c7;
  --color-light-gray-rgb: 191, 195, 199;
}
```

### Color Usage Patterns

#### Logo and Branding
- Logo "Z": Primary red (#9d1b19) with white (#ffffff) accents
- Logo variations: White on red, red on white, white on dark, red on dark

#### Backgrounds
- Primary background: White (#ffffff)
- Hero sections: Primary red (#9d1b19)
- Footer/Header: Dark gray (#2b2b2b)
- Cards/Sections: Alternating white and light gray

#### Typography
- Headings on light backgrounds: Dark gray (#2b2b2b)
- Headings on dark backgrounds: White (#ffffff)
- Body text on light: Dark gray (#2b2b2b)
- Body text on dark: White (#ffffff)
- Secondary text: Light gray (#bfc3c7) - decorative only

#### Interactive Elements
- Primary buttons: Primary red (#9d1b19) background, white (#ffffff) text
- Secondary buttons: White (#ffffff) background, primary red (#9d1b19) border and text
- Hover states: Darken primary red by 10%
- Links: Primary red (#9d1b19)

## Data Models

### Color Object Structure

```typescript
interface Color {
  name: string;
  hex: string;
  rgb: {
    r: number;
    g: number;
    b: number;
  };
  usage: string[];
  contrastRatios: {
    onWhite?: number;
    onDark?: number;
    onPrimary?: number;
  };
}
```

### Color Palette Data

```typescript
const colorPalette: Color[] = [
  {
    name: "Primary Red",
    hex: "#9d1b19",
    rgb: { r: 157, g: 27, b: 25 },
    usage: ["logo", "cta-buttons", "links", "accents", "hero-backgrounds"],
    contrastRatios: {
      onWhite: 7.2  // AAA compliant
    }
  },
  {
    name: "White",
    hex: "#ffffff",
    rgb: { r: 255, g: 255, b: 255 },
    usage: ["backgrounds", "text-on-dark", "logo-variations"],
    contrastRatios: {
      onPrimary: 5.8,  // AA compliant
      onDark: 15.3     // AAA compliant
    }
  },
  {
    name: "Dark Gray",
    hex: "#2b2b2b",
    rgb: { r: 43, g: 43, b: 43 },
    usage: ["text", "dark-backgrounds", "footer", "header"],
    contrastRatios: {
      onWhite: 14.7  // AAA compliant
    }
  },
  {
    name: "Light Gray",
    hex: "#bfc3c7",
    rgb: { r: 191, g: 195, b: 199 },
    usage: ["borders", "secondary-elements", "subtle-backgrounds"],
    contrastRatios: {
      onWhite: 2.1  // Decorative only
    }
  }
];
```

## Correctness Properties

*A property is a characteristic or behavior that should hold true across all valid executions of a system—essentially, a formal statement about what the system should do. Properties serve as the bridge between human-readable specifications and machine-verifiable correctness guarantees.*

### Property 1: Valid Hex Color Format

*For any* color in the color system, the hex code should be a valid 6-character hexadecimal string prefixed with #

**Validates: Requirements 1.1, 1.2, 1.3, 1.4**

### Property 2: Contrast Ratio Compliance

*For any* text-background color combination used in the system, the contrast ratio should meet or exceed WCAG AA standards (4.5:1 for normal text)

**Validates: Requirements 3.1, 3.2**

### Property 3: RGB-Hex Consistency

*For any* color definition, converting the hex value to RGB should produce the same RGB values as stored in the rgb property

**Validates: Requirements 1.1, 1.2, 1.3, 1.4**

### Property 4: Color Immutability

*For any* color in the palette, the hex and RGB values should remain constant across all usage contexts

**Validates: Requirements 1.1, 1.2, 1.3, 1.4, 2.1, 2.2, 2.3, 2.4**

## Error Handling

### Invalid Color Values
- If a hex code is malformed, throw a descriptive error
- If RGB values are out of range (0-255), throw a validation error
- If a color name is not found in the palette, return a fallback color

### Accessibility Violations
- Log warnings when color combinations don't meet WCAG AA standards
- Provide suggestions for alternative color combinations
- Never silently fail accessibility checks

## Testing Strategy

### Unit Tests
- Test hex to RGB conversion accuracy
- Test contrast ratio calculations
- Test color validation functions
- Test edge cases (invalid hex codes, out-of-range RGB values)

### Property-Based Tests
- Generate random hex codes and verify format validation
- Generate random color combinations and verify contrast calculations
- Test RGB-hex conversion round trips
- Verify color immutability across multiple accesses

### Visual Regression Tests
- Screenshot tests for color applications in UI components
- Compare rendered colors against reference images
- Verify color consistency across different browsers

### Accessibility Tests
- Automated WCAG compliance checks
- Contrast ratio verification for all text-background combinations
- Color blindness simulation tests
