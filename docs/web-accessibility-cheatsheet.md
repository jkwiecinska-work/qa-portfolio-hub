# Web Accessibility (A11y) Testing Cheatsheet

> My personal testing cheatsheet and notes for auditing **web applications** against WCAG 2.1 AA, verifying keyboard usability, and generating automated accessibility reports.

Web accessibility (A11y) ensures that digital products are usable by everyone, including people with visual, auditory, motor, or cognitive impairments (e.g., users relying on screen readers or keyboard-only navigation). 

---

## 1. Glossary: Accessibility Buzzwords Explained Simply

If you're new to accessibility, the jargon can feel overwhelming. Here is a simple, no-nonsense breakdown of terms used in this guide:

* **WCAG (Web Content Accessibility Guidelines):** The official global "rulebook" for accessibility, created by the W3C. It is split into levels: **A** (basic), **AA** (industry standard—what most laws require), and **AAA** (strict/advanced).
* **A11y:** A common abbreviation for *Accessibility*. Why? Because there are 11 letters between the 'A' and the 'y'!
* **Screen Reader:** Software used by blind or visually impaired people that reads webpage content out loud (e.g., NVDA on Windows, VoiceOver on Mac/iOS, TalkBack on Android).
* **Semantic HTML:** Using HTML tags for their *intended purpose*. For example, using `<button>` for something you click, instead of a `<div onclick="...">`. Think of it like using the right tool for the job.
* **ARIA (`aria-*` attributes):** Short for *Accessible Rich Internet Applications*. Think of ARIA tags like **sticky notes** you attach to HTML code to tell screen readers what a custom element is doing when regular HTML can't explain it (e.g., `aria-expanded="true"` tells a screen reader a menu is currently open).
* **Focus Ring / `:focus-visible`:** The blue or colored highlight outline that appears around a button or link when you navigate using the `Tab` key on your keyboard. It tells the user *"You are currently here"*.
* **Focus Trap:** A bug where a keyboard user gets "stuck" inside a pop-up window or modal and cannot `Tab` out of it to return to the main page.
* **Landmarks:** Major structural regions of a webpage (like `<header>`, `<nav>`, `<main>`, `<footer>`). Screen reader users use landmarks like **chapters in a book** to jump straight to the content they want.
* **Alt Text (`alt` attribute):** A written description of an image. If the image fails to load or if a blind user visits the page, the screen reader reads this text instead.
* **DOM (Document Object Model):** The invisible "tree structure" of a webpage created by the browser. Automated audit tools and screen readers scan the DOM tree rather than looking at the visual screen.

---

## 2. Core Checklist (WCAG 2.1 AA Essentials)

These are the core accessibility checks to keep an eye on. Most of them can be scanned automatically with the audit tools mentioned at the bottom of this guide.

### Semantics & Structure
- [ ] **Landmarks:** Page uses semantic containers (`<header>`, `<nav>`, `<main>`, `<footer>`, `<aside>`).
- [ ] **Heading Hierarchy:** Single `<h1>` per page. Sequential heading levels (`<h1>` -> `<h2>` -> `<h3>`) without skipping levels.
- [ ] **Semantic Controls:** Native `<button>` for actions and `<a>` for navigation (avoid `<div onclick>`).
- [ ] **ARIA Usage:** Use native HTML first; apply `aria-*` attributes only when native HTML is insufficient.

### Keyboard & Focus Management
- [ ] **Full Reachability:** All interactive elements can be reached and activated using `Tab` / `Shift+Tab` / `Enter` / `Space` / Arrow keys.
- [ ] **Visible Focus Indicator:** Clear, high-contrast focus ring visible during keyboard navigation (`:focus-visible`).
- [ ] **No Keyboard Traps:** Keyboard focus can enter and exit all modals, dropdowns, and complex components freely.
- [ ] **Skip Link:** A "Skip to main content" link is available as the first focusable element on the page.

### Color & Contrast
- [ ] **Text Contrast Ratio:** Minimum **4.5:1** for standard text and **3.0:1** for large text (≥ 18pt or 14pt bold).
- [ ] **UI Element Contrast Ratio:** Minimum **3.0:1** for input borders, focus rings, and functional icons.
- [ ] **Color Independence:** Statuses, errors, and active states do not rely solely on color (use text, icons, or patterns).

### Media & Forms
- [ ] **Alt Text:** Descriptive `alt="..."` for informative images; empty `alt=""` for decorative graphics.
- [ ] **Form Labels:** Every `<input>`, `<select>`, and `<textarea>` is explicitly paired with a `<label for="...">`.
- [ ] **Error Messages:** Programmatically linked to inputs via `aria-describedby` with actionable fix instructions.

---

## 3. Keyboard & Screen Reader Cheat Sheet

