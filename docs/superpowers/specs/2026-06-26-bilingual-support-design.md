# Bilingual (DE/EN) Language Toggle for aws1.html

## Overview
Add a DE/EN language toggle button to the aws1.html learning application. All UI text, course content, and flashcards are translated between German and English.

## Mechanism
- `translations` JS object mapping `data-i18n` keys → `{ de: "...", en: "..." }`
- Static HTML elements marked with `data-i18n="key"` attributes
- Dynamic content (`courseData`, `flashcards`) restructured to carry both languages
- `<html lang="...">` updated dynamically for accessibility

## Language Detection Priority
1. `?lang=en` or `?lang=de` URL parameter
2. `localStorage.getItem('lang')`
3. Default: `'de'`

## Components

### Toggle Button
- Simple `DE` / `EN` text button inside `#header-action-container`, right next to the "Abgeschlossen" checkbox
- Clicking toggles language, updates all text, saves to localStorage
- Active language is visually distinct

### Translation Function: `translatePage(lang)`
- Updates `<html lang="...">`
- Iterates all `[data-i18n]` elements and replaces text content from `translations[key][lang]`
- Re-renders sidebar labels (weeks, "Gesamtfortschritt", "Inhalte", "Karteikarten", search placeholder)
- Re-renders current topic content using the English `courseData` if available
- Re-renders flashcard content using the English flashcards if available
- Updates header subtitle, checkbox label, and any other static text
- Updates flashcard view headings and button labels

### Translation Data Structure

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
  "flashcards.erklärung": { de: "Erklärung", en: "Explanation" },
  "flashcards.kategorie": { de: "Kategorie", en: "Category" },
  // ... etc
};
```

### Course Data & Flashcards
Each topic and flashcard gets a parallel English version:

```js
const courseData = [
  {
    week: 1,
    title: { de: "Woche 1", en: "Week 1" },
    topics: [
      {
        id: "1.1",
        title: { de: "Das AWS-Grundmodell verstehen", en: "Understanding the AWS Basic Model" },
        content: { de: "...", en: "..." }
      },
      // ...
    ]
  },
  // ...
];
```

```js
const flashcards = [
  {
    category: { de: "AWS-Modell", en: "AWS Model" },
    question: { de: "...", en: "..." },
    answer: { de: "...", en: "..." },
    explanation: { de: "...", en: "..." },
    codeSnippet: "# Example"
  },
  // ...
];
```

## Files Changed
- `aws1.html` — only file, all changes in-place

## Implementation Order
1. Restructure `courseData` to hold both languages
2. Restructure `flashcards` to hold both languages
3. Build `translations` object for all static UI strings
4. Add `data-i18n` attributes to all static HTML elements
5. Write `translatePage(lang)` function
6. Wire language detection (URL param → localStorage → default)
7. Add DE/EN toggle button to header
8. Update all rendering functions to use current language
9. Test full toggle cycle
