# Frontend Changes: Light Mode Toggle

## Feature Summary
Added a light mode toggle button to the frontend that allows users to switch between dark and light themes. The user's preference is persisted in localStorage.

## Files Modified

### 1. `frontend/style.css`
- Added `--code-bg` CSS variable to both dark and light mode themes for consistent code block styling
- Added `:root.light-mode` CSS class with light theme color variables:
  - Light background (`#f8fafc`)
  - White surface (`#ffffff`)
  - Dark text (`#1e293b`)
  - Subtle borders (`#e2e8f0`)
  - Adjusted shadows for light backgrounds
- Added `.theme-toggle` button styles matching the sidebar aesthetic
- Added `.theme-toggle-icon` styles for sun/moon icons
- Added icon visibility rules to show sun icon in dark mode, moon icon in light mode
- Updated `.message-content code` and `.message-content pre` to use `var(--code-bg)` variable

### 2. `frontend/index.html`
- Added theme toggle button in the sidebar (after "New Chat" button)
- Button includes:
  - Sun icon (SVG) - displayed in dark mode
  - Moon icon (SVG) - displayed in light mode
  - Text label showing "Light Mode" or "Dark Mode"
- Added `aria-label` for accessibility
- Updated cache-busting version numbers for CSS and JS files (v9 -> v10)

### 3. `frontend/script.js`
- Added `themeToggle` DOM element reference
- Added `initializeTheme()` function - loads saved theme from localStorage on page load
- Added `toggleTheme()` function - toggles light-mode class and saves preference
- Added `updateThemeToggleText()` function - updates button text based on current mode
- Added theme toggle click event listener in `setupEventListeners()`
- Called `initializeTheme()` in DOMContentLoaded handler

## How It Works
1. On page load, `initializeTheme()` checks localStorage for a saved theme preference
2. If "light" theme was saved, adds `light-mode` class to `<html>` element
3. CSS variables automatically update based on presence of `.light-mode` class
4. Clicking the toggle button switches the theme and saves the preference
5. The button text and icon update to show the opposite mode (what clicking will switch to)

## User Experience
- Default theme: Dark mode
- Toggle location: Sidebar, below "New Chat" button
- Persistence: Theme preference survives page refreshes and browser sessions
- Visual feedback: Button shows sun icon + "Light Mode" text in dark mode, moon icon + "Dark Mode" text in light mode
