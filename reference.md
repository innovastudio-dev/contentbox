# Configuration Reference

This document provides detailed configuration options and advanced features for ContentBox.js.

If you're new to ContentBox, start with the [Getting Started Guide](https://github.com/innovastudio-dev/contentbox) which covers installation, basic setup, and core features. This reference is designed for when you need specific customization options beyond the basics.

## ➕ Adding Custom Buttons to the Left Sidebar

You can extend the default sidebar with your own buttons using the `addButton()` method.

Each button is defined by:

- `pos`: The position index where the button should be inserted
- `title`: Tooltip/title shown on hover
- `html`: The icon markup (typically an SVG)
- `onClick`: The function to execute when the button is clicked

### Example: Adding Animation & Utility Buttons

```javascript

builder.addButton({ 
    pos: 2,
    title: 'Animate Section',
    html: '<svg><use xlink:href="#icon-wand"></use></svg>', 
    onClick: () => {
        builder.openMotionCascade();
    }
});

builder.addButton({
    pos: 3,
    title: 'Motion Timeline',
    html: '<svg><use xlink:href="#icon-anim-timeline"></use></svg>',
    onClick: () => {
        builder.openMotionDock();
    }
});

// Open custom modal
builder.addButton({ 
    pos: 4,
    title: 'Assets',
    html: '<svg><use xlink:href="#icon-folder"></use></svg>', 
    onClick: async () => {
        builder.openModal('assets.html', {
            ariaLabel: 'Assets',
            // w: '88vw',
            // maxW: '1600px',
            // h: '88vh',
            // onOpen: (modal, iframe) => console.log('Opened:', modal, iframe),
            // onClose: (modal, iframe) => console.log('Closed:', modal, iframe),
            // showCloseButton: true,
        });
    }
});

builder.addButton({ 
    pos: 5,
    title: 'Clear Content',
    html: '<svg><use xlink:href="#icon-eraser"></use></svg>', 
    onClick: () => {
        builder.clear();
    }
});

builder.addButton({ 
    pos: 6,
    title: 'AI Assistant',
    html: '<svg><use xlink:href="#icon-sparkles"></use></svg>', 
    onClick: () => {
        builder.openAIAssistant();
    }
});
```

### Available ContentBox Methods

- `builder.openMotionCascade()` - Opens the 'Animate Section' panel.
- `builder.openMotionDock()` - Opens the Motion editor.
- `builder.openAIAssistant()` - Opens the AI Assistant.
- `builder.clear()` - Clears all content from the current page.
- `builder.modal(options)` - Builds a dialog from your own content. See 'Build Your Own Dialog'.


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

### **Using with React/Vue/Modern Frameworks**

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

### **⌨️ Keyboard Access**

Everything in a row, a column, an element or a piece of text can be done
without a mouse. With the cursor in the content, `Esc` (or `Alt+F10`) moves to
the tools for whatever you are in, and each `Esc` after that steps out one
level: the text tools, the tools of the picture or table you are in, the
element, the column, the row and the section, and back to your text.
`Shift+Esc` steps back in.

Each strip is a toolbar in its own right: the arrows move along it, `Home` and
`End` jump to its ends, and `Enter` or `Space` uses a button. A panel that opens
from a button takes the focus and hands it back when `Esc` closes it.

The toolbar along the top and the panel buttons down the side are part of the
page's normal tab order, so `Tab` reaches them and `Enter` opens one.

See `docs/keyboard-access.md` for every key, with worked examples.

Below 970px the tools are hidden, since laying out a page on a narrow screen is
awkward. To keep them at every width, set **`simpleEditingBreakpoint`** to an
empty string:

