# Build a Professional CNS Medical Lecture Reference Website

You are an expert full-stack web developer and UI/UX designer specializing in medical education platforms.

This repository already contains the final, organized lecture files and a metadata file. **Do not rename, move, or restructure the files in `/lectures/` — they are already correctly named and ordered.** Do not invent, rename, or guess lecture titles: use ONLY the titles/categories in `lectures-data.json` below.

## Actual Repository Contents (ground truth — use exactly this)

- `/lectures/` folder contains 29 self-contained HTML files, already renamed as `anatomy-01-...html` through `anatomy-14-...html` and `physio-01-...html` through `physio-15-...html`.
- `/lectures-data.json` contains the authoritative metadata for all 29 lectures: id, title, category, file path, description, tags. Load and use this file directly as the data source — do not hardcode a different list and do not re-derive titles from filenames.
- Categories are only two: "Anatomy" (14 lectures) and "Physiology" (15 lectures).
- Some lecture HTML files are large (a few MB, up to ~12MB for "EEG and Sleep") because they contain embedded/base64 images. Load lecture content via an iframe (`<iframe src="...">`) or direct navigation rather than injecting raw HTML into the DOM, and lazy-load — do not preload all 29 files on the homepage.

## 1. Website Identity

Homepage must prominently display:
- جامعة الجزيرة
- كلية الطب
- الدفعة 44
- Main Title: **CENTRAL NERVOUS SYSTEM**
- Subtitle: **Anatomy & Physiology**

Design: professional medical academic reference. Clean typography, excellent spacing, professional cards, subtle animations, responsive layout, light and dark mode, mobile-first, clear navigation, high readability.

## 2. Main Concept

The website is a lecture library/reference. It reads `lectures-data.json` and renders the lecture list dynamically — do not manually copy any lecture content into the homepage or any React component. The existing HTML files in `/lectures/` are the single source of truth for content.

When a user clicks a lecture card, open the corresponding HTML file (via iframe in the Lecture Viewer page). Never modify, rewrite, summarize, or "clean up" the medical content inside those HTML files.

## 3. Homepage

Elegant academic landing page, top to bottom:
1. جامعة الجزيرة / كلية الطب
2. الدفعة 44
3. Hero: **CENTRAL NERVOUS SYSTEM** / **Anatomy & Physiology**
4. Description: "A structured academic reference for Central Nervous System Anatomy and Physiology lectures — Batch 44, Faculty of Medicine, University of Gezira."
5. Primary button "Explore Lectures" → smooth scroll to library.

## 4. Lecture Library

Section titled "Lecture Library". Render all 29 lectures from `lectures-data.json` as cards, grouped/filterable by category (Anatomy / Physiology). Each card shows: id number, title, category badge, description, "View Lecture →" button, and an appropriate medical icon (e.g., from lucide-react — vary the icon by category/topic, not identical for all).

## 5. Data Source (already centralized — just consume it)

```ts
// src/data/lectures.ts
import lecturesData from './lectures-data.json'; // moved into src/data/

export interface Lecture {
  id: number;
  title: string;
  category: 'Anatomy' | 'Physiology';
  file: string; // relative path, e.g. "lectures/anatomy-01-introduction.html"
  description: string;
  tags: string[];
}

export const lectures: Lecture[] = lecturesData;
```

Adding a new lecture later = add one HTML file to `/public/lectures/` + one entry in `lectures-data.json`. No other code should need to change.

## 6. Opening Lecture Files

Route: `/lecture/:id` (numeric id from the JSON, e.g. `/lecture/8` for Cerebellum).

The Lecture Viewer loads the HTML file for that id inside an `<iframe>` sized to fill the reading area, preserving 100% of the original HTML/CSS/JS exactly as-is (images, tables, diagrams, headings, lists, formatting, internal links, equations, embedded content untouched).

## 7. Lecture Viewer Interface

