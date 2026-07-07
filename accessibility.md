# ARIA (Accessible Rich Internet Applications) roles and using CSS and semantic HTML

## Understanding ARIA and Its Importance
ARIA is a set of attributes you can add to your HTML to make web content more accessible to people with disabilities. It helps by providing additional context or meaning to the visually impaired who rely on screen readers to navigate the web. ARIA roles and properties describe the functions of web elements, their state, and their relationship to other elements.

## WCAG (Web Content Accessibility Guidelines)

### Largest Contentful Paint - LCP -  200ms or less

To Improve
- Optimize images by reducing their file size, using appropriate file types, compressing them, and using a content delivery network (CDN).
- Use lazy loading to load non-critical resources only when they are needed, reducing the time it takes for the largest image or text block to load.
- Use preloading and pre-fetching to load critical resources before they are needed.

### First Input Delay - FID - 100ms or less

To Improve
- Minimize the use of third-party scripts to reduce the time it takes for a website to become interactive.
- Reduce the size of JavaScript and CSS files to reduce download and parsing time.
- Use a Content Delivery Network (CDN) to improve server response time.
- Use code splitting to load only the necessary JavaScript code for the current page.

### Cumulative Layout Shift - CLS - less than 0.1

To improve
- Use a layout that doesn’t change frequently to prevent disorienting users and negative impacts on CLS.
- Avoid adding elements to the page without user interaction to prevent layout shifts and negative impacts on CLS.
- Use a CSS position property instead of a layout property to reduce layout shifts.
- Use a grid layout instead of a flexible box layout to reduce layout shifts.

Optimizing Input Delay, Processing Time, and Presentation Delay (collectively known as INP - Interaction to Next Paint)


**WCAG & ATAG**  
In WordPress **Web Content Accessibility Guidelines (WCAG) 2.2 at Level AA**, which is the latest international standard. It also encourages use of **Authoring Tool Accessibility Guidelines (ATAG) 2.0**. The latest WordPress version, **6.9 (released in late 2025)**, includes **33 accessibility enhancements** across the core, block editor, and admin dashboard.

---

## 🌍 Accessibility Standards in Web Development
- **WCAG (Web Content Accessibility Guidelines):**
  - **WCAG 2.0 (2008):** Introduced 61 success criteria across 4 principles (Perceivable, Operable, Understandable, Robust).
  - **WCAG 2.1 (2018):** Added 17 new criteria focusing on mobile accessibility, low vision, and cognitive disabilities.
  - **WCAG 2.2 (2023):** Latest version with **86 success criteria**, strengthening requirements for focus indicators, accessible authentication, and support for users with cognitive challenges.
- **ATAG 2.0 (2015):** Guidelines for authoring tools (like WordPress) to help creators produce accessible content (e.g., prompting for alt text, captions).

---

## 📊 Comparison of WCAG Versions

| Version | Year | Key Features | WordPress Adoption |
|---------|------|--------------|--------------------|
| **WCAG 2.0** | 2008 | Core accessibility principles; 61 criteria | Early WordPress compliance |
| **WCAG 2.1** | 2018 | Mobile, low vision, cognitive accessibility | Adopted in WordPress 5.x era |
| **WCAG 2.2** | 2023 | 86 criteria; focus indicators, accessible authentication, drag-and-drop improvements | Current WordPress standard (Level AA) |

---

## 🖥️ WordPress Accessibility Features (Latest: 6.9)
- **Core Enhancements (33 fixes):**
  - Improved **screen reader notifications**.
  - Better **focus management** and semantic HTML.
  - Updated CSS generated content to avoid redundant spoken text.
- **Block Editor (Gutenberg):**
  - Cleaner drafting with accessible blocks.
  - Reduced reliance on third-party plugins for accessibility.
- **Admin Dashboard:**
  - Inclusive language updates.
  - Higher contrast in AJAX-driven actions (e.g., deleting terms).
- **Themes & Customization:**
  - Bundled themes updated for semantic markup.
  - Accessibility-ready theme tags enforced.

---

## ⚠️ Risks & Compliance Notes
- **Legal Compliance:** WCAG 2.2 is now referenced in laws like the **European Accessibility Act** and ADA-related enforcement.
- **Business Risk:** Not upgrading to WordPress 6.9 or ignoring accessibility can lead to **lawsuits, lost customers, and poor SEO**.

---

## 📋 Broader WCAG 2.1 Structure
- **Perceivable:** Users must be able to perceive content (e.g., text alternatives, captions, contrast).
- **Operable:** Users must be able to operate interface (e.g., keyboard access, enough time, no seizures).
- **Understandable:** Content must be clear and predictable (e.g., readable text, consistent navigation).
- **Robust:** Content must work with current and future assistive technologies (e.g., valid code, ARIA roles).