```jsx
const contentbox = new ContentBox({
    wrapper: '.is-wrapper',
    simpleEditingBreakpoint: ''
});
```

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
    paste: 'auto' // or 'html', 'html-without-styles', 'text'
});
```

**Values:**

- **`'auto'`** – Pasted content adopts the destination block (for example, pasting a heading into a paragraph keeps it a paragraph) while preserving text formatting such as bold, italic, links, and colors (default)
- **`'html'`** – Full HTML content including styles
- **`'html-without-styles'`** – HTML content without inline style attributes
- **`'text'`** – Only plain text is pasted

### **🧹 Unwanted Markup from Browser Extensions**

Some browser extensions add their own elements and attributes to any text area they find, including the editor. That markup is not part of the content, so it is removed when the content is saved: the injected elements and attributes go, and the text they wrapped stays.

This happens automatically. No configuration is needed.

If your users have an extension that isn't covered, add its markup to the cleanup list:

```jsx
const builder = new ContentBox({
    /* options */
    extensionArtifacts: {
        elements: ['my-extension-overlay'],     // removed entirely
        wrappers: ['span[data-myext-id]'],      // removed, the text inside is kept
        attributePrefixes: ['data-myext-'],     // attributes removed by prefix
        stylePrefixes: ['--myext-']             // style properties removed by prefix
    }
});
```

Every list is added to the built-in ones.

Extensions keep working while your users edit. If you would rather they didn't run inside the editing areas at all, you can turn them off there:

```jsx
const builder = new ContentBox({
    /* options */
    disableEditorExtensions: true
});
```

Saved content is cleaned either way.

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

### **🎞️ Open 'Animate Section' panel**

Opens the 'Animate Section' panel.

```jsx
builder.openMotionCascade();
```

### **⏱️ Open Motion Editor**

Opens the Motion (timeline) editor.

```jsx
builder.openMotionDock();
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

### **🪟 Build Your Own Dialog**

`builder.modal()` builds a dialog around your content. You supply what goes
inside; the shell, title bar, close button, accessibility attributes, padding and
dragging are handled for you:

```jsx
const wordCount = builder.modal({
    title: 'Word Count',
    width: 320,
    className: 'wordcount',   // optional hook for your own CSS
    html: `
        <div>
            <div style="line-height:1">
                <span class="wc-words" style="font-size:56px;font-weight:700;">0</span>
                <span style="font-size:15px;margin-left:6px;">words</span>
            </div>
        </div>`
});
```

It returns a handle:

```jsx
wordCount.open();     // show it
wordCount.close();    // hide it
wordCount.body        // the content wrapper - query your own markup from here
wordCount.el          // the dialog element
```

So a custom button becomes:

```jsx
builder.addTextButton(buttonHtml, '.wordcount-button', () => {
    wordCount.body.querySelector('.wc-words').textContent = countWords();
    wordCount.open();
});
```

**Options**

| Option | Description |
| --- | --- |
| `html` | The content. Required. |
| `title` | Title bar text. Omit for a bare dialog with a floating close button. |
| `width` | `320` for pixels, or any CSS length such as `'40rem'`. |
| `padding` | Overrides the body padding. Skipped automatically if your outermost element already sets padding inline. |
| `closeButton` | Default `true`. |
| `draggable` | Default `true`. Applies to the title bar. |
| `className` | Extra class on the dialog, for your own CSS. |
| `onOpen` / `onClose` | Callbacks, each receiving the handle. |

### **🖥️ Page Preview**

A fullscreen preview of the page with a width ruler, for checking the layout at
any size without leaving the editor.

```jsx
builder.openPagePreview();
```

The frame loads your `previewURL`, and the content reaches that page through the
existing `onPreviewOpen` hook:

```jsx
const builder = new ContentBox({
    previewURL: 'preview.html',
    onPreviewOpen: () => {
        // default: saves builder.html(), mainCss() and sectionCss() to localStorage
    },
});
```

Nothing happens if `previewURL` is not set.

> Not to be confused with `openPreview()`, which opens Live Preview, the small
> floating window. They are separate tools and both remain available.

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
    modulePath: 'https://path.to/assets/modules/',
    pluginPath: 'https://path.to/contentbuilder/',

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
      
    // Form Builder module paths
    assetsFolder: 'https://path.to/assets/formfiles/',
    exampleImageUrl: 'https://path.to/assets/formfiles/ai-V8Khk.jpg',
    exampleVideoUrl: 'https://path.to/assets/formfiles/ai-thhyR.mp4',
    exampleAudioUrl: 'https://path.to/assets/formfiles/ai-3EVMb.wav',
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