Top navigation: "← Back to Lectures", then centered "Central Nervous System / Anatomy & Physiology".
Display the lecture title (from JSON) prominently below.
Controls: Previous Lecture / Lecture Library / Next Lecture (Previous/Next navigate by `id - 1` / `id + 1` within the full 29-lecture sequence, disabled at the boundaries). Dark/Light toggle. Reading progress indicator (based on iframe scroll if accessible, otherwise a simple top progress bar for the viewer page itself).

## 8. Navigation

Desktop navbar: HOME · LECTURES · ANATOMY · PHYSIOLOGY · ABOUT
Mobile: hamburger menu. Keep it simple and uncluttered.

## 9. Categories

Only two, exactly as in the JSON: **Anatomy** (14 lectures) and **Physiology** (15 lectures). Do not invent subcategories or additional lecture titles beyond what's in `lectures-data.json`.

## 10. Search

Live search over title, category, and tags fields from the JSON. Input placeholder "Search lectures...". Results filter instantly as the user types (e.g. typing "cerebellum" shows lecture #8).

## 11. Responsive Design

Must work well on phones (priority), tablets, laptops, desktop. Cards reflow automatically (1 col mobile → 2–3 col tablet → 3–4 col desktop).

## 12. Dark Mode

Toggle (☀ Light / 🌙 Dark), persisted via `localStorage`. Restrained medical-academic palette — no excessive color.

## 13. Visual Design

Premium, minimal, academic, medical, modern, professional. Avoid excessive gradients, unnecessary decoration, crowded layouts, huge empty spaces, childish design. Subtle animations only: card hover, fade-in, smooth scroll, button transitions.

## 14. Footer

```
جامعة الجزيرة
كلية الطب

Batch 44
Central Nervous System
Anatomy & Physiology

Academic Reference

© 2026 Batch 44
```
Do not claim official university endorsement.

## 15. Technical Stack

React + TypeScript + Vite. Tailwind CSS. lucide-react icons. React Router for `/` and `/lecture/:id`. Keep dependencies minimal.

## 16. GitHub Pages Compatibility

Must deploy correctly on GitHub Pages. Configure Vite `base` to match the repo name. Use relative paths throughout — do not hardcode `/`. Ensure the iframe `src` paths to `/lectures/*.html` resolve correctly with the configured base path. Test that images embedded inside the lecture HTML files (if any use relative paths, not base64) still resolve after deployment.

## 17. Existing HTML Files — Hard Rule

The 29 files in `/public/lectures/` are final and authoritative. Do not rewrite, summarize, replace, or convert their medical content in any way. Your job is only to build the surrounding interface.

## 18. File Organization

```
/
├── public/
│   └── lectures/            ← all 29 HTML files go here, unmodified
│       ├── anatomy-01-introduction.html
│       ├── ... (29 total)
├── src/
│   ├── components/
│   │   ├── Navbar.tsx
│   │   ├── LectureCard.tsx
│   │   ├── LectureGrid.tsx
│   │   ├── SearchBar.tsx
│   │   └── Footer.tsx
│   ├── pages/
│   │   ├── Home.tsx
│   │   └── LectureViewer.tsx
│   ├── data/
│   │   ├── lectures.ts
│   │   └── lectures-data.json
│   └── ...
├── package.json
└── README.md
```

## 19. Performance

Lazy-load iframe content (only load the HTML for the lecture currently being viewed, never all 29 at once). Minimize JS. Semantic HTML. Good accessibility.

## 20. Accessibility

Proper heading hierarchy, keyboard navigation, accessible buttons, proper contrast, alt text where applicable, ARIA labels, visible focus states.

## 21. Error Handling

If a lecture id doesn't exist in the JSON, show:
```
Lecture Not Found
The requested lecture could not be loaded.
← Back to Lecture Library
```
Never a blank page or raw error.

## 22. Final UX Checklist

User can: open homepage → see identity block → browse 29 lectures → search → filter by Anatomy/Physiology → click a lecture → read original HTML lecture in iframe → go prev/next → return to library → toggle dark/light mode. Test every one of the 29 lecture links, mobile layout, dark mode, search, prev/next boundaries at lecture 1 and 29, and GitHub Pages deployment before calling it done.

---

Created by Hassan Salaheldeen — Batch 44, Faculty of Medicine, University of Gezira.
