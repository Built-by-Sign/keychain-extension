# EthSign Keychain Extension

A secure Chrome extension that stores and protects login credentials using your MetaMask wallet. EthSign Keychain leverages MetaMask Snaps to provide decentralized, encrypted password management directly in your browser.

## Overview

EthSign Keychain is a browser extension that integrates with MetaMask to provide secure credential storage and autofill functionality. Instead of relying on centralized password managers, your credentials are encrypted and stored using your MetaMask wallet, giving you full control over your data.

### Key Features

- **🔐 Secure Storage**: Credentials are encrypted using your MetaMask wallet
- **🔄 Auto-fill**: Automatically detects and fills login forms
- **💾 Import/Export**: Backup and restore your credentials
- **🌐 Multi-Site Support**: Manage credentials for multiple websites
- **🚫 Never Save Option**: Exclude specific sites from password saving
- **📝 Manual Entry Management**: Add, edit, and delete credentials manually
- **🔗 Blockchain Integration**: Leverages MetaMask Snaps for secure key management
- **☁️ Sync Options**: Support for syncing credentials (AWS/Arweave)

## Architecture

### Components

The extension consists of several key components:

- **Popup Interface** (`src/App.tsx`): Main React application for managing credentials
- **Content Script** (`src/chromeServices/DOMContent.ts`): Detects login forms and handles autofill
- **Background Service** (`src/chromeServices/DOMBackground.ts`): Manages extension lifecycle and messaging
- **MetaMask Snap Integration** (`src/utils/snap.ts`): Communicates with the keychain-snap for secure storage

### Key Pages

1. **Connect** - Initial MetaMask connection and snap installation
2. **Credentials** - View and manage saved credentials for the current site
3. **Pending** - Review credentials detected from form submissions before saving
4. **Edit Entry** - Add or modify credential entries
5. **Never Save** - Manage sites excluded from password saving

## Prerequisites

- Node.js (v16 or higher)
- npm or yarn
- MetaMask browser extension installed
- Chrome or Chromium-based browser

## Installation

### Development Setup

1. Clone the repository:

```bash
git clone <repository-url>
cd keychain-extension
```

2. Install dependencies:

```bash
npm install
```

3. Build the extension:

```bash
npm run build
```

4. Load the extension in Chrome:
   - Navigate to `chrome://extensions/`
   - Enable "Developer mode" (toggle in top-right)
   - Click "Load unpacked"
   - Select the `build` folder from the project directory

### Production Build

For production deployment:

```bash
npm run build
```

The build process uses CRACO to customize the Create React App configuration and ensures proper Chrome extension compatibility.

## Available Scripts

### `npm start`

