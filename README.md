# ContentBox.js Documentation

**ContentBox.js is a web page builder Javascript library for creating elegant, magazine-style designs with expressive typography and visual storytelling - all fully responsive. Ideal for building custom CMS or online site builders that go beyond basic web layouts.**

Website: [https://innovastudio.com/contentbox](https://innovastudio.com/contentbox)

Other Resources:

[Using ContentBox.js in a React Project](https://innovastudio.com/using-contentbox-in-a-react-project)

[Using ContentBox.js in a Next.js 14 Project](https://innovastudio.com/using-contentbox-in-a-nextjs-project)

[Using ContentBox.js in a Laravel Project](https://innovastudio.com/using-contentbox-in-a-laravel-project)

[More](https://innovastudio.com/resources)

## **🚀 Getting Started**

### **Step 1: Installation**

### **Using NPM:**

```jsx
npm install @innovastudio/contentbox
```

Then import into your project:

```jsx
import ContentBox from '@innovastudio/contentbox';
import '@innovastudio/contentbox/public/contentbox/contentbox.css';
import '@innovastudio/contentbuilder/public/contentbuilder/contentbuilder.css';
```

### **Or include in your HTML:**

```jsx
<!-- CSS -->
<link rel="stylesheet" href="contentbox/contentbox.css" />
<link rel="stylesheet" href="contentbuilder/contentbuilder.css" />
<!-- JS -->
<script src="contentbox/contentbox.min.js"></script>
```

### **Step 2: Assets**

ContentBox requires various assets such as **snippets** , icons, images, and stylesheets. These are located inside the package’s public folder:

- **`public/assets/`**
- **`public/box/`**
- **`public/contentbox/`**
- **`public/uploads/`**
- **`public/blank.html`**
- **`public/assets.html`**
- **`public/preview.html`**

**Action Required:**

Copy the folders/files into the root of your public project directory, or host them via a CDN.

## **🛠 Usage**

### **JavaScript Initialization**

Initialize the ContentBox instance:

```jsx
const builder = new ContentBox({
    controlPanel: true,
    iframeSrc: 'blank.html'
    /* other options */
});
```

- **`controlPanel` (default: `false`)**
    
    Setting **`controlPanel`** to **`true`** enables the enhanced side panel for editing. We recommend enabling this feature.
    
- **`iframeSrc` (default: `'blank.html'`)**
    
    By specifying the **`iframeSrc`** parameter, ContentBox.js will render content inside a resizable iframe, allowing you to preview the layout at different screen sizes. We recommend setting this option.
    
    The **`blank.html`** file is included in the **`public/`** folder. If you're using a specific framework such as Bootstrap, you can add the corresponding CSS to this file to ensure consistent styling during editing.
    

## **💡 API Methods**

### **Get Generated HTML**

Retrieve the current HTML content built inside the editor:

```jsx
const html = builder.html();
```

You can then send this HTML to a server for saving.

### Get the Styles

```jsx
const mainCss = builder.mainCss(); // Returns the default typography style for the page.
const sectionCss = builder.sectionCss(); // Returns the typography styles for specific sections on the page
```

In production, the saved HTML content should be rendered with the styles.

### **Load HTML into Builder**

Load existing HTML content into the builder for editing:

```jsx
builder.loadHtml(`<div class="is-section is-section-100 is-box type-system-ui">
    <div class="is-overlay"></div>
    <div class="is-container v2 size-18 leading-14 is-content-1300">
    <div class="row">
        <div class="column">
            <h1 class="leading-11 size-88">Hello World</h1>
        </div>
    </div>
    </div>
</div>`);
```

### **Load the Styles into Builder**

```jsx
builder.loadStyles(mainCss, sectionCss); 
```

## **🧪 Example Page**

Here’s a complete example showing how to integrate ContentBox.js into an HTML file:

```jsx
<!DOCTYPE HTML>
<html>
<head>
    <meta charset="utf-8">
    <title>Example</title>
    <meta name="viewport" content="width=device-width, initial-scale=1">
    <meta name="description" content="">
    <link rel="shortcut icon" href="#">  
    <link href="contentbuilder/contentbuilder.css" rel="stylesheet">
    <link href="contentbox/contentbox.css" rel="stylesheet">
</head>
<body>

<script src="contentbox/contentbox.min.js"></script>
<script>
    const builder = new ContentBox({
        controlPanel: true,
        iframeSrc: 'blank.html'
    });
</script>

</body>
</html>
```

## **📷 Media Upload Handling**

ContentBox.js provides built-in support for media uploads such as images, videos, audio files, and general documents. These are handled through customizable event hooks that allow you to integrate your own upload logic.

### **Enabling Image Uploads**

To enable image uploads in the editor, use the **`onImageUpload`** option when initializing the ContentBox instance:

```jsx
const builder = new ContentBox({
    /* options */
    onImageUpload: async (e) => {
        const data = await uploadFile(e);
        const uploadedFileUrl = data.url;
        builder.returnUrl(uploadedFileUrl);
    }
});
```

The **`onImageUpload`** function receives a file input event. You can process the selected file using your preferred upload method and return the resulting URL back to the editor using **`builder.returnUrl(url)`**.

### **Example Upload Helper Function**

This helper function uploads the selected file to the server and returns the JSON response:

```jsx
async function uploadFile(e) {
    const selectedFile = e.target.files[0];
    const formData = new FormData();
    formData.append('file', selectedFile);

    const response = await fetch('/upload', {
        method: 'POST',
        body: formData,
    });

    const data = await response.json();
    return data;
}
```

### **Other Media Upload Handlers**

In addition to image uploads, ContentBox supports several other media types via similar hooks:

- **`onUploadCoverImage`** – For section background images.
- **`onVideoUpload`** – For video files.
- **`onAudioUpload`** – For audio files.
- **`onMediaUpload`** – Used for galleries where both images and videos are allowed.
- **`onFileUpload`** – For general document uploads (PDFs, Word docs, etc.).

Each of these follows the same structure as **`onImageUpload`**. Here's how you can implement them:

```jsx
const builder = new ContentBox({
    /* options */
    
    onUploadCoverImage: async (e) => {
        const data = await uploadFile(e);
        builder.returnUrl(data.url);
    },
    
    onImageUpload: async (e) => {
        const data = await uploadFile(e);
        builder.returnUrl(data.url);
    },

    onVideoUpload: async (e) => {
        const data = await uploadFile(e);
        builder.returnUrl(data.url);
    },

    onAudioUpload: async (e) => {
        const data = await uploadFile(e);
        builder.returnUrl(data.url);
    },

    onMediaUpload: async (e) => {
        const data = await uploadFile(e);
        builder.returnUrl(data.url);
    },

    onFileUpload: async (e) => {
        const data = await uploadFile(e);
        builder.returnUrl(data.url);
    }
});
```

With these upload handlers in place, ContentBox becomes a fully functional editorial tool that allows content creators to embed rich media directly into their content.

## **📁 Integrating a Custom File/Asset Picker**

ContentBox.js allows you to integrate your own custom file picker or asset manager for media selection through the following configuration options:

- **`imageSelect`** – URL to your image picker.
- **`videoSelect`** – URL to your video picker.
- **`audioSelect`** – URL to your audio picker.
- **`fileSelect`** – URL to your general file picker.
- **`mediaSelect`** – URL to a combined media picker (for both images and videos).

These options are typically used to open an external asset/file picker in a dialog, where users can browse and select existing files from your server or CMS.

### **Basic Integration Example**

```jsx
let builder = new ContentBox({
    /* options */
    
    imageSelect: 'assets.html',
    videoSelect: 'assets.html',
    audioSelect: 'assets.html',
    fileSelect: 'assets.html',
    mediaSelect: 'assets.html',
});
```

In the above code, we're using **`assets.html`** as the file picker. This is a simple example provided in the package that lets users select media files. When a user selects a file, it returns the selected file's URL back to the editor using:

```jsx
parent.selectAsset(url);
```

Where **`url`** is the path to the selected file (e.g., **`'uploads/image.png'`**).

This minimal integration enables basic media insertion functionality. You can extend this file picker or replace it with your own custom implementation — just make sure to call **`parent.selectAsset(url)`** with the correct file URL when a file is selected.

## **🛠 Adding a Custom Topbar**

ContentBox allows you to define a custom top toolbar with your own branding and buttons.

The following shows a basic implementation of a custom topbar using simple HTML. Implementations for **React**  or **Vue** are also available as example projects.

### **💻 HTML Markup**

Add the following HTML structure inside the **`<body>`** of your document to create a custom topbar:

```jsx
<!-- Example of Custom Topbar -->
<div class="builder-ui keep-selection custom-topbar" data-tooltip>
    <div>
        Your Name
    </div>
    <div>
        <!-- Navigation & Action Buttons -->
        <button class="btn-back" title="Back">
            <svg><use xlink:href="#icon-back"></use></svg>
            <span>Back</span>
        </button>

        <div class="separator"></div>

        <button class="btn-undo" title="Undo">
            <svg><use xlink:href="#icon-undo"></use></svg>
        </button>

        <button class="btn-redo" title="Redo">
            <svg><use xlink:href="#icon-redo"></use></svg>
        </button>

        <button class="btn-save" title="Save">
            <svg><use xlink:href="#icon-save"></use></svg>
            <span>Save</span>
        </button>

        <button class="btn-publish" title="Publish">
            <svg><use xlink:href="#icon-publish"></use></svg>
            <span>Publish</span>
        </button>
    </div>
    <div>
        <!-- Device View Buttons -->
        <button class="btn-device-desktop-large" data-device="desktop-lg" title="Desktop - Large Screen">
            <svg style="width:18px;height:18px;"><use xlink:href="#icon-device-desktop"></use></svg>
        </button>
        <button class="btn-device-desktop" data-device="desktop" title="Desktop / Laptop">
            <svg style="width:20px;height:20px;"><use xlink:href="#icon-device-laptop"></use></svg>
        </button>
        <button class="btn-device-tablet-landscape" data-device="tablet-landscape" title="Tablet - Landscape">
            <svg style="width:18px;height:18px;transform:rotate(-90deg)"><use xlink:href="#icon-device-tablet"></use></svg>
        </button>
        <button class="btn-device-tablet" data-device="tablet" title="Tablet - Portrait">
            <svg  style="width:18px;height:18px;"><use xlink:href="#icon-device-tablet"></use></svg>
        </button>
        <button class="btn-device-mobile" data-device="mobile" title="Mobile">
            <svg  style="width:18px;height:18px;"><use xlink:href="#icon-device-mobile"></use></svg>
        </button>
        <button class="btn-fullview" data-device="fullview" title="Full View">
            <svg  style="width:18px;height:18px;"><use xlink:href="#icon-fullview"></use></svg>
        </button>

        <div class="separator"></div>

        <button class="btn-download" title="Download">
            <svg><use xlink:href="#icon-download"></use></svg>
        </button>

        <button class="btn-html" title="HTML">
            <svg><use xlink:href="#icon-code"></use></svg>
        </button>

        <button class="btn-preview" title="Preview">
            <svg><use xlink:href="#icon-eye"></use></svg>
        </button>

        <div class="separator"></div>

        <button class="btn-togglepanel" data-button="togglepanel" title="Toggle Edit Panel">
            <svg><use xlink:href="#icon-pencil"></use></svg>
        </button>
    </div>
</div>
```

### **⚙️ Configuration**

```jsx
const builder = new ContentBox({
    controlPanel: true,
    iframeSrc: 'blank.html',
    
    previewURL: 'preview.html', // Path to the preview page used to display the final content as it appears in production

    topSpace: true,           // Adds space at the top for your custom toolbar
    iframeCentered: true,     // Centers the page view area within the workspace

    // Disable built-in buttons that will be handled by the topbar:
    undoRedoButtons: false,   // Hide undo/redo from the side panel
    toggleDeviceButton: false, // Hide device toggle button
    deviceButtons: false      // Hide device buttons from top of page view area
});
```

The  **`previewURL`** option specifies the URL or path to a dedicated **preview page** , which is  included in the package under the **`public/`** folder.

The preview page (**`preview.html`**) provides a clean, production-like environment for viewing the content you're editing — without the builder UI or controls. It’s ideal for final visual checks and ensures that your layout and styling appear as expected in a real-world context.

### **🔧 Binding Button Actions**

Now connect your custom buttons to ContentBox methods:

```jsx
/* Custom Topbar Event Handlers */

// Back button (example only)
document.querySelector('.custom-topbar .btn-back')?.addEventListener('click', () => {
    alert('Back button clicked.');
});

// Undo
document.querySelector('.custom-topbar .btn-undo')?.addEventListener('click', () => {
    builder.undo();
});

// Redo
document.querySelector('.custom-topbar .btn-redo')?.addEventListener('click', () => {
    builder.redo();
});

// Save (example only)
document.querySelector('.custom-topbar .btn-save')?.addEventListener('click', () => {
    alert('Save button clicked.');
});

// Publish (example only)
document.querySelector('.custom-topbar .btn-publish')?.addEventListener('click', () => {
    alert('Publish button clicked.');
});

// Device View Buttons
const btnFullView = document.querySelector('.custom-topbar .btn-fullview');
const btnDeviceDesktopLarge = document.querySelector('.custom-topbar .btn-device-desktop-large');
const btnDeviceDesktop = document.querySelector('.custom-topbar .btn-device-desktop');
const btnDeviceTabletLandscape = document.querySelector('.custom-topbar .btn-device-tablet-landscape');
const btnDeviceTablet = document.querySelector('.custom-topbar .btn-device-tablet');
const btnDeviceMobile = document.querySelector('.custom-topbar .btn-device-mobile');

function clearActiveButtons() {
    const buttons = [
        btnFullView, btnDeviceDesktopLarge, btnDeviceDesktop,
        btnDeviceTabletLandscape, btnDeviceTablet, btnDeviceMobile
    ];
    buttons.forEach(btn => btn.classList.remove('on'));
}

// Set screen mode and highlight active button
btnFullView.addEventListener('click', () => {
    builder.setScreenMode('fullview');
    clearActiveButtons();
    btnFullView.classList.add('on');
});

btnDeviceDesktopLarge.addEventListener('click', () => {
    builder.setScreenMode('desktop-lg');
    clearActiveButtons();
    btnDeviceDesktopLarge.classList.add('on');
});

btnDeviceDesktop.addEventListener('click', () => {
    builder.setScreenMode('desktop');
    clearActiveButtons();
    btnDeviceDesktop.classList.add('on');
});

btnDeviceTabletLandscape.addEventListener('click', () => {
    builder.setScreenMode('tablet-landscape');
    clearActiveButtons();
    btnDeviceTabletLandscape.classList.add('on');
});

btnDeviceTablet.addEventListener('click', () => {
    builder.setScreenMode('tablet');
    clearActiveButtons();
    btnDeviceTablet.classList.add('on');
});

btnDeviceMobile.addEventListener('click', () => {
    builder.setScreenMode('mobile');
    clearActiveButtons();
    btnDeviceMobile.classList.add('on');
});

// Set initial button state based on current screen mode
switch(builder.screenMode) {
    case 'fullview':
        btnFullView.classList.add('on');
        break;
    case 'desktop-lg':
        btnDeviceDesktopLarge.classList.add('on');
        break;
    case 'desktop':
        btnDeviceDesktop.classList.add('on');
        break;
    case 'tablet-landscape':
        btnDeviceTabletLandscape.classList.add('on');
        break;
    case 'tablet':
        btnDeviceTablet.classList.add('on');
        break;
    case 'mobile':
        btnDeviceMobile.classList.add('on');
        break;
}

// Download
document.querySelector('.custom-topbar .btn-download')?.addEventListener('click', () => {
    builder.download();
});

// View HTML Source
document.querySelector('.custom-topbar .btn-html')?.addEventListener('click', () => {
    builder.viewHtml();
});

// Preview
document.querySelector('.custom-topbar .btn-preview')?.addEventListener('click', () => {
    const html = builder.html();
    const mainCss = builder.mainCss();
    const sectionCss = builder.sectionCss();

		// preview.html is a standalone page that displays content in a production-like view.
		// It requires current HTML and CSS to be saved in localStorage before opening.

    localStorage.setItem('preview-html', html);
    localStorage.setItem('preview-maincss', mainCss);
    localStorage.setItem('preview-sectioncss', sectionCss);

    window.open('/preview.html', '_blank')?.focus();
});

// Toggle Edit Panel
document.querySelector('.custom-topbar .btn-togglepanel')?.addEventListener('click', () => {
    builder.toggleEditPanel();
});
```

**Available ContentBox Methods (Used in Button Bindings)**

- **`builder.undo()`**
    
    Reverts the last editing action.
    
- **`builder.redo()`**
    
    Re-applies the last undone editing action.
    
- **`builder.setScreenMode(mode)`**
    
    Changes the page view area (iframe preview) to a specific device view mode (e.g., **`'desktop'`**, **`'mobile'`**, **`'fullview'`**).
    
- **`builder.download()`**
    
    Triggers downloading the current content as an HTML file.
    
- **`builder.viewHtml()`**
    
    Opens a modal displaying the raw HTML of the current page.
    
- **`builder.html()`**
    
    Returns the full HTML content of the editable area as a string.
    
- **`builder.mainCss()`**
    
    Returns the styles used in the current page.
    
- **`builder.sectionCss()`**
    
    Returns the styles specific to individual sections on the page.
    
- **`builder.toggleEditPanel()`**
    
    Shows or hides the side edit panel.
    

With this setup, you now have a fully functional **custom topbar** integrated with ContentBox.js. You can customize the branding, layout, and behavior as needed.

## **➕ Adding Custom Buttons to the Left Sidebar**

You can extend the default sidebar with your own buttons using the **`addButton()`** method.

Each button is defined by:

- **`pos`**: The position index where the button should be inserted
- **`title`**: Tooltip/title shown on hover
- **`html`**: The icon markup (typically an SVG)
- **`onClick`**: The function to execute when the button is clicked

### **Example: Adding Animation & Utility Buttons**

```jsx
builder.addButton({ 
    pos: 2,
    title: 'Animation',
    html: '<svg class="is-icon-flex" style="fill:rgba(0, 0, 0, 0.7);width:14px;height:14px;"><use xlink:href="#icon-wand"></use></svg>', 
    onClick: () => {
        builder.openAnimationPanel();
    }
});

builder.addButton({ 
    pos: 3,
    title: 'Timeline Editor',
    html: '<svg><use xlink:href="#icon-anim-timeline"></use></svg>', 
    onClick: () => {
        builder.openAnimationTimeline();
    }
});

builder.addButton({ 
    pos: 4,
    title: 'AI Assistant',
    html: '<svg class="is-icon-flex"><use xlink:href="#icon-message"></use></svg>', 
    onClick: () => {
        builder.openAIAssistant();
    }
});

builder.addButton({ 
    pos: 5,
    title: 'Clear Content',
    html: '<svg class="is-icon-flex"><use xlink:href="#icon-eraser"></use></svg>', 
    onClick: () => {
        builder.clear();
    }
});
```

**Available ContentBox Methods**

- **`builder.openAnimationPanel()`**
    
    Opens the basic animation settings panel for quick adjustments.
    
- **`builder.openAnimationTimeline()`**
    
    Opens the advanced timeline-based animation editor.
    
- **`builder.openAIAssistant()`**
    
    Launches the AI Assistant panel for content generation.
    
- **`builder.clear()`**
    
    Clears all content from the current page.
    

## **💾 Save Content to Server**

For saving content, you can retrieve the current content and then send the data to your server using `fetch()`.

### **Example:**

This function sends the content and styles to a server endpoint.

```jsx
async function save() {
    const html = builder.html(); // Get generated HTML
    const mainCss = builder.mainCss(); // Returns the default typography style for the page.
    const sectionCss = builder.sectionCss(); // Returns the typography styles for specific sections on the page

    const response = await fetch('/save', {
        method: 'POST',
        body: JSON.stringify({ html, mainCss, sectionCss }),
        headers: {
            'Content-Type': 'application/json'
        }
    });

    const result = await response.json();
    if (result.error) {
        console.error('Save error:', result.error);
    } else {
        console.log('Content saved successfully.');
    }
}
```

In production, the saved HTML content should be rendered with the styles.

### **🔁 Auto-Saving on Change**

You can use the **`onChange`** event in the configuration to implement auto-saving:

```jsx
let intervalId, previousHtml; // For auto-save tracking

const builder = new ContentBox({
    /* Other options */
    
    onChange: function () {
        clearInterval(intervalId);
        intervalId = setInterval(() => {
            const html = builder.htmlCheck(); // Lightweight check for changes
            if (previousHtml !== html) {
                save();
                console.log('Auto-saving...');
                previousHtml = html;
            }
        }, 2000); // Check every 2 seconds
    }
});
```

## **📥 Load Content from Server**

You can load saved content and styles into the editor as in the following example:

```jsx
async function load() {
    const response = await fetch('/load');
    const result = await response.json();

    const html = result.html;
    const mainCss = result.mainCss;
    const sectionCss = result.sectionCss;

    builder.loadHtml(html);           // Load the HTML content
    builder.loadStyles(mainCss, sectionCss); // Load the styles
}
```

Call this function after the ContentBox initialization.

## 📄 **Render Content in Production**

To display content created with ContentBox on a live (production) page, include the required CSS and JavaScript files.

These files are needed to render the content properly:

```jsx
<!-- Main -->
<link href="assets/minimalist-blocks/content.css" rel="stylesheet">
<link href="box/box-flex.css" rel="stylesheet">
<script src="box/box-flex.js"></script>

<!-- Slider -->
<link href="assets/scripts/glide/css/glide.core.css" rel="stylesheet">
<link href="assets/scripts/glide/css/glide.theme.css" rel="stylesheet">
<script src="assets/scripts/glide/glide.js"></script>

<!-- Form -->
<link href="assets/scripts/formviewer/formviewer.css" rel="stylesheet">
<script src="assets/scripts/formviewer/formviewer.min.js"></script>

<!-- Navbar -->
<link href="assets/scripts/navbar/navbar.css" rel="stylesheet">
<script src="assets/scripts/navbar/navbar.min.js"></script>
```

In your CMS, you can conditionally render includes based on whether certain elements are used. For example, include the slider script only if the content contains a slider.

### **💡 Example HTML Page**

```jsx
<!DOCTYPE HTML>
<html>
<head>
    <meta charset="utf-8">
    <title></title>
    <meta name="viewport" content="width=device-width, initial-scale=1">

    <!-- ContentBox styles -->
    <link href="assets/minimalist-blocks/content.css" rel="stylesheet">
    <link href="box/box-flex.css" rel="stylesheet">

    <!-- [render main and section styles here] -->
</head>
<body>

<div class="is-wrapper">
    <!-- [render content here] -->
</div>

<!-- Optional block assets (include only if used) -->

<!-- Form Viewer -->
<link href="assets/scripts/formviewer/formviewer.css" rel="stylesheet">
<script src="assets/scripts/formviewer/formviewer.min.js"></script>

<!-- Slider -->
<link href="assets/scripts/glide/css/glide.core.css" rel="stylesheet">
<link href="assets/scripts/glide/css/glide.theme.css" rel="stylesheet">
<script src="assets/scripts/glide/glide.js"></script>

<!-- Navbar -->
<link href="assets/scripts/navbar/navbar.css" rel="stylesheet">
<script src="assets/scripts/navbar/navbar.min.js"></script>

<!-- ContentBox runtime -->
<script src="box/box-flex.js"></script>

</body>
</html>
```

## **🤖 AI Assistant Integration**

ContentBox.js includes an **AI Assistant panel** that allows users to generate editable content using AI.

To use the AI Assistant, you need a **server-side endpoint** that handles communication with an AI  provider (like OpenAI or OpenRouter).

You can configure this endpoint using the **`sendCommandUrl`** option:

```jsx
const builder = new ContentBox({
    // ...
    sendCommandUrl: '/sendcommand' // Server endpoint for handling AI commands
    // aiSectionReplace: true, // Optional: Replace current section with AI result
});
```

The package includes ready-to-use endpoint examples in **Node.js** and **PHP** , making it easy to set up and test the AI Assistant.

By default, the **`aiSectionReplace`** option is set to **`false`**. This means that when a user requests changes (e.g., revising a headline), the AI result is added as a **new section** , allowing for comparison.

If you set **`aiSectionReplace: true`**, the AI-generated result will **replace the current section directly** .

### **Steps to Use the AI Assistant**

**1. Get an API Key**

You'll need an API key from one of the following services:

- **OpenAI** – [https://openai.com](https://openai.com/)
- **OpenRouter** – [https://openrouter.ai](https://openrouter.ai/)
    
    > Use OpenRouter if you want access to multiple AI models beyond just OpenAI.
    > 

**2. Configure Your API Key**

Update your environment file with the appropriate key:

**For Node.js:**

Edit **`.env`**:

```jsx
OPENAI_API_KEY=your_openai_api_key
# or
OPENROUTER_API_KEY=your_openrouter_api_key
```

**For PHP:**

Edit **`api/config.php`**:

```jsx
$OPENAI_API_KEY = 'your_openai_api_key';
// or
$OPENROUTER_API_KEY = 'your_openrouter_api_key';
```

**3. Set the Correct Endpoint**

Make sure the **`sendCommandUrl`** points to your server-side endpoint URL:

```jsx
const builder = new ContentBox({
    // ...
    sendCommandUrl: '/sendcommand', // default endpoint for AI commands
    // sendCommandUrl: '/openrouter', // alternative for OpenRouter
});
```

**For PHP:**

```jsx
const builder = new ContentBox({
    // ...
    sendCommandUrl: 'api/sendcommand.php', // PHP backend
    // sendCommandUrl: 'api/openrouter.php', // OpenRouter support
});
```

Most examples in the package already have this configured, so you can run and test them right away.

💡 The JavaScript example uses **Node.js** (**`server.js`**) as the backend for the **`/sendcommand`** endpoint. 

### **Optional: AI Image Generation**

ContentBuilder also supports AI-generated images via external API.

**1. Get an API Key**

🔗 [https://getimg.ai/tools/api](https://getimg.ai/tools/api)

**2. Configure Your API Key**

**For Node.js:**

Edit **`.env`**:

```jsx
GETIMG_API_KEY=your_getimg_api_key
```

**For PHP:**

Edit **`api/config.php`**:

```jsx
$GETIMG_API_KEY = 'your_getimg_api_key';
```

**3. Set the Endpoints and Model**

```jsx
const builder = new ContentBox({
    // ...
    textToImageUrl: '/texttoimage', 
    upscaleImageUrl: '/upscaleimage',
    imageModel: 'flux-schnell',
    // imageAutoUpscale: false,
});
```

**For PHP:**

```jsx
const builder = new ContentBox({
    // ...
    textToImageUrl: 'api/texttoimage.php', 
    upscaleImageUrl: 'api/upscaleimage.php',
    imageModel: 'flux-schnell',
    // imageAutoUpscale: false,
});
```

Once configured, users can generate images directly from the AI Assistant panel.

## **🔤 Language File Support**

ContentBox.js supports interface translation through language files. This allows you to display the editor UI in different languages.

### **Provided Language Files**

The package includes the following language files:

```jsx
contentbox/lang/en.js
```

You can create your own language file by copying and modifying an existing one, then translating the values accordingly.

### **Enabling a Language**

To enable a language, include the corresponding language file **before** loading **`contentbox.min.js`**:

```jsx
<script src="contentbox/lang/fr.js" type="text/javascript"></script>
```

### **Using with React, Vue or JavaScript Projects**

If you're using a frontend framework, you can import and pass the language object directly during initialization.
First, create your language file (e.g. **`locales/en.js`**) like this:

```java
const _txt = new Array();
_txt["Bold"] = "Bold";
_txt["Italic"] = "Italic";
// ...
export default _txt;
```

Then import it into your project and assign it when creating the ContentBox instance:

```jsx
import texts from '../Locales/fr';
```

```jsx
builder = new ContentBox({
    /* other options */
    
    lang: texts
});
```

With this system, you can easily localize the entire ContentBox.js interface to match your users' preferred language.

## **🧩 Section Templates**

To help users start building a page, ContentBox includes a section template library. Users can open it by clicking the **(+) button** in the sidebar.

Three default template sets are available:

- **Simple Start** – basic, clean designs
- **Quick Start** – ready-to-use examples
- **Animated Sections** – sections with animations

Each set is located in its own folder:

```jsx
assets/templates-simple/
assets/templates-quick/
assets/templates-animated/
```

Each folder contains:

- **`templates.js`** – JSON file with predefined section templates
- **`images/`** – assets used in the templates
- **`preview/`** – thumbnail images for preview

### **🔧 Configuration**

By default, ContentBox loads these templates automatically using the built-in configuration:

```jsx
const builder = new ContentBox({
    /* other options */
    templates: [
        {   
            url: 'assets/templates-simple/templates.js',
            path: 'assets/templates-simple/', 
            pathReplace: [],
            numbering: true,
            showNumberOnHover: true,
        },
        {   
            url: 'assets/templates-quick/templates.js',
            path: 'assets/templates-quick/', 
            pathReplace: [],
            numbering: true,
            showNumberOnHover: true,
        },
        {   
            url: 'assets/templates-animated/templates.js',
            path: 'assets/templates-animated/', 
            pathReplace: [],
            numbering: true,
            showNumberOnHover: true,
        },
    ],
});
```

You can customize this to load templates from different locations or apply path replacements:

```jsx
const builder = new ContentBox({
    wrapper: '.is-wrapper',
    templates: [
        {   
            url: 'https://path-to/assets/templates-simple/templates.js',   
            path: 'https://path-to/assets/templates-simple/',    
            pathReplace: [],
        },
        // ... other template sets
    ],
});
```

Use the **`pathReplace`** array if you need to rewrite parts of the paths defined in the template files.

### **📄 Template File**

Let’s look at an example template file from the **Simple Start** set: **`assets/templates-simple/templates.js`**

```jsx
var data_templates = {
    name: 'Simple Start',
    categories: [
        { id: 1, name: 'Basic' }, 
        { id: 2, name: 'Slider' }, 
        { id: 3, name: 'Video' }, 
        { id: 4, name: 'Custom' }
    ],
    designs: [
        {
            'thumbnail': 'preview/simple-01.png',
            'category': '1',
            'contentCss': 'type-poppins.css',
            'contentClass': 'type-poppins',
            'html': `
                <div class="is-section is-box is-section-100 type-poppins">
                    ...
                </div>`
        }
    ]
};
```

Each section template must include the following properties:

- **`thumbnail`** – Relative path to the preview image (e.g. **`preview/simple-01.png`**)
- **`category`** – The category ID this design belongs to
- **`contentCss`** – CSS file for typography (choose from **`assets/styles/`**, must start with **`type-`**)
- **`contentClass`** – CSS class name (use the filename without extension)
- **`html`** – HTML markup of the section

💡 Inside the **`html`** string, you can use the placeholder **`[%IMAGE_PATH%]`** which will be replaced with the correct asset path when loaded: 

```jsx
<img src="[%IMAGE_PATH%]images/photo.jpg">
<!-- becomes -->
<img src="assets/templates-simple/images/photo.jpg">
```

### **🔧 Featuring Specific Categories**

The **`featuredCategories`** option is used to feature selected categories when the panel is opened.

```jsx
const builder = new ContentBox({
		/* other options */
    featuredCategories: [
        { id: 1, designId: 1, name: 'Basic' },
        { id: 1, designId: 2, name: 'Header' },
        { id: 2, designId: 1, name: 'Slider' },
        { id: 2, designId: 2, name: 'Article' },
        { id: 3, designId: 2, name: 'Photos' }
    ],
    defaultCategory: {
        id: 1,
        designId: 1
    }
});
```

- **`designId`**: Refers to the order in which the template sets are loaded (1 = Simple Start, 2 = Quick Start)
- **`id`**: The category ID inside that set
- **`defaultCategory`**: Selects the initial category shown when the panel opens

## **🧩 Snippets Management**

Snippets are pre-designed content blocks that you can add or drag & drop into your content.

When you add a content block by clicking the **(+) icon** , you can also select the **MORE** button to open the snippets dialog.

Snippet files are located in the folder:

```jsx
assets/minimalist-blocks/
```

This folder contains:

- **`content.js`** – The file containing the available snippets
- **`content.css`** – Styles used by the snippets
- **`images/`** – Image assets used in the snippets

### **Configuring Snippet Paths**

If you host your assets remotely (e.g., on a CDN), you can configure the paths during initialization:

```jsx
const builder = new ContentBox({
    /* options */

    snippetUrl: 'https://path.to/assets/minimalist-blocks/content.js',  // Full URL to snippet file
    snippetPath: 'https://path.to/assets/minimalist-blocks/',  // Base path for images and resources
    snippetPathReplace: [
        'assets/minimalist-blocks/images/',
        'https://path.to/assets/minimalist-blocks/images/' 
    ] // Replace local image paths with remote URLs
});
```

These options ensure that all relative paths inside the snippets (especially image URLs) resolve correctly when hosted externally.

### **Customizing Snippets**

You can add or edit snippets by modifying the **`content.js`** file. It contains the list of available snippets in JSON format.

Please refer to the *ContentBuilder.js* documentation, specifically the “Customizing Snippets” section:

[https://github.com/innovastudio-dev/contentbuilder](https://github.com/innovastudio-dev/contentbuilder)

## **🎨 Other Configuration Options**

### **🌈 Color Picker**

By default, ContentBox includes a color picker with a set of predefined colors. You can customize this list using the **`colors`** option:

```jsx
const builder = new ContentBox({
    /* options */
    colors: [
        "#ff8f00", "#ef6c00", "#d84315", "#c62828", "#58362f", "#37474f", "#353535",
        "#f9a825", "#9e9d24", "#558b2f", "#ad1457", "#6a1b9a", "#4527a0", "#616161",
        "#00b8c9", "#009666", "#2e7d32", "#0277bd", "#1565c0", "#283593", "#9e9e9e"
    ]
});
```

This defines the color palette available in the editor’s color picker dropdown.

### **🏷️ Custom Tags (for CMS Integration)**

Custom tags allow you to insert dynamic placeholders into content, which can be replaced with custom values at runtime.

You can define custom tags like this:

```jsx
const builder = new ContentBox({
    /* options */
    customTags: [
        ["Site Name", "{%SITENAME%}"],
        ["Logo", "{%LOGO%}"],
        ["My Plugin", "{%MY_PLUGIN%}"]
    ]
});
```

These tags appear in the tags menu (on the editing toolbar) and let users add placeholders directly into their content.

### **📋 Paste Result Behavior**

When users paste content from external sources (like Word or another website), you can control how it is handled using the **`paste`** option:

```jsx
const builder = new ContentBox({
    /* options */
    paste: 'text' // or 'html', 'html-without-styles'
});
```

**Values:**

- **`'text'`** – Only plain text is pasted (default)
- **`'html'`** – Full HTML content including styles
- **`'html-without-styles'`** – HTML content without inline style attributes

This helps maintain clean formatting based on your use case.

This ensures the editor starts fresh each time, ignoring any stored settings.

### **📋 Control the Editing on Mobile**

You can control when the editing interface switches to a mobile-friendly layout using the **`simpleEditingBreakpoint`** option.

```jsx
const builder = new ContentBox({
    /* options */
    simpleEditingBreakpoint: 0 // Show editing controls on mobile (default: '970px')
});
```

- **`simpleEditingBreakpoint`** : Sets the maximum width (in pixels) at which the mobile editing layout is used.
- By default, mobile layout is activated at **`970px`** or below.
- Setting it to **`0`** disables the mobile layout entirely, always showing the full editing interface.

### **⚙️ Set Custom Headers for Server Requests**

When ContentBuilder.js interacts with a server — such as when using AI features via the **`sendCommandUrl`** or other backend integrations — it uses the **`fetch()`** API to send requests.

You can customize the HTTP headers sent with these requests using the **`headers`** option:

```jsx
const builder = new ContentBox({
    /* options */
    headers: {
        'Authorization': 'Bearer YOUR_TOKEN_HERE',
        'X-Custom-Header': 'CustomValue'
    }
});
```

🔒 **Note:** The **`'Content-Type': 'application/json'`** header is automatically added by default. You do not need to include it.

These custom headers will be included in all internal **`fetch()`** requests made by the editor.

## **⚙️ Methods**

### **📘 Get Generated HTML**

Retrieve the current HTML content built inside the editor:

```jsx
const html = builder.html();
```

You can then send this HTML to a server for saving.

### **🎨 Get the Styles**

```jsx
const mainCss = builder.mainCss(); // Returns the default typography style for the page.
const sectionCss = builder.sectionCss(); // Returns the typography styles for specific sections on the page
```

In production, the saved HTML content should be rendered with the styles.

### **📥 Load HTML Content Programmatically**

To completely replace the current content in the editor with new HTML, use the **`loadHtml()`** method:

```jsx
builder.loadHtml(html);
```

This is ideal for loading saved content from a database or pre-defined template.

Example:

```jsx
const myContent = `
  <div class="is-section is-section-100 is-box type-system-ui">
	    <div class="is-overlay"></div>
	    <div class="is-container v2 size-18 leading-14 is-content-1300">
	    <div class="row">
	        <div class="column">
	            <h1 class="leading-11 size-88">Hello World</h1>
	        </div>
	    </div>
	    </div>
	</div>
`;

builder.loadHtml(myContent);
```

### **📥 Load Styles into Builder**

Load previously saved styles to restore typography:

```jsx
builder.loadStyles(mainCss, sectionCss); 
```

### **⚙️ Programmatically Open HTML Code Editor**

You can open the HTML code editor from code using:

```jsx
builder.viewHtml();
```

### **⬇️ Download the Generated HTML**

Triggers downloading the current content as an HTML file.

```jsx
builder.download();
```

### **🎞️ Open Animation Panel**

Opens the basic animation settings panel for quick adjustments.

```jsx
builder.openAnimationPanel();
```

### **⏱️ Open Animation Timeline**

Opens the advanced timeline-based animation editor.

```jsx
builder.openAnimationTimeline();
```

### **🤖 Open AI Assistant Panel**

Launches the AI Assistant panel for content generation.

```jsx
builder.openAIAssistant();
```

### **🧹 Clear Content**

Clears all content from the current page.

```jsx
builder.clear();
```

### **⚙️ Change Element Selection Behavior**

By default, when a user presses **CMD+A (Mac)** or **CTRL+A (Windows)** inside an editable area, ContentBox selects only the current element (such as a paragraph or heading) — not the entire text in a column.

You can control this behavior using the **`elementSelection`** option:

```jsx
const builder = new ContentBuilder({
    container: '.container',
    elementSelection: true // Default behavior
});
```

- When **`elementSelection: true`** – Selects only the current element inside the column.
- When **`elementSelection: false`** – Selects the entire column content.

### **↩️ Programmatically Trigger Undo or Redo**

You can also trigger undo or redo actions from your code using the following methods:

```jsx
builder.undo();  // Reverts the last action

builder.redo();  // Re-applies the last undone action
```

These are useful when integrating with custom UI elements.

### **🛑 Destroying the Editor Instance**

To completely remove a ContentBox instance, use:

```jsx
builder.destroy();
```

## **⚡ Event Handling**

ContentBox.js provides several built-in event hooks that allow you to respond to user actions and content changes in real time. These events are useful for integrating with external systems or applying custom logic.

### **🔄 `onRender` – Triggered When Content Is Updated or Re-rendered**

This event is fired whenever the content structure changes — such as when a new snippet is added, a block is moved, or layout changes occur.

Use it to re-apply custom scripts or behaviors after content updates.

**Example:**

```jsx
const builder = new ContentBox({
    /* options */
    onRender: function() {
        console.log('Content structure updated');
    }
});
```

### **📝 `onChange` – Triggered When Content Is Edited**

This event fires whenever the actual content is modified — for example, when text is typed, formatted, or deleted.

Useful for auto-saving content.

**Example:**

```jsx
const builder = new ContentBox({
    /* options */
    onChange: function() {
        console.log('Content was edited');
    }
});
```

### **✅ `onStart` – Triggered When Editor Is Initialized**

This event is called once when the editor has fully loaded and is ready for interaction.

Useful for initializing external integrations or UI enhancements after the editor starts.

**Example:**

```jsx
const builder = new ContentBox({
    /* options */
    onStart: function() {
        console.log('Editor has started');
    }
});
```

## **📁 Path Configuration**

When hosting ContentBox.js or its associated asset files in a different location — such as a CDN, cloud storage, or separate directory — you can use the following configuration options to ensure correct file loading and path resolution.

Here’s an example of how to set up custom paths:

```jsx
const builder = new ContentBox({
    /* options */
    controlPanel: true,
    iframeSrc: 'blank.html' // Must be served from the same origin

    // Base path for general assets
    assetPath: 'https://path.to/assets/', 

    // Font assets path
    fontAssetPath: 'https://path.to/assets/fonts/', 

    // Snippet file and related paths
    snippetUrl: 'https://path.to/assets/minimalist-blocks/content.js',  // Full URL to snippet file
    snippetPath: 'https://path.to/assets/minimalist-blocks/',  // Base path for images and resources
    snippetPathReplace: [
        'assets/minimalist-blocks/images/',
        'https://path.to/assets/minimalist-blocks/images/' 
    ], // Replace local image paths with remote URLs

    // Module and plugin paths
    modulePath: '/assets/modules/',  // module files. Use local path to avoid CORS issues (e.g., public/assets/modules/)
    pluginPath: 'https://path.to/contentbuilder/',  // plugin files

    // Media path for AI-generated content (e.g., random images)
    mediaPath: 'https://path.to/assets/gallery/',
    
    // Typography styles path
    contentStylePath: 'https://path.to/assets/styles/',
    
    // Templates path
    templates: [
        {   
            url: 'https://path.to/assets/templates-simple/templates.js',
            path: 'https://path.to/assets/templates-simple/', 
            pathReplace: [],
            numbering: true,
            showNumberOnHover: true,
        },
        {   
            url: 'https://path.to/assets/templates-quick/templates.js',
            path: 'https://path.to/assets/templates-quick/', 
            pathReplace: [],
            numbering: true,
            showNumberOnHover: true,
        },
        {   
            url: 'https://path.to/assets/templates-animated/templates.js',
            path: 'https://path.to/assets/templates-animated/', 
            pathReplace: [],
            numbering: true,
            showNumberOnHover: true,
        },
    ],
});
```

These settings allow full control over where ContentBox looks for external files — making it easy to host assets remotely or serve them from optimized delivery networks.

## **🧩 Extending ContentBox with a Custom Panel**

You can extend ContentBox by adding a custom sidebar button that opens a **custom panel** — allowing users to insert predefined content or sections into the editor.

### **🔧 Step 1: Add a Sidebar Button**

Use the **`addButton()`** method to add a new button to the sidebar. This button will open your custom panel HTML file.

```jsx
builder.addButton({ 
    pos: 0,
    title: 'Products',
    src: 'mypanel.html', // Path to your custom panel HTML
    html: '<svg style="width:17px;height:17px;" viewBox="0 0 2048 2048" xmlns="http://www.w3.org/2000/svg">...</svg>',
    class: 'sidebar-sections'
});
```

- **`src`**: The path to your custom HTML panel (e.g., **`mypanel.html`**)
- **`html`**: SVG icon markup for the button
- **`class`**: Optional CSS class for styling

> ✅ Tip: You can find a working example in the package at public/example-custom.html.
> 

### **💡 Step 2: Create Your Custom Panel (`mypanel.html`)**

Create a simple HTML file (**`mypanel.html`**) containing buttons or links that call **`parent.contentbox.addSection()`** to insert content.

Each button should execute:

```jsx
parent.contentbox.addSection(html, css);
```

**Parameters:**

- **`html`** : The HTML structure of your section
- **`css`** : A typography CSS file from **`assets/styles/`** (choose one starting with **`type-`**)

**Example: Inserting a Section**

```jsx
parent.contentbox.addSection(`
    <div class="is-section is-box is-section-100 type-rufina-oxygen">
        <div class="is-boxes">
            <div class="is-box-centered">
                <div class="is-container v2 is-content-960 leading-13 size-17">
                    <div class="row">
                        <div class="column full">
                            <h1 class="text-center leading-09 size-92">Product One</h1>
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </div>
`, 'type-rufina-oxygen.css');
```

Make sure to:

- Use a valid **`type-*.css`** file from the **`assets/styles/`** folder
- Apply the matching class name to the **`.is-section`** element

### **🔒 Make Content Non-editable (Optional)**

To make a section **non-editable** , add the **`protected`** class to the **`.is-section`** element and avoid using the **`is-container`** class inside it:

```jsx
<div class="is-section is-box is-section-100 type-rufina-oxygen protected">
    ...
</div>
```

### **⚙️ Add Custom JavaScript (Optional)**

You can embed custom scripts inside a section using the **`data-module`** attribute:

```jsx
<div class="is-overlay-content"
     data-module="code"
     data-module-desc="Custom HTML or JavaScript"
     data-html="${encodeURIComponent(`

        <!-- Your custom script here -->

    `)}">
</div>
```
