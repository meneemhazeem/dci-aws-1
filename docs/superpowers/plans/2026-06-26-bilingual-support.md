# Bilingual (DE/EN) Language Toggle Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Add German/English language toggle with `data-i18n` attributes, a translations JS object, URL param support, and localStorage persistence to aws1.html.

**Architecture:** A `translations` object maps `data-i18n` keys to `{ de, en }` for static UI. Dynamic content (`courseData`, `flashcards`) is restructured to carry both languages. A `translatePage(lang)` function updates `<html lang>`, walks `[data-i18n]` elements, and re-renders dynamic content. Language is detected from `?lang=` URL param → localStorage → default `'de'`.

**Tech Stack:** Vanilla JS, HTML5, CSS (all in single file)

**Files modified:** `aws1.html` (only file)

---

### Task 1: Restructure `courseData` to carry both languages

**Files:**
- Modify: `aws1.html` lines 500-3790

- [ ] **Step 1: Read the full courseData array**

Run: `read -offset 500 -limit 3290 aws1.html` to capture all 30 topic entries.

- [ ] **Step 2: Restructure each topic's `title` and `content` to bilingual objects**

For every topic in the `courseData` array, change:
```js
{
    id: "1.1",
    title: "Das AWS-Grundmodell verstehen",
    content: `<h3>Das AWS-Grundmodell verstehen</h3>...`
}
```
to:
```js
{
    id: "1.1",
    title: {
        de: "Das AWS-Grundmodell verstehen",
        en: "Understanding the AWS Basic Model"
    },
    content: {
        de: `<h3>Das AWS-Grundmodell verstehen</h3>...`,
        en: `<h3>Understanding the AWS Basic Model</h3>...`
    }
}
```

Also restructure each `week` entry's `title`:
```js
title: { de: "Woche 1", en: "Week 1" },
```

**Translation pairs needed (30 topics):**
- 1.1: "Das AWS-Grundmodell verstehen" / "Understanding the AWS Basic Model"
- 1.2: "AWS Identity and Access Management (IAM)" / "AWS Identity and Access Management (IAM)"
- 1.3: "Die AWS Management Console" / "The AWS Management Console"
- 1.4: "AWS Organizations" / "AWS Organizations"
- 1.5: "AWS Cloud Adoption Framework (AWS CAF)" / "AWS Cloud Adoption Framework (AWS CAF)"
- 2.1: "Amazon Elastic Compute Cloud (EC2)" / "Amazon Elastic Compute Cloud (EC2)"
- 2.2: "AWS Elastic Load Balancing (ELB)" / "AWS Elastic Load Balancing (ELB)"
- 2.3: "Amazon CloudWatch" / "Amazon CloudWatch"
- 2.4: "Amazon EC2 Auto Scaling" / "Amazon EC2 Auto Scaling"
- 2.5: "AWS Well-Architected Framework" / "AWS Well-Architected Framework"
- 3.1: "Amazon Simple Storage Service (S3)" / "Amazon Simple Storage Service (S3)"
- 3.2: "Amazon S3 – Speicherklassen" / "Amazon S3 – Storage Classes"
- 3.3: "Amazon S3 – Versioning, Replikation & Lifecycle" / "Amazon S3 – Versioning, Replication & Lifecycle"
- 3.4: "AWS Lambda" / "AWS Lambda"
- 3.5: "Amazon DynamoDB" / "Amazon DynamoDB"
- 4.1: "Amazon Virtual Private Cloud (VPC)" / "Amazon Virtual Private Cloud (VPC)"
- 4.2: "VPC-Sicherheitsgruppen & ACLs" / "VPC Security Groups & ACLs"
- 4.3: "Amazon Route 53" / "Amazon Route 53"
- 4.4: "Amazon CloudFront" / "Amazon CloudFront"
- 4.5: "AWS Direct Connect & VPN" / "AWS Direct Connect & VPN"
- 5.1: "AWS CloudFormation" / "AWS CloudFormation"
- 5.2: "AWS Elastic Beanstalk" / "AWS Elastic Beanstalk"
- 5.3: "AWS CloudTrail" / "AWS CloudTrail"
- 5.4: "AWS Key Management Service (KMS)" / "AWS Key Management Service (KMS)"
- 5.5: "AWS Kostenmanagement" / "AWS Cost Management"
- 6.1: "AWS Datenbanken im Vergleich" / "AWS Database Comparison"
- 6.2: "Amazon RDS" / "Amazon RDS"
- 6.3: "AWS Shared Responsibility Model" / "AWS Shared Responsibility Model"
- 6.4: "AWS Disaster Recovery" / "AWS Disaster Recovery"
- 6.5: "AWS Prüfungsvorbereitung & Klausur" / "AWS Exam Preparation & Final Test"