Runs the app in development mode at [http://localhost:3000](http://localhost:3000). Note: For full functionality, you need to build and load as a Chrome extension.

### `npm run build`

Builds the extension for production to the `build` folder. The build is optimized and minified with:

- INLINE_RUNTIME_CHUNK disabled for Chrome extension compatibility
- Static assets properly configured for manifest v3
- Content scripts and background service worker compiled

### `npm test`

Launches the test runner in interactive watch mode.

## Project Structure

```
keychain-extension/
├── public/
│   ├── manifest.json          # Chrome extension manifest v3
│   ├── notifications.js       # Notification handling
│   └── index.html            # Popup HTML template
├── src/
│   ├── chromeServices/       # Chrome extension integration
│   │   ├── DOMContent.ts     # Content script for form detection
│   │   └── DOMBackground.ts  # Background service worker
│   ├── components/           # React UI components
│   │   ├── CredentialRow.tsx
│   │   ├── DisplayCredentials.tsx
│   │   ├── FileDropzone.tsx
│   │   └── icons/            # SVG icon components
│   ├── config/               # Configuration files
│   │   ├── index.ts
│   │   └── snap.ts           # MetaMask Snap ID and version
│   ├── hooks/                # React hooks
│   │   └── MetaMaskContext.tsx  # MetaMask state management
│   ├── pages/                # Main page components
│   │   ├── Connect.tsx       # MetaMask connection page
│   │   ├── Credentials.tsx   # Credential management page
│   │   ├── EditEntry.tsx     # Add/edit credential page
│   │   ├── NeverSave.tsx     # Never save sites management
│   │   └── Pending.tsx       # Pending credentials review
│   ├── types/                # TypeScript type definitions
│   │   ├── index.ts
│   │   ├── snap.ts
│   │   └── DOMMessages.ts    # Chrome messaging types
│   ├── ui/                   # Reusable UI components
│   │   └── forms/
│   ├── utils/                # Utility functions
│   │   ├── autofill.ts       # Form detection and autofill logic
│   │   ├── files.ts          # Import/export functionality
│   │   ├── forms.ts          # Form handling utilities
│   │   └── snap.ts           # MetaMask Snap API wrapper
│   └── App.tsx               # Main application component
├── craco.config.js           # CRACO configuration
├── tailwind.config.js        # Tailwind CSS configuration
└── tsconfig.json             # TypeScript configuration
```

## How It Works

1. **Installation**: User installs the extension and connects their MetaMask wallet
2. **Snap Installation**: The extension installs the `keychain-snap` into MetaMask
3. **Form Detection**: Content script monitors web pages for login forms
4. **Credential Capture**: When a form is submitted, credentials are captured and stored in "pending"
5. **User Approval**: User reviews and saves pending credentials through the popup
6. **Auto-fill**: When revisiting a site, the extension detects login forms and offers to auto-fill
7. **Encryption**: All credentials are encrypted using keys derived from your MetaMask wallet

## Configuration

### Snap Configuration

The extension connects to the MetaMask Snap defined in `src/config/snap.ts`:

```typescript
export const SNAP_ID = "npm:keychain-snap";
export const SNAP_VERSION = "0.3.6";
```

For local development with a custom snap:

```typescript
export const SNAP_ID = "local:http://localhost:8081";
```

## Security

- **No Centralized Storage**: Credentials never leave your control
- **Wallet-Based Encryption**: Uses MetaMask's secure key management
- **Local Processing**: All encryption/decryption happens locally
- **Open Source**: Code is transparent and auditable
- **Manifest V3**: Uses the latest Chrome extension security standards

## Dependencies

### Core Dependencies

- **React 18**: UI framework
- **TypeScript**: Type-safe development
- **@metamask/providers**: MetaMask integration
- **Tailwind CSS**: Styling framework
- **react-dropzone**: File import functionality
- **lodash**: Utility functions

### Chrome Extension APIs Used

- `chrome.tabs`: Tab management and messaging
- `chrome.storage`: Local data persistence
- `chrome.runtime`: Background service worker communication

## Troubleshooting

### Extension not working after installation

- Ensure MetaMask is installed and unlocked
- Check that the snap is installed (visit the Connect page in the extension)
- Reload the extension from `chrome://extensions/`

### Credentials not auto-filling

- Check that the site is not in the "Never Save" list
- Ensure the form fields are detected (check console for errors)
- Verify MetaMask is connected and the snap is installed

### Build errors

- Clear node_modules and reinstall: `rm -rf node_modules && npm install`
- Ensure you're using Node.js v16 or higher
- Check that all peer dependencies are compatible

## Contributing

Contributions are welcome! Please ensure:

- Code follows TypeScript best practices
- Components are properly typed
- Chrome extension APIs are used correctly
- Changes are tested in both development and production builds

## License

This project is licensed under the GNU Affero General Public License v3.0 (AGPLv3). See the [LICENSE](LICENSE) file for details.

## Related Projects

- **keychain-snap**: The MetaMask Snap that handles secure credential storage
- **MetaMask Snaps**: https://metamask.io/snaps/

## Support

For issues, questions, or contributions, please [open an issue](link-to-issues) on GitHub.
