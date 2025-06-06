# PulseBase Style Guide

This document outlines the visual and interaction design guidelines for the PulseBase platform. Adhering to this style guide will ensure a consistent and cohesive user experience across all parts of the application.

## 1. Color Palette

### 1.1. Primary Colors
- **Primary Brand Color:** `#XXXXXX` (e.g., a shade of blue or green) - Used for main calls to action, active states, and key branding elements.
- **Secondary Brand Color:** `#XXXXXX` (e.g., a complementary or accent color) - Used for secondary actions, highlights, and less prominent branding elements.

### 1.2. Neutral Colors
- **Darkest Grey (Text):** `#XXXXXX` (e.g., `#333333`) - For body text and primary content.
- **Medium Grey (Sub-text/Borders):** `#XXXXXX` (e.g., `#777777`) - For secondary text, icons, and subtle borders.
- **Light Grey (Backgrounds/Dividers):** `#XXXXXX` (e.g., `#F5F5F5`) - For page backgrounds, card backgrounds, and dividers.
- **White:** `#FFFFFF` - For text on dark backgrounds and main content areas.

### 1.3. Semantic Colors
- **Success/Confirmation:** `#XXXXXX` (e.g., a shade of green) - For success messages, validation, and positive confirmations.
- **Warning/Alert:** `#XXXXXX` (e.g., a shade of yellow/orange) - For warnings, alerts, and important notices.
- **Error/Danger:** `#XXXXXX` (e.g., a shade of red) - For error messages, destructive actions, and critical alerts.
- **Information:** `#XXXXXX` (e.g., a shade of blue) - For informational messages and neutral notifications.

## 2. Typography

### 2.1. Font Family
- **Primary Font:** `[Font Name]` (e.g., Inter, Roboto, Open Sans) - Used for all UI text, including headings, body copy, and labels.
  - *Fallback:* `sans-serif`

### 2.2. Font Weights & Styles
- **Regular:** `400`
- **Medium:** `500`
- **Semi-Bold:** `600`
- **Bold:** `700`

### 2.3. Font Sizing & Line Height (Base: 16px)
- **H1 (Page Titles):** `32px` / `1.2` line height
- **H2 (Section Titles):** `24px` / `1.3` line height
- **H3 (Sub-Section Titles):** `20px` / `1.4` line height
- **H4 (Card Titles/Important Labels):** `18px` / `1.4` line height
- **Body Text (Default):** `16px` / `1.5` line height
- **Small Text (Captions, Helper Text):** `14px` / `1.5` line height
- **Extra Small Text (Fine Print):** `12px` / `1.5` line height

## 3. Iconography

- **Icon Library:** `[Name of Icon Library]` (e.g., Material Icons, Font Awesome, Feather Icons, or custom SVG set).
- **Style:** `[Outline, Filled, Duotone]`
- **Default Size:** `24x24px` (with variations for smaller/larger contexts, e.g., `16x16px`, `32x32px`).
- **Color:** Typically use Medium Grey (`#777777`) or inherit color from parent text/button.
- **Usage:** Icons should be clear, universally understandable, and used consistently for similar actions/meanings.

## 4. UI Components

This section will detail the styling for common UI components. For each component, specify states (default, hover, active, disabled, focus).

### 4.1. Buttons
- **Primary Button:**
  - Background: Primary Brand Color
  - Text: White
  - Border: None / or 1px solid Primary Brand Color
  - Padding: `Xpx Ypx`
  - Border Radius: `Xpx`
- **Secondary Button:**
  - Background: Secondary Brand Color / or White
  - Text: Primary Brand Color / or Darkest Grey
  - Border: 1px solid Secondary Brand Color / or Primary Brand Color
  - Padding: `Xpx Ypx`
  - Border Radius: `Xpx`
- **Tertiary/Text Button:**
  - Background: Transparent
  - Text: Primary Brand Color
  - Border: None
  - Padding: `Xpx Ypx`
  - Border Radius: `Xpx`
