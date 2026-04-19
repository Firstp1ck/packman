# 📦 PackMan TUI - Specification

## Project Overview
- **Project Name**: PackMan TUI
- **Type**: Terminal User Interface Application (Rust)
- **Core Functionality**: A unified dashboard for managing packages across multiple package managers in one terminal interface
- **Target Users**: Developers and system administrators who work with multiple package ecosystems

## Supported Package Managers
- **pip** (Python)
- **npm** (Node.js)
- **cargo** (Rust)
- **apt** (Debian/Ubuntu)
- **pacman** (Arch Linux)
- **aur** (Arch User Repository - via yay/paru)
- **rpm** (RHEL/Fedora)
- **flatpak** (Universal Linux)
- **snap** (Ubuntu)

## UI/UX Specification

### Layout Structure
- **Header**: App title "📦 PackMan", current distro indicator, system info
- **Sidebar**: Package manager tabs (vertical navigation)
- **Main Content**: Package list/table with search and filters
- **Status Bar**: Selected package details, action hints, version info

### Visual Design
- **Color Palette**:
  - Background: #1a1b26 (Tokyo Night dark)
  - Foreground: #a9b1d6
  - Primary: #7aa2f7 (blue)
  - Secondary: #bb9af7 (purple)
  - Accent: #9ece6a (green)
  - Warning: #e0af68 (yellow)
  - Error: #f7768e (red)
  - Surface: #24283b
  - Border: #414868
- **Typography**:
  - Font: Monospace (system default)
  - Title: 18px bold
  - Headers: 14px bold
  - Body: 12px regular
- **Spacing**: 8px base unit, 16px padding
- **Visual Effects**: Subtle borders, highlighted rows, selection glow

### Components
1. **Package Manager Selector** (Sidebar tabs)
   - States: default, hover, active/selected
   - Icon + label for each PM
2. **Package Table**
   - Columns: Name, Version, Status, Size, Description
   - Sortable columns
   - Row states: default, hover, selected
3. **Search Bar**
   - Fuzzy search across packages
   - Filter by status (installed/available/outdated)
4. **Action Panel**
   - Upgrade/Downgrade/Remove/Install buttons
   - Confirmation dialogs
5. **Package Details Panel**
   - Full metadata, dependencies, reverse deps

### Interactions
- **Keyboard Navigation**:
  - `Tab`/`Shift+Tab`: Navigate sections
  - `↑`/`↓`: Navigate list
  - `Enter`: Select/execute action
  - `/` or `Ctrl+F`: Focus search
  - `u`: Upgrade selected
  - `r`: Remove selected
  - `i`: Install/reinstall
  - `q` or `Esc`: Quit
  - `1-9`: Switch to PM by number
  - `Ctrl+R`: Refresh list
  - `Ctrl+O`: Show outdated
- **Mouse**: Click to select, double-click for details

## Functionality Specification

### Core Features
1. **Package Listing**
   - List all installed packages per PM
   - Show version, status, size
   - Fuzzy search
   - Filter: all/installed/available/outdated
2. **Package Operations**
   - Install package (search + install)
   - Upgrade single/all packages
   - Remove package
   - Show package info/details
3. **System Info**
   - Detect available PMs on system
   - Show current distro
   - Update cache functionality

### Data Handling
- Async execution of PM commands
- Caching with timestamp
- Parse output to structured data
- Error handling per PM

### Edge Cases
- PM not installed → show as disabled
- Permission denied → prompt for sudo
- Network errors → graceful degradation
- Empty lists → helpful messages

## Acceptance Criteria
1. App launches without errors
2. Can switch between all detected PMs
3. Package lists load and display correctly
4. Search filters packages in real-time
5. Keyboard navigation works throughout
6. Upgrade/remove operations execute
7. Visual design matches spec
8. Responsive to terminal resize