## 🚀 New WCAG 2.2 Criteria (2023–2025)
- **Focus Appearance (2.4.11):** Clear visible focus indicators.  
- **Dragging Movements (2.5.7):** Alternatives for drag‑and‑drop.  
- **Accessible Authentication (3.3.8):** No cognitive tests (like puzzles) required for login.  
- **Target Size (2.5.8):** Clickable areas at least 24×24 px.  
- **Redundant Entry (3.3.7):** Don’t force users to re‑enter info.

---

## 📋 WCAG 2.1 Testing Checklist (Mapped to Common Issues)

| Area | Common Issue | WCAG 2.1 Success Criterion | What to Check |
|------|--------------|----------------------------|---------------|
| **Page Structure** | Headings styled visually but not coded (`<div>` instead of `<h1>`) | **1.3.1 Info and Relationships (Level A)** | Ensure headings, lists, tables use semantic HTML so screen readers announce structure. |
| **Images** | Missing or incorrect alt text | **1.1.1 Non-text Content (Level A)** | Every meaningful image has descriptive alt text; decorative images use empty alt (`alt=""`). |
| **Forms** | Input fields without labels | **1.3.1 Info and Relationships** + **4.1.2 Name, Role, Value (Level A)** | Each form control has a programmatically associated label; error messages are announced. |
| **Links & Buttons** | “Click here” or unlabeled icons | **2.4.4 Link Purpose (Level A)** | Links/buttons must have clear accessible names describing their purpose. |
| **Keyboard Navigation** | Elements not reachable by Tab | **2.1.1 Keyboard (Level A)** | All interactive elements must be operable via keyboard only. |
| **Color Contrast** | Text too light against background | **1.4.3 Contrast (Minimum, Level AA)** | Text must have at least 4.5:1 contrast ratio (3:1 for large text). |
| **Dynamic Content** | Updates not announced (e.g., error messages, popups) | **4.1.3 Status Messages (Level AA)** | Use ARIA live regions so screen readers announce changes. |
| **Tables** | No header associations | **1.3.1 Info and Relationships** | Table headers (`<th>`) must be associated with data cells (`<td>`). |
| **Navigation** | No skip link to bypass menus | **2.4.1 Bypass Blocks (Level A)** | Provide “Skip to content” links or landmarks for quick navigation. |
| **Consistency** | Different pages use inconsistent navigation | **3.2.3 Consistent Navigation (Level AA)** | Menus and navigation elements should appear in consistent order across pages. |

---

## ✅ How to Use This
- Document as: *Issue → Criterion → Impact → Suggested Fix*.  
- Example: *Form field missing label → Violates 4.1.2 → Screen reader users can’t identify field → Add `<label>` element.*

---


**Quick Answer:**  
In WCAG (Web Content Accessibility Guidelines), **Level A** is the *minimum* standard of accessibility, addressing the most basic barriers for users with disabilities. **Level AA** is the *recommended and widely adopted standard*, requiring stronger accessibility features that make content usable for a broader range of people.

---

## 🌐 WCAG Conformance Levels Explained

### 📊 Comparison Table

| Level | Meaning | Requirements | Practical Impact |
|-------|---------|--------------|------------------|
| **A** | Minimum accessibility | Covers the most basic success criteria (e.g., text alternatives for images, keyboard navigation) | Ensures content is accessible at a fundamental level but may still exclude many users |
| **AA** | Recommended standard | Includes all Level A + additional criteria (e.g., sufficient color contrast, resizable text, consistent navigation) | Provides accessibility for most users, including those with moderate visual, auditory, or cognitive disabilities |
| **AAA** | Highest standard | Includes all A + AA + advanced criteria (e.g., sign language interpretation, enhanced contrast ratios) | Ideal but often impractical for all content; rarely required by law |

Sources: 

---

## 🔑 Key Points

- **Level A (Basic):**  
  - Focuses on removing the most severe barriers.  
  - Example: Providing alt text for images so screen readers can describe them.  
  - Without Level A compliance, many users cannot access content at all.

- **Level AA (Intermediate, Recommended):**  
  - Builds on Level A with more robust requirements.  
  - Example: Ensuring color contrast ratio of at least 4.5:1 for text, so people with low vision can read it.  
  - Most legal frameworks (like ADA in the US, EN 301 549 in the EU) require **Level AA compliance**.

- **Level AAA (Advanced):**  
  - Goes beyond AA, but is not always practical for all websites.  
  - Example: Providing sign language interpretation for all prerecorded video content.  
  - Considered aspirational rather than mandatory.

---

## ⚠️ Risks & Considerations

- **Legal Compliance:** Most accessibility laws worldwide (ADA, Section 508, EAA) mandate **Level AA**, not just Level A. Stopping at Level A could expose organizations to lawsuits or fines.  
- **User Experience:** Level A compliance alone often leaves users with partial or frustrating access. Level AA ensures inclusivity for a much wider audience.  
- **Feasibility:** Level AAA is excellent for government or specialized sites but can be costly and difficult to implement universally.

---

👉 In short: **Level A = bare minimum, Level AA = the real-world standard you should aim for.**  