### Standard Keyboard Navigation
| Action | Shortcut |
| :--- | :--- |
| Move focus forward | `Tab` |
| Move focus backward | `Shift + Tab` |
| Activate link or button | `Enter` or `Space` (for buttons) |
| Close modal / dropdown | `Escape` |
| Navigate options / scroll | `Up Arrow` / `Down Arrow` / `Left Arrow` / `Right Arrow` |
| Toggle checkbox / radio button | `Space` |

### NVDA (Windows Screen Reader)
| Action | Shortcut |
| :--- | :--- |
| Mute / Resume Speech | `Control` |
| Read next / previous item | `Down Arrow` / `Up Arrow` |
| Jump to next Heading | `H` (or `Shift + H` for previous) |
| Jump to next Landmark | `D` (or `Shift + D` for previous) |
| Jump to next Form Field | `F` (or `Shift + F` for previous) |
| Jump to next Button | `B` |
| Jump to next Link | `K` |
| Elements List (Headings, Links, Landmarks) | `Insert + F7` (or `Caps Lock + F7`) |

### VoiceOver (macOS Screen Reader)
| Action | Shortcut |
| :--- | :--- |
| Enable / Disable VoiceOver | `Cmd + F5` (or `Cmd + Touch ID`) |
| VoiceOver Modifier Key (VO) | `Control + Option` |
| Mute / Pause Speech | `Control` |
| Read next / previous item | `VO + Right Arrow` / `VO + Left Arrow` |
| Open Web Rotor (Quick element navigation) | `VO + U` (Use `Left`/`Right` arrows to switch categories) |
| Read continuously from selection | `VO + A` |

---

## 4. Automated Audit Tools & Report Generation

### 1. Chrome Lighthouse (Built-in Browser Audit)
* **Description:** Google’s automated web auditing engine built directly into Google Chrome.
* **Capabilities:** Scans the DOM for accessibility violations against WCAG guidelines, assigns an overall numerical score (0–100), and provides actionable fix guidelines.
* **How to Generate a Report:**
  1. Open the target webpage in Chrome.
  2. Open Chrome DevTools (`F12` or `Right Click -> Inspect`).
  3. Select the **Lighthouse** tab.
  4. Under **Categories**, check **Accessibility**. Choose **Desktop** or **Mobile**.
  5. Click **Analyze page load**.
  6. Click the top-right `⋮` menu in the audit result to **Save as HTML** or **Save as JSON**.
* **Expected Output:** An interactive HTML report featuring an overall Accessibility score, list of failed audits, impacted CSS selectors, and links to Google web.dev guides.

---

### 2. Axe DevTools (Browser Extension)
* **Description:** Industry-standard accessibility testing engine developed by Deque Systems.
* **Capabilities:** Highly reliable scanner with zero false-positives. Tests dynamic page states, modals, popups, and shadow DOM.
* **How to Generate a Report:**
  1. Install the [axe DevTools Browser Extension](https://www.deque.com/axe/devtools/) (Chrome/Firefox/Edge).
  2. Open your web app and open DevTools (`F12`).
  3. Switch to the **axe DevTools** tab.
  4. Click **Scan ALL of my page** (or scan specific component containers).
  5. Review identified issues categorized by severity (*Critical*, *Serious*, *Moderate*, *Minor*).
* **Expected Output:** A structured breakdown of violations identifying exact DOM elements, specific WCAG criteria failures, and precise code remediation recommendations.

---

### 3. Pa11y CLI (Command-Line Automated Reports)
* **Description:** An open-source CLI tool for automated accessibility testing, ideal for local developer workflows and CI/CD pipelines.
* **Capabilities:** Runs headless browser audits from the terminal and exports formatted reports (HTML, CSV, JSON).
* **How to Generate a Report:**
  1. Ensure [Node.js](https://nodejs.org/) is installed.
  2. Run via `npx` (no global installation required):
     ```bash
     # Generate HTML Report
     npx pa11y https://your-website.com --reporter html > docs/a11y-report.html

     # Generate JSON Report
     npx pa11y https://your-website.com --reporter json > docs/a11y-report.json
     ```
* **Expected Output:** A standalone HTML or JSON report documenting errors, warnings, notices, CSS selectors, and corresponding WCAG guideline numbers.

---

### 4. WAVE (Web Accessibility Evaluation Tool)
* **Description:** Visual evaluation extension by WebAIM.
* **Capabilities:** Overlays visual icons directly onto the webpage to highlight structural headings, ARIA landmarks, contrast errors, and missing alt text in real-time.
* **How to Generate a Report:**
  1. Install the WAVE extension / navigate to [wave.webaim.org](https://wave.webaim.org/).
  2. Click the WAVE icon in your browser toolbar on the target page.
  3. Review the left sidebar summary showing **Errors**, **Contrast Errors**, **Alerts**, and **Features**.
  4. Click **Print/Export** in the WAVE sidebar to generate a PDF or printable report.
* **Expected Output:** A visually annotated web page with color-coded overlays indicating accessible vs. inaccessible elements, paired with a print-ready summary report.
