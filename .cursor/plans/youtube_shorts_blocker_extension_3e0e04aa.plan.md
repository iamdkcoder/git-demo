---
name: YouTube Shorts Blocker Extension
overview: Build a Chrome extension that hides YouTube Shorts from all YouTube pages using content scripts and DOM manipulation with MutationObserver for dynamically loaded content.
todos:
  - id: create-manifest
    content: Create manifest.json with Chrome Extension Manifest V3 configuration, content scripts, and required permissions
    status: pending
  - id: create-content-script
    content: Implement content.js with Shorts detection logic, DOM manipulation, and MutationObserver for dynamic content
    status: pending
  - id: create-styles
    content: Create styles.css with CSS rules to hide Shorts elements as a fallback method
    status: pending
  - id: create-readme
    content: Create README.md with installation instructions and usage guide
    status: pending
---

# YouTube Shorts Blocker Extension

A Chrome extension that hides YouTube Shorts from all YouTube pages by detecting and removing Shorts elements from the DOM.

## Architecture

The extension uses:

- **manifest.json**: Chrome extension configuration with content scripts and permissions
- **content.js**: Main script that detects and hides Shorts elements
- **styles.css**: Optional CSS for additional hiding (backup method)

## Implementation Details

### 1. Extension Manifest (`manifest.json`)

- Chrome Extension Manifest V3 format
- Content script that runs on all YouTube pages (`*://*.youtube.com/*`)
- Permissions: `activeTab` (to access YouTube pages)
- Content script injection on document_idle for performance

### 2. Content Script (`content.js`)

Main logic to hide Shorts:

**Detection Methods:**

- URL pattern: `/shorts/` in links and iframe sources
- DOM selectors: YouTube Shorts containers (e.g., `a[href*="/shorts/"]`, Shorts shelf sections)
- Data attributes: YouTube's internal Shorts indicators

**Hiding Strategy:**

- Initial scan on page load
- MutationObserver to watch for dynamically loaded content (infinite scroll)
- Remove or hide parent containers of Shorts links
- Handle both thumbnail links and full Shorts player pages

**Key Features:**

- Removes Shorts from homepage feed
- Hides Shorts shelf sections
- Blocks Shorts in search results
- Handles sidebar Shorts recommendations
- Works with YouTube's dynamic content loading

### 3. Styles (`styles.css`)

- CSS rules as a fallback to hide any missed Shorts elements
- Targets common Shorts container classes

## File Structure

```
shorts-blockerExtension/
├── manifest.json          # Extension configuration
├── content.js            # Main content script
├── styles.css            # CSS fallback styles
└── README.md            # Installation and usage instructions
```

## Technical Approach

1. **Content Script Injection**: Runs on all YouTube pages at `document_idle`
2. **Element Detection**: Scans for Shorts using multiple selectors:

   - Links: `a[href*="/shorts/"]`
   - Containers: Parent elements of Shorts links
   - YouTube's Shorts shelf sections

3. **Dynamic Content**: Uses MutationObserver to detect new Shorts as they're loaded
4. **Removal**: Hides elements by setting `display: none` or removing from DOM

## Testing Considerations

- Test on YouTube homepage with infinite scroll
- Test on search results page
- Test on channel pages
- Test on video watch pages (sidebar recommendations)
- Verify no performance impact on page loading