For each topic, generate an English version of the full HTML content. Keep all HTML structure, CSS classes, and code snippets identical — only translate visible text, headings, table headers, and labels. Use the same `bg-slate-...`, `text-...`, `border-...`, `rounded-...` etc. classes.

- [ ] **Step 3: Verify the restructured data**

Confirm the file still parses correctly with `node -e "require('fs').readFileSync('aws1.html','utf8'); console.log('OK')"` or similar.

- [ ] **Step 4: Commit**

```
git add aws1.html
git commit -m "feat: restructure courseData to hold DE/EN translations"
```

---

### Task 2: Restructure `flashcards` to carry both languages

**Files:**
- Modify: `aws1.html` lines 3792-4004
- Modify: `aws1.html` lines 4006-4037 (topicToFlashcardMap uses category strings)

- [ ] **Step 1: Restructure each flashcard**

Change each flashcard from:
```js
{
    category: "AWS-Modell",
    question: "Was beschreibt das AWS-Grundmodell?",
    answer: "AWS stellt Infrastruktur als Dienst bereit...",
    explanation: "AWS verwaltet Rechenzentren, Energie und Netzwerk...",
    codeSnippet: "# Beispiel"
}
```
to:
```js
{
    category: { de: "AWS-Modell", en: "AWS Model" },
    question: {
        de: "Was beschreibt das AWS-Grundmodell?",
        en: "What does the AWS basic model describe?"
    },
    answer: {
        de: "AWS stellt Infrastruktur als Dienst bereit...",
        en: "AWS provides infrastructure as a service..."
    },
    explanation: {
        de: "AWS verwaltet Rechenzentren, Energie und Netzwerk...",
        en: "AWS manages data centers, power, and network..."
    },
    codeSnippet: "# Beispiel"
}
```

**All 30 flashcard translation pairs needed:**
1. "AWS-Modell" / "AWS Model"
2. "Regionen & AZs" / "Regions & AZs"
3. "IAM-Basics" / "IAM Basics"
4. "Cross-Account" / "Cross-Account"
5. "Support-Pläne" / "Support Plans"
6. "EC2-Grundlagen" / "EC2 Basics"
7. "ELB & Metriken" / "ELB & Metrics"
8. "Auto Scaling" / "Auto Scaling"
9. "Well-Architected" / "Well-Architected"
10. "S3-Grundlagen" / "S3 Basics"
11. "S3-Speicherklassen" / "S3 Storage Classes"
12. "S3-Lifecycle" / "S3 Lifecycle"
13. "Lambda" / "Lambda"
14. "DynamoDB" / "DynamoDB"
15. "VPC & Subnetze" / "VPC & Subnets"
16. "Sicherheit in der VPC" / "VPC Security"
17. "Route 53" / "Route 53"
18. "CloudFront" / "CloudFront"
19. "Hybrid-Netzwerke" / "Hybrid Networks"
20. "CloudFormation" / "CloudFormation"
21. "Elastic Beanstalk" / "Elastic Beanstalk"
22. "CloudTrail" / "CloudTrail"
23. "KMS & Verschlüsselung" / "KMS & Encryption"
24. "Kostenoptimierung" / "Cost Optimization"
25. "Datenbanken" / "Databases"
26. "RDS" / "RDS"
27. "Shared Responsibility" / "Shared Responsibility"
28. "Disaster Recovery" / "Disaster Recovery"
29. "Monitoring & Observability" / "Monitoring & Observability"
30. "Prüfungsfokus" / "Exam Focus"

Translate all questions, answers, and explanations for each flashcard.

- [ ] **Step 2: Update `topicToFlashcardMap`**

