# SudutKode — Landing Page

Official landing page for **SudutKode**, an Indonesian software house focused on mobile and web application development.

Built on top of the [shadcn-vue-landing-page](https://github.com/leoMirandaa/shadcn-vue-landing-page) template — Vue 3 + Vite + Tailwind CSS + shadcn/ui.

---

## Tech Stack

| Layer         | Technology                |
| ------------- | ------------------------- |
| Framework     | Vue 3 (Composition API)   |
| Build Tool    | Vite                      |
| Styling       | Tailwind CSS              |
| UI Components | shadcn/ui (Vue port)      |
| Font          | Montserrat (Google Fonts) |
| Icons         | Lucide Vue                |

---

## Getting Started

```bash
# Clone repo
git clone https://github.com/your-org/sudut-kode-landing.git
cd sudut-kode-landing

# Install dependencies
bun run install

# Run dev server
bun run dev

# Build for production
bun run build

# Preview build result
bun run preview
```

---

## Project Structure

```
src/
├── App.vue                  # Root — determines section order
├── assets/
│   ├── index.css            # Global styles + import Montserrat
│   └── icon.ico             # SudutKode Favicon
├── components/
│   ├── Navbar.vue
│   ├── Hero.vue
│   ├── About.vue            # New component — not in original template
│   ├── Services.vue
│   ├── Products.vue         # New component — not in original template
│   ├── Team.vue
│   ├── Contact.vue
│   ├── Footer.vue
│   └── ui/                  # Base shadcn/ui components
├── icons/                   # Social media icons (GitHub, LinkedIn, etc.)
└── lib/
    └── utils.ts

public/
├── images/
│   ├── uangku/              # UangKu app screenshots
│   │   ├── screen-1.png
│   │   ├── screen-2.png
│   │   └── screen-3.png
│   └── team/                # Team member photos
│       └── [name].jpg
└── favicon.ico
```

---

## Design System Configuration

### Colors

The color system uses CSS variables via Tailwind. Currently **monochrome** (black and white). Brand accent colors will be added after final visual identity confirmation.

To change accent colors, update the following values in `tailwind.config.js`:

```js
accent: {
  DEFAULT: "hsl(...)", // ganti dengan warna brand
  foreground: "hsl(0 0% 96%)",
},
primary: {
  DEFAULT: "hsl(...)", // ganti dengan warna brand
  foreground: "hsl(0 0% 4%)",
},
```

### Font

Uses **Montserrat** from Google Fonts for all text including brand name.

---

## Featured Products

### UangKu

- Category: Mobile App — Personal Finance Tracker
- Platform: Android
- Status: Available
- Screenshots: `public/images/uangku/`

> The second product is in development and will be added to the Products section when ready.

---

## Deployment

Recommended to use **Vercel** or **Netlify**.

```bash
# Build
bun run build

# Output is in the dist/ folder
# Upload dist/ to Netlify, or push to GitHub and connect to Vercel
```

---

## Important Notes

- Dark mode is permanently active — the `dark` class is added to the `<html>` tag in `index.html`
- Unused template components (`Pricing`, `FAQ`, `Sponsors`, etc.) are not deleted from disk, just not imported in `App.vue`
- UangKu download link currently uses Google Drive. Final target: Google Play Store
