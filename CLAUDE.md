# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This is an Electron-based desktop application for inspecting Android accessibility trees. The app connects to Android devices via ADB to capture accessibility node hierarchies and screenshots, providing an interface for developers to understand and test accessibility implementations.

## Development Commands

### Core Development
- `npm start` - Start development server (runs prestart, then start:renderer)
- `npm run start:main` - Start main process with hot reload and Tailwind watch
- `npm run start:renderer` - Start renderer process development server
- `npm run prestart` - Build main process for development

### Building
- `npm run build` - Build both main and renderer processes for production
- `npm run build:main` - Build main process only
- `npm run build:renderer` - Build renderer process only
- `npm run build:dll` - Build development DLL

### Code Quality
- `npm run lint` - Run ESLint
- `npm run lint:fix` - Run ESLint with auto-fix
- `npm test` - Run Jest tests

### Packaging
- `npm run package` - Build and package app for distribution
- `npm run publish` - Build and publish to GitHub releases

### CSS/Styling
- `npm run tailwind` - Watch and compile Tailwind CSS

## Architecture

### Main Process (`src/main/`)
- **main.ts**: Entry point, handles ADB client initialization, IPC communication, and window management
- **menu.ts**: Application menu configuration
- **preload.ts**: Preload script for secure renderer communication
- **util.ts**: Utility functions for main process

### Renderer Process (`src/renderer/`)
- **App.tsx**: Root React component with routing
- **views/**: Main UI components
  - **main-view.tsx**: Primary application interface with device selection, tree view, and details
  - **connect-device.tsx**: Device connection and pairing interface
  - **basic-tree-view.tsx**: Accessibility tree visualization
  - **view-details.tsx**: Selected node details panel
  - **screenshot.tsx**: Device screenshot with overlay highlighting
  - **search-view.tsx**: Search functionality for accessibility nodes
  - **log-view.tsx**: Message logging interface
- **models/AndroidView.ts**: TypeScript interfaces for accessibility node data
- **utils/adb.ts**: Renderer-side ADB API wrapper for IPC communication

### Key Technologies
- **Electron**: Desktop app framework
- **React**: UI library with TypeScript
- **adb-ts**: Android Debug Bridge integration
- **react-accessible-treeview**: Accessibility tree visualization
- **TailwindCSS + DaisyUI**: Styling framework
- **Webpack**: Module bundling with ERB configuration

### ADB Integration
The app manages ADB connections through:
- Device discovery and listing
- Port forwarding (tcp:38301 for accessibility service)
- Screenshot capture
- App installation and service management
- WiFi pairing via mDNS discovery

### IPC Communication
Main process exposes these handlers:
- `adb-list-devices`: Get connected devices
- `adb-screencap`: Capture device screenshot
- `adb-forward`: Set up port forwarding
- `adb-start-service`: Start accessibility service
- `adb-app-installed`: Check if companion app is installed
- `wifi-connect-start/stop`: WiFi pairing management

## Testing

- Uses Jest with `@testing-library/react`
- Test files in `src/__tests__/`
- Run with `npm test`

## Key Dependencies

- **adb-ts**: Android Debug Bridge client
- **react-accessible-treeview**: Tree component for accessibility hierarchy
- **electron-builder**: Application packaging
- **webpack**: Module bundling with custom ERB configuration
- **tailwindcss**: Utility-first CSS framework

## Asset Management

- ADB binaries located in `assets/adb/`
- Icons and images in `assets/icons/`
- Companion APK: `assets/adb/app-debug.apk`

## Build Configuration

Uses Electron React Boilerplate (ERB) with custom webpack configurations in `.erb/configs/`. The build process handles:
- TypeScript compilation
- CSS processing with Tailwind
- Asset bundling
- Cross-platform packaging