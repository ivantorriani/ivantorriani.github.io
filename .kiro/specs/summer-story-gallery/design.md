# Design Document: Summer Story Gallery

## Overview

This design document describes the technical implementation for the "Studies for Summer Story" gallery page feature. The feature adds a new gallery page (`summer-story.html`) to the personal website that displays photos from the `photos/studies/summer_study` directory with caption areas for reflections, along with a navigation button on the homepage.

The design follows a minimal, clean aesthetic consistent with the existing website style, using the EB Garamond font family and a simple color palette.

## Architecture

### High-Level Structure

```
┌─────────────────────────────────────────────────────────────┐
│                      index.html                              │
│  ┌─────────────────────────────────────────────────────┐    │
│  │  Navigation Section                                  │    │
│  │  [About] [LLM Blog] [KeepTalking] [CVF] [NN]        │    │
│  │  [Studies for Summer Story] ← NEW BUTTON            │    │
│  └─────────────────────────────────────────────────────┘    │
└─────────────────────────────────────────────────────────────┘
                              │
                              ▼
┌─────────────────────────────────────────────────────────────┐
│                   summer-story.html                          │
│  ┌─────────────────────────────────────────────────────┐    │
│  │  Header: "Studies for Summer Story"                  │    │
│  │  [← Back to Home]                                    │    │
│  └─────────────────────────────────────────────────────┘    │
│  ┌─────────────────────────────────────────────────────┐    │
│  │  Gallery Container                                   │    │
│  │  ┌─────────────┐  ┌─────────────┐  ┌─────────────┐  │    │
│  │  │   Photo 1   │  │   Photo 2   │  │   Photo 3   │  │    │
│  │  ├─────────────┤  ├─────────────┤  ├─────────────┤  │    │
│  │  │  Caption 1  │  │  Caption 2  │  │  Caption 3  │  │    │
│  │  └─────────────┘  └─────────────┘  └─────────────┘  │    │
│  └─────────────────────────────────────────────────────┘    │
└─────────────────────────────────────────────────────────────┘
```

### File Structure

```
project-root/
├── index.html                    # Homepage (modified)
├── summer-story.html             # New gallery page
├── css/
│   ├── style.css                 # Existing styles
│   └── summer-story.css          # New gallery-specific styles
└── photos/
    └── studies/
        └── summer_study/         # Photo directory (to be created)
            ├── photo1.jpg
            ├── photo2.jpg
            └── ...
```

## Components and Interfaces

### 1. Navigation Button Component

**Location:** `index.html` - within the `<nav>` element

**Structure:**
```html
<a href="summer-story.html">Studies for Summer Story</a>
```

**Styling:** Uses existing `nav a` styles from `css/style.css`:
- White background with dark text
- Rounded corners (4px border-radius)
- Hover state with lighter background

### 2. Gallery Page Component

**Location:** `summer-story.html`

**Structure:**
```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Studies for Summer Story</title>
  <link rel="stylesheet" href="css/style.css">
  <link rel="stylesheet" href="css/summer-story.css">
  <link rel="icon" href="/icon_2.png" type="image/x-icon">
</head>
<body>
  <header class="gallery-header">
    <h1>Studies for Summer Story</h1>
    <nav>
      <a href="index.html">← Back to Home</a>
    </nav>
  </header>
  
  <main class="gallery-container">
    <!-- Photo items rendered here -->
  </main>
  
  <footer>
    <p>&copy; 2025 Ivan</p>
  </footer>
</body>
</html>
```

### 3. Photo Item Component

**Structure:**
```html
<article class="photo-item">
  <img src="photos/studies/summer_study/[filename]" alt="Summer study photo">
  <div class="caption-area">
    <p>[Caption text here]</p>
  </div>
</article>
```

**Behavior:**
- Displays photo at consistent width
- Caption area appears directly below photo
- Maintains aspect ratio of original image

### 4. Gallery Stylesheet Component

**Location:** `css/summer-story.css`

**Responsibilities:**
- Gallery page layout (header, main container)
- Photo item styling (size, spacing)
- Caption area styling (typography, background)
- Responsive adjustments

## Data Models

### Photo Item Model

Since this is a static HTML page, there is no dynamic data model. Photos are referenced directly in the HTML:

| Property | Type | Description |
|----------|------|-------------|
| src | string | Path to image file (e.g., `photos/studies/summer_study/photo1.jpg`) |
| alt | string | Descriptive alt text for accessibility |
| caption | string | Text content for the caption area |

### Directory Structure

Photos are stored in: `photos/studies/summer_study/`

Supported image formats:
- `.jpg` / `.jpeg`
- `.png`
- `.webp`

## Error Handling

### Missing Images

**Scenario:** An image file referenced in HTML does not exist.

**Handling:**
- Browser displays broken image icon
- Alt text provides context
- No JavaScript error handling needed (static page)

### Empty Gallery

**Scenario:** No photos exist in the gallery directory.

**Handling:**
- Page displays header and footer normally
- Empty gallery container shows no content
- Consider adding placeholder text: "No photos available yet"

### Styling Fallbacks

**Scenario:** Custom stylesheet fails to load.

**Handling:**
- Base styles from `style.css` provide readable fallback
- Page remains functional without gallery-specific styles
- EB Garamond font falls back to serif

## Testing Strategy

### Why Property-Based Testing Does Not Apply

This feature consists of static HTML pages and CSS styling. Property-based testing is not appropriate because:

1. **UI Rendering Focus:** The feature is primarily about visual layout and presentation
2. **No Data Transformations:** There are no pure functions processing variable inputs
3. **No Business Logic:** The page simply displays static content
4. **Configuration-Based:** Success depends on correct file paths and CSS rules

### Recommended Testing Approach

#### Manual Visual Testing

1. **Navigation Flow:**
   - Click "Studies for Summer Story" button on homepage
   - Verify navigation to gallery page
   - Click "Back to Home" link
   - Verify return to homepage

2. **Gallery Display:**
   - Verify all photos render at consistent size
   - Verify caption areas appear below each photo
   - Verify adequate whitespace between items

3. **Responsive Testing:**
   - Test on desktop (1920px, 1440px, 1024px widths)
   - Test on tablet (768px width)
   - Test on mobile (375px width)

4. **Cross-Browser Testing:**
   - Chrome
   - Firefox
   - Safari

#### HTML Validation

- Validate `summer-story.html` using W3C Markup Validator
- Ensure semantic HTML structure
- Verify accessibility attributes (alt text, heading hierarchy)

#### CSS Validation

- Validate `css/summer-story.css` using W3C CSS Validator
- Check for syntax errors
- Verify no conflicting rules with main stylesheet

#### Accessibility Testing

- Verify keyboard navigation works
- Check color contrast ratios
- Test with screen reader (VoiceOver/NVDA)
- Ensure images have descriptive alt text

### Test Checklist

| Test Case | Expected Result |
|-----------|-----------------|
| Homepage nav button visible | "Studies for Summer Story" link appears in nav |
| Nav button links correctly | Clicking navigates to `summer-story.html` |
| Gallery page title correct | Page title is "Studies for Summer Story" |
| Back link works | Returns to `index.html` |
| Photos display | All images in directory render |
| Photos sized consistently | All photos have same max-width |
| Captions visible | Caption area appears below each photo |
| Captions styled distinctly | Caption has different background/border from photo |
| Whitespace adequate | Visible spacing between photo items |
| Mobile responsive | Layout adjusts for small screens |
| No console errors | Browser console shows no errors |
