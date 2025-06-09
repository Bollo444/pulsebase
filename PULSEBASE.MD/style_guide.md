# PulseBase Style Guide

This document outlines the visual and interaction design guidelines for the PulseBase platform. Adhering to this style guide will ensure a consistent and cohesive user experience across all parts of the application.

## 1. Color Palette

### 1.1. Primary Colors
- **Primary Brand Color:** `#D2691E` (Dark Orange / Chocolate) - Used for main calls to action, active states, and key branding elements.
- **Secondary Brand Color:** `#A9A9A9` (Dark Gray) - Used for secondary actions, highlights, and less prominent branding elements.

### 1.2. Neutral Colors
- **Darkest Grey (Text):** `#333333` - For body text and primary content.
- **Medium Grey (Sub-text/Borders):** `#777777` - For secondary text, icons, and subtle borders.
- **Light Grey (Backgrounds/Dividers):** `#F5F5F5` - For page backgrounds, card backgrounds, and dividers.
- **White:** `#FFFFFF` - For text on dark backgrounds and main content areas.

### 1.3. Semantic Colors
- **Success/Confirmation:** `#28A745` (Green) - For success messages, validation, and positive confirmations.
- **Warning/Alert:** `#FFC107` (Yellow/Amber) - For warnings, alerts, and important notices.
- **Error/Danger:** `#DC3545` (Red) - For error messages, destructive actions, and critical alerts.
- **Information:** `#17A2B8` (Blue/Teal) - For informational messages and neutral notifications.

## 2. Typography

