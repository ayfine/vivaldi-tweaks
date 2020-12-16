# Vivaldi Tweaks

This repository contains tweaks and customizations for [Vivaldi Browser](https://vivaldi.net/). The documentation assumes working knowledge of messing around with Vivaldi in general, as well as css and html.

Because Vivaldi is closed source (especially the UI layer) the documentation for tweaks may be annoyingly vague at times.

## References

### Important Paths

(For Windows)

`%localappdata%\Vivaldi\Application\[version]\resources\vivaldi\`
`%localappdata%\Vivaldi\Application\[version]\resources\vivaldi\user_files`
`%localappdata%\Vivaldi\Application\User Data\default`


```
%localappdata%\Vivaldi
├── Application
│		└── [version]
│   		├── resources
│   		└── vivaldi
│       		├── background-common-bundle.js
│       		├── user_files
│       		│ 	└── [page action css/js]
│       		└── style
│       				├── common.css
│       				└── common.js
└── User Data
		└── default
    	├── extensions
    	└── cache
```

### Vivaldi URLs List
`vivaldi://vivaldi-urls/`

### Browser Inspector

The browser inspector can be found at the Vivaldi URL `vivaldi://inspect/#apps`.

It can also be added to the general context menu by setting the flag `vivaldi://flags/#debug-packed-apps` to `enabled`

Some of these are useful for menu customization, like `vivaldi://bookmarks`

### Vivaldi Chrome Policies

Vivaldi Chrome Policies can be accessed at `vivaldi://policy/`. Vivaldi does not technically support these policies as far as I am aware and some behavior may be unexpected.

The only Chrome Policy I use personally is [**DisablePrintPreview**](https://cloud.google.com/docs/chrome-enterprise/policies/?policy=DisablePrintPreview "View Documentation") to force Vivaldi to use the system print dialog instead of Chrome's print preview (which as of the time of writing cannot be swapped via keyboard shortcuts on Windows 10).

To do this on Windows 10:

> - Create a new key named `Vivaldi` in `Computer\HKEY_LOCAL_MACHINE\SOFTWARE\Policies\`
> - Add a new `REG_DWORD` in the newly created key (`Computer\HKEY_LOCAL_MACHINE\SOFTWARE\Policies\Vivaldi`) with the name `DisablePrintPreview` and a value of `1` (for true)
> - To confirm that the policy has been set navigate to `vivaldi://policy`. You may need to click `Reload Policies` even after restarting your browser.
> - If it's been set correctly `DisablePrintPreview` will show up with the set value, and using the `Ctrl+P` shortcut will go directly to the Windows System print dialog  

I was not succesfully in using [**AutoOpenFileTypes**](https://cloud.google.com/docs/chrome-enterprise/policies/?policy=AutoOpenFileTypes).

## CSS Tweaks

Recent versions of Vivaldi allow a styles directory to be defined in the general settings (previous versions required chrome userstyles type styles) and can be placed anywhere.

Here are a few I use.

#### Uniform Browser UI text-size
``` css
#browser, #browser + div, #browser + div + div {
	font-size:12px !important;
}
```

#### Hide Synced Tabs Icon
``` css
.synced-tabs-button {
	display:none !important;
}
```

#### Remove Spacing Between Tab Bar and Horizontal Menu
``` css
.tabs-top.stacks-on#browser.native.linux .topmenu,
.tabs-top.stacks-on#browser.native.win .topmenu {
	margin-bottom:0px !important;
}
#browser.native.linux.normal #tabs-container.top,
#browser.native.win.normal #tabs-container.top {
	padding-top:0px !important;
}
```

#### Remove Spacing Between 'Add Web Panel' and 'Settings'

``` css
#switch > button.preferences {
	margin-top:initial !important;
}
```

#### Make Web Panel Headers Bold

```css
.panel > header h1 {
	font-weight:bold !important;
}
```
#### Active Web Panel Color + Bold
```css
#switch > button.active {
	fill:var(--colorHighlightBg);
	color:var(--colorHighlightBg);
	stroke-width:.5px;
	stroke:var(--colorHighlightBg);
}
```
#### Hide Trash Icon
```css
.button-toolbar.toggle-trash {
	display:none !important;
}
```

#### Change Panel Icon Order
```css
#switch > button.name {
	order: int
}
```

#### Make Extension Drawer Icon Less Prominent
```css
.extensionPopupIcons svg {
	height:20px !important;
	fill:var(--colorFgFadedMost) !important;
}
```

#### Disable Panel Icon on click scaling
```css
#switch .addwebpanel-wrapper > button:active svg,
#switch > button:active svg {
	transform:scale(1) !important;
}
```

#### Hide Reader View Button in Address Bar

Somewhat useful if you are using an alternative Reader View extension like [Mecury Reader](https://chrome.google.com/webstore/detail/mercury-reader/oknpjjbmpnndlpmnhmekjpocelpnlfdi?hl=en). Important to note that setting the flag `vivaldi://flags/#enable-reader-mode` to disabled does not work and won't remove the icon from the address bar.

```css
button[title="Reader View"] {
	display:none !important;
}
```


## UI SVG Icons

Vivaldi's inline UI SVG icons can be somewhat customized via CSS but does not work well for more complicated icons. [This method of customization is documented on the Vivaldi forums](https://forum.vivaldi.net/topic/20227/changing-icons-with-css-part-ii?page=1).

An example of the above from user **luetage** is below in the event that the thread gets deleted or moved.

```css
.UrlBar button[title="Go to homepage"] svg path {
    d: path("M 4 6 h 18 v 2.5 h -3.5 v 11.5 h -2.5 v -9.5 h -6 v 9.5 h -2.5 v -11.5 h -3.5 Z");
}
```

Almost all of Vivaldi's icons are inline SVGs loaded into the browser from `background-common-bundle.js`, which as compiled and minified is ~1.5MB. To make this file less cumbersome to traverse I recommend beautifying it (**after backing up the original**), and the below notes use line numbers that will assume that you are working with it beautified.

This can be done a variety of ways, I use the the python package [js-beautify](https://github.com/beautify-web/js-beautify). The online version may struggle with the file size - if so, install the command line version.

The definitions of the UI SVGs can be found in the ballpark of lines `39400-39650` `46200-46500`. If these line numbers are not helpful for whatever reason, search for `e.exports = '<svg width="16" height="16" xmlns="http://www.w3.org/2000/svg">` to get in the right area.

The following are in order of how they appear in `background-common-bundle.js` in Vivaldi version 3.5.2115.81, with unique strings (pulled from as early in the code as possible) to help identify them as well.

Changes made will be reverted on browser update.

String | Icon
:----|:-----
`M11.1425 14.123L8 10.9091L4.8575`| Address Bar Bookmark Button
`M1 2.5A1.5 1.5 0 012.5`| Start Page Tab Favicon
`M5 2C4.44772 2`| Bookmark Tab Favicon
`M7.329 5a.75.75` | Warning symbol
`M7.13381 2.50139` | Warning symbol
`M11.256 4.73144L2.67246` | Warning Symbol
`M15.2929 20.7071C15.6834` | Main navigation back button
`M9.29289 19.2929C8` | Main navigation forward button
`M17 18C17 18.5523 16.5523` | Main navigation fast forward
`M9 8C9 7.44772` | Main navigation rewind
`M14.0607 5.14645C13` | Main navigation home button
`M20 6.20711C20 5.76166`| Main navigation refresh button
`M8.70711 7.29289C8.31658` | Close X symbol
`M16.2929 20.2929L13` | **Bookmark Panel**
`M13 6C11.8954` | Down arrow
`M4 2C4 1.44772`| Calendar
`M4 1C3.44772 1 3`| Calendar
`M21 9C21 7.89543`| Calendar
`M8 14C11.3137 14 14 11.3137`|Alarm Clock - small
`M8 13.4C10.9823` | Alarm Clock - medium
`M13 20.3C17.0317` | Alarm Clock - Large
`M0 2h14.546c.359 0 .715`| Send/share outline
`M16 3.869c0-.557-.461` | Send/share solid
`M22 8.086c0-.696`|Send/share solid
`M7.5 4C7.22386 4 7`|History
`M7 5.5C7 5.22386 7.22386` | History
`M5 13C5 11.1401 5`| **History Panel**
`"m6.64645 7.64645c`|Email
`M13.414.5c-.398 0-.7`|Notes
`M14.05 1.28a.96.96 0`|Notes
`M9 16h2.53l7-7`|Notes
`M6.32251 9`|Contacts
`M10.4243 7.42417`|Selection ??
`M2 1C1.44772 1 1 1.44772`| Selection ??
`M7 6C6.44772 6 6 6.44772`|Selection
`9 3.5c0-.27614-.22386`|Email send?
`M2 4.5a.5.5`|Sort?
`M1 3.5a.5.5 0 01.5-.5h13`|Sort? Bold
`M4.5 6a.5.5 0 00-.5`|Sort?
`M0 2h14.546c.359 0`|Send/share? Outline
`M16 3.869c0-.557`|Send/share? Solid
`M22 8.086c0-.696`|Send/share? solid
`M9.915 6.939h3`|Circular arrows?
`M10.065 7.082l4`|Circular arrows
`M19.929 12h.071`|Circular arrows
`M3 2h10v1H3V2zM2`|Trash?
`M13 3v2H3V3h10zM2.1`|Trash?
`M19 9V7H7v2h12zm2`|Trash?
`M8 6.5h.5v-2`|Right arrow?
`M8.006 3.839`|Right arrow solid
`M12.007 8.00`|Right arrow solid
`M7.5 6.001l.001`|Undo?
`M7.994 3`|Undo solid
`M13.993 8.007l.00`|Undo
`M8.5 6.001l.001.499h`|Undo two layers?
`M6.203 3.914c0`|Undo double?
`M11 7.895c0`|Same as above
`M7.256 13h-2.256`|Delete Note
`M11.5 7.091v-`|Delete note
`M5 6c0-1.1.9`|Delete note
`M6 4c0-.552-.4`|Directory tree?
`M8 6c1.105 0 2`|Same as above
`M8 6c1.105`|Same as above
`M3 4c0-.552.448-1`|List?
`M8 16c1.105 0 2`|Same as above
`M8 16c1.105 0 2 .895`|Similar to above
`m7.29386.706223c`|Email open
`"M1.293 7.293l6-6c`|Same as above
`M14.414 5c-.781`|Same as above
`M14 11v-5c-.351`|Unread Email
`M14 5c1.105 0 2-.895`|Same as above
`M21.5 10c1.381 0`|Same as above
`M6.777 16.002c-1.32`|Attachment Paperclip
`M14.5 1.28a1.04`|Compose ? New Note?
`M11.25 21.4a6.25`|Bold attachment paperclip
`M2.357 3.36A1.238 1.238`|Move folder
`M8 4.958L6.8 3H`|Move folder, dark
`M11.293 6.293l1.414`|Same as above
`"M7 18C7 19.1046`|Note?
`M11.2774 8.9856C11.6733`|RSS
`"M19 12C19 12.0568`|Cloud Sync Solid
`M15.2222 7C13.3064`|Cloud Sync Outline
`M10.2132 18H18`|Cloud Sync Error?
`M11 15C11 15.5523`|Cloud Options
`M11 6C11 5.44772`| **Downloads Panel**
`M5.01415 3.98943C4.43062`|Revert?
`"M7.60744 11.9857C6.6858`|Bolder Revert
`M13 6c-3.866`|Same as above
`M16 13l-6 5V8l6 5z`|Right pointing triangle - maybe play?
`M9 9v8h3v-8zM14`|Pause
`M8 5C6.34314 5`| **New Tab Enclosed Plus Sign**
`M13.1429 11H2`|??
`<rect x="2" y="10" width="12" height="4" rx="1" />\n  <rect x="9`|Same as above but solid
`width="15" height="4" rx="1"/>\n  <rect x="15" y="10" width="2"`|Same as above
`M12 14H9C8.44772 14 8 13.5523`| **Add Web Panel**
`M11.1464 3.06066C11`|Edit?
`M10.7929 2.70711L10`|Edit? Larger
`M16 6.12132L16.7071`|Same as above
`M2.35717 3.36075C2.13323`|Folder, outline
`M9.47244 10.3917C8.9108`|Search Magnifying Glass
`M8.75739 10.1716C7.96696`|Same as above
`M13.76 15.17a5`|Same as above
`M12.5 16L8.86568`|Downward triangle / carrot
`M10 13l3.634-3`|Left facing triangle / carrot
`M16 13l-3.634`|Right facing triangle / carrot
`M8 6C6.34315`|Maybe panels ??
`M8 6C6.34315 6 5`|Maybe panels ??
`M8 1c1.55 0 2.9.28`|Database Stack
`M8 2c1.72 0 3.2.3`|Same as above
`M14.6 3c-1.4-.6-3.3-1-5`|Variation on Above
`M7.4 1.7C7.8.7 6.4.2`|Coffee / Java?
`M7.4 1.7C7.8.7 6.4.2 5.6`|Coffee / Java Solid
`7.4c.1 1.4 2.1 2.3 3.1`|Same as above
`M3 3h10a1 1 0 011`|Pause
` 1H3a1 1 0 01-1-1z`|Same as Above
`003-3V9a3 3 0 00-3-3zM6`|Same as Above
`012 2v8a2 2 0 01-2 2H3a2`|Play
`7V5.7A.8.8 0 017.3 5z`|Play, Solid
`1v8a1 1 0 01-1 1H7a1`|Play, solid
`M8.992 3.823l-.208-1.823h-1.571l`|Settings Gear
`M9.711 1.296l.207 1.817c.491.193.945.458`|Settings Gear, Solid
`.197-.195.296-.293.296h-2.684c`|Settings Gear
`M8.581 3h-3.581l-.117.007`|Flag
`"M4 3h4.697l.02.002c`|Flag
`M8 7h5.697l.02.002`|Flag
`M4 3h3.586l5 5L8`|Tag, Outline
`M7.146 1.146A.5.5`|Tag, Solid
`M12.004 5.146A.5.5 0`|Tag, Solid
`M21 8C21 6.89543 20.1046`|**Window Panel Icon**
`M23 8C23 6.89543 22.1046 6 21`|Image Cache ?
`M10.5 6C9.11929 6 8 7.11929 8 8.5C8`|Capture Page
`M7 6C5.34315 6 4`|Tile Tabs
`M7 6C5.89543 6 5 6.89543`|Tile Tabs
`M9.79289 5.79285L4.08578`|Page Actions
`<circle cx="8" cy="8" r="3" />\n</svg>`|Circle / Dot
`M10.5 2C10.2239 2 10`|Trash
`M10.5 2C10.2239 2 10`|Trash
`M15.5 7c-.276 0-.5-.224-.5`|Trash
`M3 4c-.552 0-1 .448-1`|Block Video
`M5.854 2.354c.195-.195`|?? Permission
`M9.271 2.787c.999-1.078`|Flash Permission Icon
`"M8 2.5c-1.933 0-3.5`|Location Permission
`M3.021 5.5c.276 0 .5.224.5.5 0`|Microphone Permission
`M4 2c-.552 0-1 .448-1`|Audio Input Permission
`M8 4.099v-.002l-.767.184`|Notification Permission
`M4 10.5c0 .276-.224`|?? Permission
`M13.3 2c.126 0 .241.047`|?? Permission