- **Destructive Button:**
  - Background: Error/Danger Color
  - Text: White
  - Border: None
  - Padding: `Xpx Ypx`
  - Border Radius: `Xpx`

### 4.2. Input Fields (Text, Select, Textarea)
- **Default State:**
  - Background: White
  - Text: Darkest Grey
  - Border: 1px solid Light Grey / Medium Grey
  - Padding: `Xpx Ypx`
  - Border Radius: `Xpx`
- **Focus State:**
  - Border: 1px solid Primary Brand Color
  - Box Shadow: Subtle glow with Primary Brand Color
- **Error State:**
  - Border: 1px solid Error/Danger Color
- **Disabled State:**
  - Background: Light Grey (lighter shade)
  - Text: Medium Grey
  - Border: 1px solid Light Grey

### 4.3. Cards & Containers
- Background: White / Light Grey (slightly off-white)
- Border: 1px solid Light Grey (or none if using box shadow)
- Border Radius: `Xpx` (e.g., 4px, 8px)
- Box Shadow: Subtle shadow (e.g., `0 2px 4px rgba(0,0,0,0.1)`)
- Padding: `Xpx` (internal padding)

### 4.4. Modals & Dialogs
- Overlay Background: Dark semi-transparent (e.g., `rgba(0,0,0,0.5)`)
- Modal Background: White
- Border Radius: `Xpx`
- Box Shadow: Prominent shadow to lift it off the page.
- Padding: `Xpx` (internal padding for content)

### 4.5. Navigation (Menus, Tabs)
- **Active State:** Primary Brand Color (text or background highlight)
- **Inactive State:** Medium Grey (text)
- **Hover State:** Light Grey background or subtle underline.

### 4.6. Tables
- Header Background: Light Grey / or subtle shade of Primary Brand Color
- Header Text: Darkest Grey / or White
- Row Borders: 1px solid Light Grey
- Zebra Striping: Optional, using a very light shade of grey for alternate rows.

### 4.7. Notifications & Alerts
- **Success:** Background Success Color, White/Dark text.
- **Warning:** Background Warning Color, Dark text.
- **Error:** Background Error Color, White/Dark text.
- **Info:** Background Info Color, White/Dark text.
- Border Radius: `Xpx`
- Padding: `Xpx`
- Icons: Appropriate semantic icons.

## 5. Spacing & Layout

- **Grid System:** `[e.g., 12-column grid, Bootstrap grid, custom]`
- **Base Spacing Unit:** `8px` (All margins, paddings, and layout spacing should be multiples of this unit, e.g., 4px, 8px, 12px, 16px, 24px, 32px).
- **Consistent Gutters:** Define standard gutter widths between columns and elements.

## 6. Accessibility (A11Y)

- **Color Contrast:** Ensure all text meets WCAG AA contrast ratios (4.5:1 for normal text, 3:1 for large text).
- **Keyboard Navigation:** All interactive elements must be focusable and operable via keyboard.
- **ARIA Attributes:** Use appropriate ARIA roles and attributes for custom components to ensure screen reader compatibility.
- **Focus Indicators:** Visible focus indicators for all focusable elements.

## 7. Tone of Voice

- **Clarity:** Use clear, concise, and straightforward language.
- **Professionalism:** Maintain a professional yet approachable tone.
- **Helpfulness:** Provide helpful guidance and feedback to users.
- **Consistency:** Use consistent terminology throughout the platform.

## 8. Design System (Future Consideration)

As the platform evolves, consider developing a full-fledged design system with reusable React/Vue/Angular components that encapsulate these styles and behaviors. This will improve development efficiency and ensure long-term consistency.

---
*This style guide is a living document and will be updated as the PulseBase platform evolves. Please replace `XXXXXX` and `[Placeholder]` values with actual decisions.*