### 2.1. Font Family
- **Primary Font:** `Consolas` (Similar to VS Code's default on Windows, which Trae is based on) - Used for all UI text, including headings, body copy, and labels.
  - *Fallback:* `Menlo, Monaco, 'Lucida Console', 'Liberation Mono', 'DejaVu Sans Mono', 'Bitstream Vera Sans Mono', 'Courier New', monospace, sans-serif`

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

- **Icon Library:** `Feather Icons` or `Heroicons` (Recommended for a clean, modern, and versatile style, similar to the aesthetic often found in developer tools. Both offer SVG icons which are scalable and customizable).
- **Style:** `Outline` (This style is generally lightweight and contemporary, aligning well with a developer-focused platform. Ensure consistency if an alternative is chosen).
- **Default Size:** `24x24px` (with variations for smaller/larger contexts, e.g., `16x16px`, `20x20px`, `32x32px`).
- **Color:** Typically use Medium Grey (`#777777`) for standalone icons. For icons within buttons or interactive elements, they should inherit the color of the parent element's text or be explicitly set for contrast and meaning (e.g., white on a dark primary button).
- **Usage:** Icons should be clear, universally understandable, and used consistently for similar actions/meanings. Ensure icons are accompanied by text labels where ambiguity might exist, especially for less common actions. Consider accessibility by providing appropriate ARIA labels for icons used as interactive elements.

## 4. UI Components

This section will detail the styling for common UI components, aiming for a modern, appealing, and interactive experience. Animations, gradients, translucency, and rounded borders should be used thoughtfully to enhance usability and visual appeal. For each component, specify states (default, hover, active, disabled, focus).

**General Principles for UI Components:**
- **Animations:** Utilize subtle animations for state transitions (e.g., hover, focus, active) to provide feedback and create a dynamic feel. Avoid overly distracting or lengthy animations.
- **Gradients & Translucency:** Apply gradients and translucency to elements like buttons, table backgrounds, or card backgrounds where appropriate to add depth and visual interest. Ensure readability and accessibility are maintained.
- **Rounded Borders:** Prefer rounded borders for most elements (buttons, inputs, cards) to create a softer, more modern look. Default to a moderate radius (e.g., `4px` or `8px`) and adjust as needed for specific components.
- **Hover Effects:** Implement clear and engaging hover effects, potentially including subtle animations (e.g., slight scaling, shadow changes, border emphasis).

### 4.1. Buttons
- **General Button Style:**
  - **Border Radius:** `8px` (default, can be adjusted for specific button types/sizes).
  - **Transitions:** Smooth transitions for hover, active, and focus states (e.g., `transition: all 0.2s ease-in-out;`).
  - **Hover Animation:** Consider slight lift/scale or border/shadow animation on hover.
- **Primary Button:**
  - Background: Gradient from Primary Brand Color (`#D2691E`) to a slightly lighter/darker shade, or a translucent overlay on an image/pattern if applicable. Default to solid Primary Brand Color if gradient is too complex for initial implementation.
  - Text: White (`#FFFFFF`).
  - Border: None, or 1px solid transparent/slightly darker shade of Primary Brand Color.
  - Padding: `12px 24px` (adjust based on font size and context).
  - Hover: Slightly lighter gradient or increased shadow, subtle animation.
  - Active: Slightly darker gradient or inset shadow.
- **Secondary Button:**
  - Background: Translucent White (`rgba(255, 255, 255, 0.1)`) over a darker background, or a light gradient. Alternatively, outline style with Primary Brand Color border.
  - Text: Primary Brand Color (`#D2691E`) or Darkest Grey (`#333333`).
  - Border: 1px solid Primary Brand Color (`#D2691E`) or a subtle translucent border.
  - Padding: `12px 24px`.
  - Hover: Background becomes slightly more opaque or border color intensifies, subtle animation.
- **Tertiary/Text Button:**
  - Background: Transparent.
  - Text: Primary Brand Color (`#D2691E`).
  - Border: None.
  - Padding: `8px 16px`.
  - Hover: Text underline or slight background highlight (e.g., very light, translucent Primary Brand Color), subtle animation.
- **Destructive Button:**
  - Background: Gradient from Error/Danger Color (`#DC3545`) to a slightly darker shade.
  - Text: White (`#FFFFFF`).
  - Border: None.
  - Padding: `12px 24px`.
  - Hover: Slightly lighter gradient or increased shadow, subtle animation.

### 4.2. Input Fields (Text, Select, Textarea)
- **General Input Style:**
  - **Border Radius:** `6px`.
  - **Transitions:** Smooth transitions for focus and error states.
- **Default State:**
  - Background: White (`#FFFFFF`) or very light, slightly translucent grey.
  - Text: Darkest Grey (`#333333`).
  - Border: 1px solid Medium Grey (`#777777`).
  - Padding: `10px 12px`.
  - **Placeholder Text (Ghost Text):** Use descriptive examples (e.g., "Enter your email address", "Search..."). Color: Medium Grey (`#777777`).
- **Focus State:**
  - Border: 2px solid Primary Brand Color (`#D2691E`).
  - Box Shadow: Subtle inner or outer glow with Primary Brand Color (e.g., `0 0 0 2px rgba(210, 105, 30, 0.2)`).
  - Animation: Border color/shadow transition.
- **Error State:**
  - Border: 2px solid Error/Danger Color (`#DC3545`).
  - Text (if displaying error message within field): Error/Danger Color (`#DC3545`).
  - **Persistence:** Error styling and associated messages should remain visible until the error is corrected by the user.
- **Disabled State:**
  - Background: Light Grey (`#F5F5F5` with slight translucency if possible).
  - Text: Medium Grey (`#777777`).
  - Border: 1px solid Light Grey (`#F5F5F5`).

### 4.3. Cards & Containers
- Background: White (`#FFFFFF`) or a very light, slightly translucent shade of Light Grey (`#F5F5F5`). Consider subtle gradients for larger container backgrounds.
- Border: 1px solid Light Grey (`#F5F5F5`) or none if using a distinct box shadow.
- Border Radius: `8px` to `12px` (larger radius for larger cards).
- Box Shadow: Modern, diffused shadow (e.g., `0 4px 12px rgba(0,0,0,0.08)`). Consider animated shadow on hover.
- Padding: `16px` to `24px` (internal padding, consistent with spacing units).
- Hover Animation: Slight lift (transform: `translateY(-2px)`), increased shadow, or subtle border highlight.

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