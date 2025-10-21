# Getting Started with ContentBox.js

ContentBox.js is a Javascript library for designing and building complete web pages with ease. It brings elegant, editorial-style content and magazine-style design to responsive websites with simple drag & drop.

Website: [https://innovastudio.com/contentbox](https://innovastudio.com/contentbox)

## Installation

### Via NPM

bash

```bash
npm install @innovastudio/contentbox @innovastudio/contentbox-runtime
```

### Via CDN

Include the files directly in your HTML:

html

```html
<script src="contentbox/contentbox.min.js"></script>
<link href="contentbox/contentbox.css" rel="stylesheet">
<link href="contentbuilder/contentbuilder.css" rel="stylesheet">
```

## Quick Start

### Basic Usage

**Using CDN (HTML/JavaScript):**

html

```html
<!-- Include stylesheets -->
<link href="contentbox/contentbox.css" rel="stylesheet">
<link href="contentbuilder/contentbuilder.css" rel="stylesheet">

<!-- Include scripts -->
<script src="contentbox/contentbox.min.js"></script>
<script>
// Initialize ContentBox
const builder = new ContentBox({
    controlPanel: true, 
    iframeSrc: 'blank.html'
});

// Load initial HTML content
const html = `
<div class="is-section is-box type-system-ui">
    <div class="is-container v2 leading-14 size-18">
        <div class="row">
            <div class="column">
                <p>Lorem Ipsum is simply dummy text...</p>
            </div>
        </div>
    </div>
</div>`;
const mainCss = '', sectionCss = '';
builder.loadHtml(html);
builder.loadStyles(mainCss, sectionCss);

// Get the HTML content
const html = builder.html();
// Get the styles (main styles and sections' styles)
const mainCss = builder.mainCss();
const sectionCss = builder.sectionCss();
</script>
```

**Using NPM (React/Vue/Modern Frameworks):**

javascript

```javascript
// Import ContentBox library
import ContentBox from '@innovastudio/contentbox'
import '@innovastudio/contentbox/public/contentbox/contentbox.css'
import '@innovastudio/contentbuilder/public/contentbuilder/contentbuilder.css'

// Initialize ContentBox
const builder = new ContentBox({
    controlPanel: true,
    iframeSrc: 'blank.html'
});

// Load initial HTML content
const html = `
<div class="is-section is-box type-system-ui">
    <div class="is-container v2 leading-14 size-18">
        <div class="row">
            <div class="column">
                <p>Lorem Ipsum is simply dummy text...</p>
            </div>
        </div>
    </div>
</div>`;
const mainCss = '', sectionCss = '';
builder.loadHtml(html);
builder.loadStyles(mainCss, sectionCss);

// Get the HTML content
const html = builder.html();
// Get the styles (main styles and sections' styles)
const mainCss = builder.mainCss();
const sectionCss = builder.sectionCss();
```

## Setting Up Assets

ContentBox requires certain assets (snippets, icons, etc.) to be accessible in your project. These assets are available in the ContentBox package (downloaded from the official website or from the examples provided in GitHub).

- Copy `/contentbox`, `/runtime`, `/assets` folders and `blank.html` to your public directory

**For advanced setup:** These assets can also be hosted on a CDN or external storage by adjusting the paths in the configuration (see full documentation for details).

## 📢 Upgrade Note for Existing Users

**Important:** If you're upgrading from a previous version, your existing implementation will continue to work without any changes.

The installation now includes two parts: the **ContentBox library** (for editing/building content) and the **Runtime library** (for displaying/rendering the content you create). The new runtime library is **optional but recommended** for better performance and enhanced rendering capabilities. You can upgrade at your own pace.

## Examples

### Example 1: HTML - Editing Mode

```html
<!DOCTYPE HTML>
<html>
<head>
    <meta charset="utf-8">
    <title>ContentBox Example</title>
    <meta name="viewport" content="width=device-width, initial-scale=1">

    <link href="contentbox/contentbox.css" rel="stylesheet">
    <link href="contentbuilder/contentbuilder.css" rel="stylesheet">
</head>
<body>

<script src="contentbox/contentbox.min.js"></script>
<script>
const builder = new ContentBox({
    controlPanel: true,
    iframeSrc: 'blank.html',

    autoSave: () => {
        save();
    },
});

// Load content from localStorage
const html = localStorage.getItem('content') || '';
builder.loadHtml(html);

// Load styles
const mainCss = localStorage.getItem('mainCss') || '';
const sectionCss = localStorage.getItem('sectionCss') || '';
builder.loadStyles(mainCss, sectionCss);

// Save function
function save() {
    const html = builder.html();
    const mainCss = builder.mainCss();
    const sectionCss = builder.sectionCss();
    
    localStorage.setItem('content', html);
    localStorage.setItem('mainCss', mainCss);
    localStorage.setItem('sectionCss', sectionCss);
    
    console.log('Content saved!');
}
</script>

</body>
</html>
```

### Example 2: HTML - Viewing Mode

```html
<!DOCTYPE html>
<head>
    <meta charset="utf-8">
    <title>View Content</title>
    <meta name="viewport" content="width=device-width, initial-scale=1">
    
    <link href="runtime/contentbox-runtime.css" rel="stylesheet">
    
    <!-- Your saved global styles and section-specific styles here 
        (mainCss & sectionCss) -->
</head>
<body style="touch-action: pan-y">

    <!-- Important: Use 'is-wrapper' class when viewing -->
    <div class="is-wrapper">
        <!-- Your saved content here -->
    </div>
  
    <script src="runtime/contentbox-runtime.min.js"></script>
    <script>
        const runtime = new ContentBoxRuntime();
        runtime.init();
    </script>
</body>
</html>
```

**Important:** When using Runtime for viewing, always add the `is-wrapper` class to your content wrapper.

### Example 3: React - Editing Mode

tsx

```tsx
import { useEffect, useRef } from 'react'
import ContentBox from '@innovastudio/contentbox'
import '@innovastudio/contentbox/public/contentbox/contentbox.css'
import '@innovastudio/contentbuilder/public/contentbuilder/contentbuilder.css'

function App() {
    // Ref to hold ContentBox instance
    const builderRef = useRef<ContentBox | null>(null);

    useEffect(() => {

        builderRef.current = new ContentBox({

            controlPanel: true,
            iframeSrc: `/blank.html`,

            autoSave: () => {
                save();
            }
        });

        // Load initial content
        const savedHtml = localStorage.getItem('savedHtml') || '';
        builderRef.current.loadHtml(savedHtml);

        // Cleanup on unmount
        return () => {
            builderRef.current?.destroy()
            builderRef.current = null
        };
    }, []);

    const save = async () => {
        if(!builderRef.current) return;
        const builder = builderRef.current;

        const html = builder.html();
        const mainCss = builder.mainCss();          // Global styles used on the page
        const sectionCss = builder.sectionCss();    // Section-specific styles

        // Save to localStorage as an example
        localStorage.setItem('savedHtml', html)
        localStorage.setItem('savedMainCss', mainCss)
        localStorage.setItem('savedSectionCss', sectionCss)

        console.log('Content saved!');
    }

    return (
        <>
            {/* ContentBox will be rendered here by the library */}
        </>
    );
}

export default App
```

### Example 4: React - Viewing Mode

tsx

```tsx
// src/ViewPage.tsx - For viewing saved content
import { useEffect, useState, useRef } from 'react'
import ContentBoxRuntime from '@innovastudio/contentbox-runtime'
import '@innovastudio/contentbox-runtime/dist/contentbox-runtime.css'

export default function ViewPage() {
    const [html, setHtml] = useState<string>('')
    const [mainCss, setMainCss] = useState<string>('')
    const [sectionCss, setSectionCss] = useState<string>('')
    const runtimeRef = useRef<ContentBoxRuntime | null>(null)

    // Load data from localStorage
    useEffect(() => {
        const html = localStorage.getItem('savedHtml') || ''
        const mainCss = localStorage.getItem('savedMainCss') || ''
        const sectionCss = localStorage.getItem('savedSectionCss') || ''
        
        setHtml(html)
        setMainCss(mainCss)
        setSectionCss(sectionCss)
    }, []);

    // Initialize runtime after content is rendered
    useEffect(() => {
        if (html && !runtimeRef.current) {
            runtimeRef.current = new ContentBoxRuntime()
            runtimeRef.current.init()
        }

        return () => {
            if (runtimeRef.current) {
                runtimeRef.current.destroy()
                runtimeRef.current = null
            }
        }
    }, [html])

    return (
        <>
            <style>{mainCss}</style>
            <style>{sectionCss}</style>
            <div className="is-wrapper" dangerouslySetInnerHTML={{ __html: html }} />
        </>
    );
}
```

------

## Custom Toolbar

While ContentBox works without a custom toolbar, **implementing one is highly recommended** for production use. A custom toolbar allows you to add essential buttons like Save, Undo/Redo, device preview controls, and integrate with your own backend/navigation.

### Why Use a Custom Toolbar?

- **Save Button**: Integrate with your backend/storage
- **Undo/Redo**: Quick access to history controls
- **Device Preview**: Toggle between desktop, tablet, and mobile views
- **Custom Actions**: Add your own buttons (Back, Preview, Download, etc.)
- **Better UX**: Keep important controls always visible

### Setting Up

Enable custom toolbar by adding `topSpace: true` to the configuration:

javascript

```javascript
const builder = new ContentBox({
    controlPanel: true,
    iframeSrc: 'blank.html',
    topSpace: true  // Adds space at the top for your custom toolbar
});
```

### HTML Implementation

Here's a simplified custom toolbar example:

html

```html
<!DOCTYPE HTML>
<html>
<head>
    <meta charset="utf-8">
    <title>ContentBox with Custom Toolbar</title>
    <meta name="viewport" content="width=device-width, initial-scale=1">

    <link href="contentbox/contentbox.css" rel="stylesheet">
    <link href="contentbuilder/contentbuilder.css" rel="stylesheet">
    
    <style>
        /* Simple toolbar styling */
        .custom-topbar {
            display: flex;
            justify-content: space-between;
            align-items: center;
            padding: 10px 20px;
            background: #2c3e50;
            color: white;
            gap: 10px;
        }
        .custom-topbar button {
            padding: 8px 16px;
            border: none;
            background: #34495e;
            color: white;
            border-radius: 4px;
            cursor: pointer;
        }
        .custom-topbar button:hover {
            background: #4a5f7f;
        }
        .custom-topbar .btn-group {
            display: flex;
            gap: 8px;
        }
    </style>
</head>
<body>

<!-- Custom Toolbar -->
<div class="custom-topbar">
    <div>ContentBox Editor</div>
    
    <div class="btn-group">
        <button onclick="builder.undo()">Undo</button>
        <button onclick="builder.redo()">Redo</button>
        <button onclick="save()">Save</button>
        <button onclick="builder.viewHtml()">View HTML</button>
        <button onclick="builder.toggleEditPanel()">Toggle Panel</button>
    </div>
    
    <div class="btn-group">
        <button onclick="builder.setScreenMode('desktop')">Desktop</button>
        <button onclick="builder.setScreenMode('tablet')">Tablet</button>
        <button onclick="builder.setScreenMode('mobile')">Mobile</button>
    </div>
</div>

<script src="contentbox/contentbox.min.js"></script>
<script>
const builder = new ContentBox({
    controlPanel: true,
    iframeSrc: 'blank.html',
    topSpace: true  // Enable custom toolbar area
});

// Load content
const html = localStorage.getItem('content') || '';
builder.loadHtml(html);
const mainCss = localStorage.getItem('mainCss') || '';
const sectionCss = localStorage.getItem('sectionCss') || '';
builder.loadStyles(mainCss, sectionCss);

// Save function
function save() {
    const html = builder.html();
    const mainCss = builder.mainCss();
    const sectionCss = builder.sectionCss();
    
    localStorage.setItem('content', html);
    localStorage.setItem('mainCss', mainCss);
    localStorage.setItem('sectionCss', sectionCss);
    
    alert('Content saved!');
}
</script>

</body>
</html>
```

### React Implementation

For React, create a separate toolbar component:

tsx

```tsx
// src/App.tsx
import { useEffect, useRef } from 'react'
import ContentBox from '@innovastudio/contentbox'
import '@innovastudio/contentbox/public/contentbox/contentbox.css'
import '@innovastudio/contentbuilder/public/contentbuilder/contentbuilder.css'
import Topbar from './components/Topbar'  // Custom toolbar component

function App() {
    const builderRef = useRef<ContentBox | null>(null);

    useEffect(() => {
        builderRef.current = new ContentBox({
            controlPanel: true,
            iframeSrc: `/blank.html`,
            topSpace: true  // Enable custom toolbar area
        });

        const savedHtml = localStorage.getItem('savedHtml') || '<div>...</div>';
        builderRef.current.loadHtml(savedHtml);

        return () => {
            builderRef.current?.destroy()
            builderRef.current = null
        };
    }, []);

    const handleSave = async () => {
        if(!builderRef.current) return;
        
        const html = builderRef.current.html();
        const mainCss = builderRef.current.mainCss();
        const sectionCss = builderRef.current.sectionCss();

        localStorage.setItem('savedHtml', html)
        localStorage.setItem('savedMainCss', mainCss)
        localStorage.setItem('savedSectionCss', sectionCss)
        
        alert('Content saved!')
    };

    const handleUndo = () => builderRef.current?.undo();
    const handleRedo = () => builderRef.current?.redo();
    const handleViewHtml = () => builderRef.current?.viewHtml();
    const handleTogglePanel = () => builderRef.current?.toggleEditPanel();
    const handleSetScreenMode = (mode: string) => builderRef.current?.setScreenMode(mode);

    return (
        <>
            <Topbar
                onSave={handleSave}
                onUndo={handleUndo}
                onRedo={handleRedo}
                onViewHtml={handleViewHtml}
                onTogglePanel={handleTogglePanel}
                onSetScreenMode={handleSetScreenMode}
            />
            {/* ContentBox will be rendered here */}
        </>
    );
}

export default App
```

> **💡 Complete Implementation:** The Topbar component with full functionality, icons, and styling is available in `examples/react/src/components/Topbar.tsx` in the package.

### Available Toolbar Methods

Common methods you'll use in your custom toolbar:

- `builder.undo()` - Undo last action
- `builder.redo()` - Redo last undone action
- `builder.viewHtml()` - View HTML source
- `builder.toggleEditPanel()` - Show/hide the edit panel
- `builder.setScreenMode(mode)` - Set device preview mode (`'desktop'`, `'tablet'`, `'mobile'`, `'fullview'`)
- `builder.download()` - Download content as HTML file
- `builder.clear()` - Clear all content

------

## Previous Version: Without Runtime Library

You can still use the previous version stylesheet and script for viewing basic content:

html

```html
<link href="box/box-flex.css" rel="stylesheet">
<script src="box/box-flex.js"></script>
```

------

## Media Upload Handling

ContentBox allows users to embed images, videos, audio files, or add links to uploaded documents directly within the editor. You can enable this feature by providing your own upload logic through the `upload` parameter.

### Enabling Media Upload

To enable media uploads, pass an `upload` function when initializing ContentBox:

```javascript
const builder = new ContentBox({
  
		// options...
  
    upload: async (file) => {
        // Your custom file upload logic
        const formData = new FormData();
        formData.append('file', file);

        const response = await fetch('/upload', {
            method: 'POST',
            body: formData,
        });

        const data = await response.json();
        return data; // must return a JSON object containing at least { url }
    },
});
```

The `upload` function receives the selected file as a parameter. Your function should:

1. Upload the file to your server or storage service
2. Return a JSON object containing at least `{ url: 'uploaded-file-url' }`

ContentBox will automatically use the returned URL to embed the media in the editor.

**Note:** Your upload function can save files to your local server, AWS S3, or any other storage service of your choice. The implementation is entirely up to you.

### 📢 Upgrade Note

**For existing users:** If you're currently using `onImageUpload`, `onVideoUpload`, `onAudioUpload`, or `onFileUpload` parameters, they will continue to work without any changes. The new `upload` parameter is a simplified alternative that handles all media types with a single function. You can upgrade to the new approach at your own pace.

## Integrating a File Picker / Asset Manager

In addition to uploading new files, ContentBuilder allows users to select existing files from your server or storage. This is done by integrating a file picker or asset manager application that opens in a modal dialog.

### Enabling File Picker

```javascript
const builder = new ContentBuilder({
    container: '.container',

    // Opens the file picker for inserting images, videos, audio, or document links
    filePicker: 'assets.html',
    // You can replace this with your own file manager application,
    // or use the Files.js Asset Manager: https://innovastudio.com/asset-manager

    filePickerSize: 'large', // medium (default), large, fullscreen
});
```

The file picker opens in a modal (loaded as an iframe). This is a separate application where you can build your own custom file browser or use an existing asset manager. Users can browse and select files from your server or storage service.

### How It Works

When a user selects a file in the file picker, the picker application must return the selected file URL back to ContentBox by calling:

```javascript
builder.selectAsset(url); // url is the selected file URL
```

Where `url` is the path to the selected file (e.g., `'uploads/image.png'` or `'https://yourdomain.com/media/video.mp4'`).

**Example File Picker:**

A simple example (`assets.html`) is included in the ContentBox package. This demonstrates how file selection works and shows the proper use of the `selectAsset()` method. Note that this is just a basic demonstration with selectable images, not a full file browsing system.

**Recommended Asset Manager:**

For a production-ready file management solution, you can use [Files.js Asset Manager](https://innovastudio.com/asset-manager), which integrates seamlessly with ContentBox.

**Custom Implementation:**

You can build your own file picker application. The only requirement is that it must call `builder.selectAsset(url)` with the selected file URL when a file is chosen.

### 📢 Upgrade Note

**For existing users:** If you're currently using `imageSelect`, `videoSelect`, `audioSelect`, `fileSelect`, or `mediaSelect` parameters, they will continue to work without any changes. The new `filePicker` parameter is a simplified alternative that handles all media types with a single configuration. You can upgrade to the new approach at your own pace.

## AI Assistant Integration

ContentBox includes an **AI Assistant panel** that allows users to generate and edit content using AI. This feature requires a server-side endpoint to communicate with AI providers like OpenAI or OpenRouter.

**Important:** ContentBox.js is a client-side library and works with any server platform. The AI Assistant feature simply requires you to create an API endpoint on your server. Examples are provided in **Node.js, Next.js, and PHP** to cover common scenarios, but you can implement the endpoint in any server-side language or framework.

### Requirements

You'll need an API key from one of these services:

- **OpenAI** – https://openai.com
- **OpenRouter** – https://openrouter.ai (provides access to multiple AI models)

### Configuration

**Store your API key:**

**Node.js/Next.js** (`.env` file):

```
OPENAI_API_KEY=your_openai_api_key
```

**PHP** (`config.php` file):

```php
$OPENAI_API_KEY = 'your_openai_api_key';
```

**Configure ContentBuilder:**

**Node.js:**

```javascript
const builder = new ContentBuilder({
    container: '.container',
    sendCommandUrl: '/sendcommand' // Your server endpoint for handling AI requests
});
```

**Next.js:**

```javascript
const builder = new ContentBuilder({
    container: '.container',
    sendCommandUrl: '/api/sendcommand' // Next.js API route
});
```

**PHP:**

```javascript
const builder = new ContentBuilder({
    container: '.container',
    sendCommandUrl: 'api/sendcommand.php' // PHP endpoint
});
```

### Server-Side Implementation

Your endpoint receives AI requests from ContentBuilder and forwards them to the AI provider. Server-side endpoint examples are provided in **Node.js, Next.js, and PHP** within the ContentBuilder package and on GitHub.

The examples included in the ContentBuilder package have this pre-configured, so you can test the AI Assistant immediately after adding your API key.

## AI Image Generation

ContentBuilder supports AI-powered image generation directly from the AI Assistant panel. Users can type a text prompt to generate images that are automatically embedded into their content.

### Requirements

To enable AI image generation, you need an API key from:

**Fal.ai** – https://fal.ai

### Configuration

**Store your API key:**

**Node.js/Next.js** (`.env` file):

```
FAL_API_KEY=your_fal_api_key
```

**PHP** (`config.php` file):

```php
$FAL_API_KEY = 'your_fal_api_key';
```

**Configure ContentBuilder:**

**Node.js:**

```javascript
const builder = new ContentBuilder({
    container: '.container',
    
    defaultImageGenerationProvider: 'fal',
    generateMediaUrl_Fal: '/request-fal',
    checkRequestStatusUrl_Fal: '/status-fal',
    getResultUrl_Fal: '/result-fal',
});
```

**Next.js:**

```javascript
const builder = new ContentBuilder({
    container: '.container',
    
    defaultImageGenerationProvider: 'fal',
    generateMediaUrl_Fal: '/api/request-fal',
    checkRequestStatusUrl_Fal: '/api/status-fal',
    getResultUrl_Fal: '/api/result-fal',
});
```

**PHP:**

```javascript
const builder = new ContentBuilder({
    container: '.container',
    
    defaultImageGenerationProvider: 'fal',
    generateMediaUrl_Fal: 'api/request-fal.php',
    checkRequestStatusUrl_Fal: 'api/status-fal.php',
    getResultUrl_Fal: 'api/result-fal.php',
});
```

### Choosing Image Models

Fal.ai provides access to multiple AI image models, from Google's latest models to FLUX variants. Users can select their preferred model and image size/orientation from the AI Assistant settings panel.

### Server-Side Implementation

Server-side endpoint examples for handling image generation requests are provided in **Node.js, Next.js, and PHP** within the ContentBuilder package and on GitHub.

### 📢 Upgrade Note

**For existing users:** If you're currently using Getimg.ai for image generation (`textToImageUrl`, `upscaleImageUrl` parameters), your implementation will continue to work without any changes. The Fal.ai integration is the current recommended approach for accessing a wider range of AI models, but you can continue using your existing setup or upgrade at your own pace.

## Resources

- **[Plugin Development Guide](https://github.com/innovastudio-dev/contentbuilder/blob/main/plugin-development.md)** - Learn how to create custom plugins for ContentBox
- **[Configuration Reference](reference.md)**
- **Product Website** - https://innovastudio.com/contentbox
- **GitHub Repository** - https://github.com/innovastudio-dev/contentbox


