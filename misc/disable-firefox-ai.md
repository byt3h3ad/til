# Taming Firefox’s New AI Features

I’ve found the new Firefox “AI” features - specifically those intrusive pop-ups that appear whenever I highlight text - to be far more distracting than useful. The sidebar chat isn't something I need integrated into the browser UI; if I want that experience, I’ll simply open ChatGPT in a containerised tab and be done with it.

### The "Nuke It" Approach

If you want to turn these features off entirely, here is the quick fix:

1. Open `about:config` in your address bar.
2. Search for `browser.ml.enable`.
3. Set it to **false**.

### The Granular Approach

If you’d rather test drive specific features while disabling the annoyances, keep `browser.ml.enable` set to **true** and toggle the individual settings listed below.

### Configuration Reference

Below is a list of the specific "AI" toggles found in `about:config`, along with my understanding of what each one controls.

| Setting | What it controls |
| :--- | :--- |
| `browser.ml.enable` | **The Master Switch.** Controls the built-in machine-learning platform. Setting this to false kills all AI features instantly. |
| `browser.ml.chat.enabled` | Enables the backend for the AI chatbot integration. |
| `browser.ml.chat.sidebar` | Toggles the AI chat panel appearing in the sidebar. |
| `browser.ml.chat.shortcuts` | Allows chat to be triggered via keyboard shortcuts or selected text. |
| `browser.ml.chat.page` | Powers the on-page chat menu context (the popup when you highlight text). |
| `browser.ml.chat.page.footerBadge` | Controls the floating badge display at the bottom of the page. |
| `browser.ml.chat.page.menuBadge` | Toggles the “Ask AI” badge inside the standard page menu. |
| `browser.ml.chat.menu` | Adds the “Chat about this” entry to your right-click context menus. |
| `browser.ml.linkPreview.enabled` | Generates AI-based previews and suggestions for links. |
| `extensions.ml.enabled` | Grants browser extensions permission to access Firefox’s ML API. |
| `browser.ml.pageAssist.enabled` | Runs the AI “page assist” summarizer and assistant tools. |
| `browser.tabs.groups.smart.enabled` | The feature I'm testing: uses AI to automatically group and label tabs. |
| `browser.tabs.groups.smart.userEnable` | Allows the user to manually toggle the smart tab groups feature on or off. |

[Source](https://flamedfury.com/posts/disable-ai-in-firefox/)