The map currently references category strings like `"AWS-Modell"`. Update to use a shared key that works for both languages, or restructure to use the `category.de` value as the lookup key, and map it to an index.

Simplest approach: keep `topicToFlashcardMap` using German category keys (they won't change), and have flashcard lookup compare against `flashcards[i].category.de`.

- [ ] **Step 3: Commit**

```
git add aws1.html
git commit -m "feat: restructure flashcards to hold DE/EN translations"
```

---

### Task 3: Build `translations` object for all static UI strings

**Files:**
- Modify: `aws1.html` (insert new object before the existing JS code)

- [ ] **Step 1: Read the HTML structure to identify all translatable UI strings**

Search for all visible German text in the HTML body (lines 349-497) and in any JS-rendered UI strings in lines 4039-4233.

Strings to translate (with their `data-i18n` keys):

```js
const translations = {
    "sidebar.inhalte": { de: "Inhalte", en: "Content" },
    "sidebar.karteikarten": { de: "Karteikarten", en: "Flashcards" },
    "sidebar.suchen": { de: "Suchen...", en: "Search..." },
    "sidebar.gesamtfortschritt": { de: "Gesamtfortschritt", en: "Total Progress" },
    "header.subtitle": { de: "Lernen Sie Cloud-Infrastruktur strukturiert", en: "Learn Cloud Infrastructure Structured" },
    "header.abgeschlossen": { de: "Abgeschlossen", en: "Completed" },
    "flashcards.title": { de: "Interaktive Merksätze", en: "Interactive Memory Cards" },
    "flashcards.subtitle": { de: "Testen Sie Ihr Wissen durch aktives Lernen. Klicken Sie auf die Karte, um die Lösung zu enthüllen.", en: "Test your knowledge through active learning. Click the card to reveal the answer." },
    "flashcards.frage": { de: "Frage", en: "Question" },
    "flashcards.klicken": { de: "Klicken für Lösung", en: "Click for Answer" },
    "flashcards.zurueck": { de: "Zurück", en: "Back" },
    "flashcards.weiter": { de: "Weiter", en: "Next" },
    "flashcards.erklaerung": { de: "Erklärung", en: "Explanation" },
    "flashcards.kategorie": { de: "Kategorie", en: "Category" },
    "flashcards.antwort": { de: "Antwort", en: "Answer" },
    "toggle.de": { de: "DE", en: "DE" },
    "toggle.en": { de: "EN", en: "EN" },
};
```

- [ ] **Step 2: Insert the translations object**

Insert it right before the `// Core Course Data` comment at line 501.

- [ ] **Step 3: Commit**

```
git add aws1.html
git commit -m "feat: add UI translations object"
```

---

### Task 4: Add `data-i18n` attributes to static HTML elements

**Files:**
- Modify: `aws1.html` lines 349-497

- [ ] **Step 1: Read the HTML body section**

Read lines 349-497 to see all current HTML with hardcoded German text.

- [ ] **Step 2: Add `data-i18n` attributes to each translatable element**

For each element containing German text, add `data-i18n="key"`:

Line 361: `<span data-i18n="sidebar.gesamtfortschritt">Gesamtfortschritt</span>`
Line 378: `<button data-i18n="sidebar.inhalte">Inhalte</button>`
Line 381: `<button data-i18n="sidebar.karteikarten">Karteikarten</button>`
Line 387: `<input ... placeholder="Suchen..." data-i18n-placeholder="sidebar.suchen">`
Line 405: `<p data-i18n="header.subtitle" ...>Lernen Sie Cloud-Infrastruktur strukturiert</p>`
Line 411: `<span data-i18n="header.abgeschlossen">Abgeschlossen</span>`
Line 423: `<h3 data-i18n="flashcards.title" ...>Interaktive Merksätze</h3>`
Line 424: `<p data-i18n="flashcards.subtitle" ...>Testen Sie Ihr Wissen...</p>`
Line 436: `<span data-i18n="flashcards.frage">Frage</span>`
Line 444: `<span data-i18n="flashcards.klicken">Klicken für Lösung</span>`
Line 470: `<button data-i18n="flashcards.zurueck">Zurück</button>`
Line 472: `<button data-i18n="flashcards.weiter">Weiter</button>`
Line 487: `<h4 data-i18n="flashcards.erklaerung">Erklärung</h4>`
Line 435: `<span data-i18n="flashcards.kategorie" id="fc-category-front">Kategorie</span>`
Line 466: `<span data-i18n="flashcards.antwort">Antwort</span>`

For the search input placeholder, use `data-i18n-placeholder` instead of `data-i18n` since it's a `placeholder` attribute, not text content.

Also add `data-i18n` to the week labels in `renderSidebar()` JS function (around line 4045+).

- [ ] **Step 3: Commit**

```
git add aws1.html
git commit -m "feat: add data-i18n attributes to static HTML"
```

---

### Task 5: Write `translatePage(lang)` function and language detection

**Files:**
- Modify: `aws1.html` (insert before courseData, or after the translations object)

- [ ] **Step 1: Add the language state variable and detection logic**

```js
// --- Language / i18n ---
let currentLang = 'de';

function detectLanguage() {
    const urlParams = new URLSearchParams(window.location.search);
    const urlLang = urlParams.get('lang');
    if (urlLang === 'en' || urlLang === 'de') return urlLang;
    const stored = localStorage.getItem('lang');
    if (stored === 'en' || stored === 'de') return stored;
    return 'de';
}
```

- [ ] **Step 2: Write the `translatePage(lang)` function**

```js
function translatePage(lang) {
    currentLang = lang;
    document.documentElement.lang = lang === 'en' ? 'en' : 'de';
    localStorage.setItem('lang', lang);

    // Update static elements with data-i18n
    document.querySelectorAll('[data-i18n]').forEach(el => {
        const key = el.dataset.i18n;
        if (translations[key] && translations[key][lang]) {
            el.textContent = translations[key][lang];
        }
    });

    // Update placeholder attributes
    document.querySelectorAll('[data-i18n-placeholder]').forEach(el => {
        const key = el.dataset.i18nPlaceholder;
        if (translations[key] && translations[key][lang]) {
            el.placeholder = translations[key][lang];
        }
    });

    // Re-render sidebar (week titles)
    renderSidebar();

    // Re-render current topic if loaded
    const currentId = document.getElementById('current-topic-title')?.dataset?.topicId;
    if (currentId) {
        loadTopic(currentId);
    }

    // Re-render flashcard if in flashcard view
    updateFlashcard(currentFlashcardIndex);

    // Update toggle button text
    const toggleBtn = document.getElementById('lang-toggle');
    if (toggleBtn) {
        toggleBtn.textContent = lang === 'de' ? 'EN' : 'DE';
    }
}
```

- [ ] **Step 3: Wire language detection at init**

In `initPortal()`, add at the top:
```js
currentLang = detectLanguage();
translatePage(currentLang);
```

But be careful: during initial load, `renderSidebar()` and `loadTopic()` already use `currentLang` (from Task 7), so calling `translatePage()` would re-render them unnecessarily. Instead, call a lightweight static-only update in `initPortal()`.

The adjusted flow in `initPortal()`:
```js
function initPortal() {
    currentLang = detectLanguage();
    document.documentElement.lang = currentLang === 'en' ? 'en' : 'de';
    renderSidebar();
    loadTopic('1.1');
    switchView('content');
    // Apply static translations after initial render
    document.querySelectorAll('[data-i18n]').forEach(el => {
        const key = el.dataset.i18n;
        if (translations[key] && translations[key][currentLang]) {
            el.textContent = translations[key][currentLang];
        }
    });
    document.querySelectorAll('[data-i18n-placeholder]').forEach(el => {
        const key = el.dataset.i18nPlaceholder;
        if (translations[key] && translations[key][currentLang]) {
            el.placeholder = translations[key][currentLang];
        }
    });
    const toggleBtn = document.getElementById('lang-toggle');
    if (toggleBtn) toggleBtn.textContent = currentLang === 'de' ? 'EN' : 'DE';
}
```

- [ ] **Step 4: Commit**

```
git add aws1.html
git commit -m "feat: add translatePage() and language detection"
```

---

### Task 6: Add DE/EN toggle button to header

**Files:**
- Modify: `aws1.html` line 408 (inside `#header-action-container`)

- [ ] **Step 1: Add the toggle button HTML**

Add before or after the "Abgeschlossen" label in `#header-action-container` (around line 408):

```html
<button id="lang-toggle" onclick="toggleLanguage()"
    class="bg-slate-700 hover:bg-slate-600 px-3 py-2.5 rounded-lg border border-slate-600 hover:border-slate-500 text-sm font-bold transition-all select-none text-white min-w-[48px]">
    EN
</button>
```

- [ ] **Step 2: Write the `toggleLanguage()` function**

```js
function toggleLanguage() {
    const newLang = currentLang === 'de' ? 'en' : 'de';
    translatePage(newLang);
}
```

- [ ] **Step 3: Commit**

```
git add aws1.html
git commit -m "feat: add DE/EN toggle button to header"
```

---

### Task 7: Update all rendering functions to use current language

**Files:**
- Modify: `aws1.html` (renderSidebar, loadTopic, flashcard functions)

- [ ] **Step 1: Update `renderSidebar()`**

Inside `renderSidebar()` where week titles are rendered, use:
```js
const weekTitle = week.title[currentLang] || week.title.de;
```
And where topic titles are rendered in the sidebar links:
```js
const topicTitle = topic.title[currentLang] || topic.title.de;
```

Also update "Gesamtfortschritt" static text — this will be handled by `data-i18n` attribute.

- [ ] **Step 2: Update `loadTopic(id)`**

When setting the topic title:
```js
document.getElementById('current-topic-title').innerHTML = `
    <span class="inline-block w-1 h-8 bg-gradient-to-b from-blue-500 to-blue-600 rounded-full"></span>
    ${topic.title[currentLang] || topic.title.de}
`;
```
When setting the topic content:
```js
document.getElementById('view-content').innerHTML = topic.content[currentLang] || topic.content.de;
```
Also set a data attribute for the title element:
```js
document.getElementById('current-topic-title').dataset.topicId = id;
```

- [ ] **Step 3: Update flashcard rendering functions**

In `updateFlashcard(index)`:
```js
const card = flashcards[index];
document.getElementById('fc-category-front').textContent = card.category[currentLang] || card.category.de;
document.getElementById('fc-question').textContent = card.question[currentLang] || card.question.de;
document.getElementById('fc-answer').textContent = card.answer[currentLang] || card.answer.de;
document.getElementById('fc-explanation').textContent = card.explanation[currentLang] || card.explanation.de;
```

- [ ] **Step 4: Update topic-to-flashcard mapping lookup**

In the flashcard navigation or load logic where `topicToFlashcardMap` is used, update any category comparison to use `card.category.de` as the lookup key (since the map stores German category names).

- [ ] **Step 5: Commit**

```
git add aws1.html
git commit -m "feat: update rendering functions for bilingual support"
```

---

### Task 8: Test full toggle cycle

**Files:**
- Modify: none (test only)

- [ ] **Step 1: Manual test — page load in German**

Open `aws1.html`. Verify:
- All text displays in German
- `<html lang="de">` is set
- Toggle button shows "EN"

- [ ] **Step 2: Manual test — toggle to English**

Click the "EN" button. Verify:
- All static UI text switches to English
- Current topic content switches to English
- Sidebar week/topic titles switch to English
- Flashcard content switches to English
- `<html lang="en">` is set
- Button now shows "DE"
- localStorage has `lang=en`

- [ ] **Step 3: Manual test — toggle back to German**

Click "DE". Verify everything switches back to German.

- [ ] **Step 4: Manual test — URL parameter**

Open `aws1.html?lang=en`. Verify English is shown immediately without toggling.
Open `aws1.html?lang=de`. Verify German is shown.
Open `aws1.html?lang=fr`. Verify falls back to German (default).

- [ ] **Step 5: Manual test — localStorage persistence**

After toggling to English, close and reopen the file (without URL param). Verify English is still shown.

- [ ] **Step 6: Manual test — all 30 topics and flashcards**

Toggle to English, click through all 30 topics. Verify content is in English. Enter flashcard view, verify flashcards are in English.

- [ ] **Step 7: Commit**

```
git add aws1.html
git commit -m "feat: complete bilingual DE/EN support"
```
