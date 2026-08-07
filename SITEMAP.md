# Site Map and Code Reference

This file is a maintenance guide for the Hugo site. It is intentionally kept
local and ignored by Git so it can be used as a personal reference without
being published in the repository.

## Site Entry Points

| Purpose | File |
| --- | --- |
| Hugo configuration | `hugo.toml` |
| Main language content | `content/en/`, `content/pt/`, `content/es/` |
| Main homepage template | `layouts/index.html` |
| Real-estate templates | `layouts/real-estate/` |
| Shared homepage shell | `layouts/_default/baseof.html` |
| Shared homepage styles | `assets/scss/main.scss` |
| Structured content | `data/` |
| Translations | `i18n/` |
| Public static files | `static/` |
| Deployment workflow | `.github/workflows/deploy.yml` |

## Hugo Configuration

Edit `hugo.toml` for:

- Site title, base URL and default language.
- Language definitions, language names and content directories.
- Taxonomy names.
- Markdown and syntax highlighting behavior.
- Output formats.
- Site parameters such as author, ORCID, LinkedIn and contact metadata.

## Content by Language

The same content structure exists in each language directory:

- `content/en/_index.md`
- `content/pt/_index.md`
- `content/es/_index.md`

These files contain homepage front matter and localized biography fields.

Real-estate content is located at:

- `content/en/real-estate/`
- `content/pt/real-estate/`
- `content/es/real-estate/`

Each real-estate directory contains:

- `_index.md`: real-estate portfolio section front matter.
- `buritis-v.md`: Buritis V property page and `property_id`.
- `europa-iii.md`: Jardim Europa III property page and `property_id`.

## Homepage Layouts

### Document shell

`layouts/_default/baseof.html` controls the homepage document structure and
client-side navigation logic:

- Loads shared head metadata and compiled SCSS.
- Includes the sidebar, mobile topbar, header and footer.
- Defines `toggleSidebar`, `setActiveSection`, `scrollToSection` and language switching.
- Tracks the active section while the internal scroll container moves.

### Shared partials

- `layouts/partials/head.html`: metadata, fonts, SCSS compilation and automatic language detection at the root URL.
- `layouts/partials/header.html`: desktop breadcrumb plus CV, ORCID and LinkedIn actions.
- `layouts/partials/sidebar.html`: desktop sidebar, section navigation and desktop language switcher.
- `layouts/partials/mobile-topbar.html`: mobile section buttons, breadcrumb and language dropdown.
- `layouts/partials/footer.html`: homepage copyright footer.

### Homepage sections

All sections are rendered in `layouts/index.html`:

| Section | HTML id | Main data source |
| --- | --- | --- |
| Biography and credentials | `about` | `data/profile.yml` and site params |
| Professional metrics | `about` | `data/impact.yml` |
| Education | `education` | `data/education.yml` |
| Research experience | `research-experience` | `data/experience.yml` |
| Professional leadership | `professional-leadership` | `data/experience.yml` |
| Teaching and academic service | `teaching-academic-service` | `data/experience.yml` |
| Competencies | `competencies` | `data/skills.yml` |
| Research | `research` | `data/research.yml` |
| Publications | `publications` | `data/publications.yml` |
| Portfolio | `portfolio` | Portfolio link to real-estate pages |
| Contact | `contact` | Inline Google Forms integration |

To change text or records, edit the corresponding YAML file first. To change
the section structure or markup, edit `layouts/index.html`.

## Homepage Styling

`assets/scss/main.scss` contains the shared homepage styles:

- Beginning of the file: color variables, typography and global layout.
- Sidebar selectors: `aside.sidebar-black`, `.sidebar-menu`, `.menu-item` and language controls.
- Main shell: `main.main-viewport`, `.top-dash-bar` and `.scroll-container`.
- Academic content: `.academic-*`, `.institutional-box`, `.impact-*`, `.capability-*`, `.publication-*` and `.contact-*`.
- Portfolio cards: `.portfolio-*`.
- Mobile behavior: the `@media (max-width: 640px)` block containing `.mobile-topbar-*` and `.mobile-lang-*`.

The real-estate pages do not load this SCSS file. Their styles are currently
inline in `layouts/real-estate/baseof.html`.

## Real-Estate Layouts

`layouts/real-estate/baseof.html` contains the complete real-estate document:

- Inline fonts, colors, desktop sidebar and mobile topbar styles.
- Desktop sidebar expansion via `toggleSidebar`.
- Mobile language dropdown and responsive topbar sizing.
- Hero, cards, gallery/lightbox, map, footer and tenant form styling.
- Lightbox JavaScript for property galleries.
- `switchRealEstateLang` for language changes while preserving the current property path.

### Portfolio list

`layouts/real-estate/list.html` contains:

- Hero banner: lines near 4-9.
- About section: lines near 11-24.
- Property cards: lines near 26-58.
- Contact and tenant CTA: lines near 60-86.

### Individual property

`layouts/real-estate/single.html` contains:

- Property hero and availability status: lines near 12-26.
- Features: lines near 28-46.
- Rental price and bonus: lines near 48-54.
- Contract terms: lines near 56-63.
- Photo gallery/lightbox triggers: lines near 65-86.
- Location map: lines near 88-103.
- Contact and tenant CTA: lines near 105-131.

Property text, availability, photos, prices, maps and features are stored in
`data/real_estate.yml`. The `property_id` in each content file selects the
matching record.

## Data Files

| File | Used for |
| --- | --- |
| `data/profile.yml` | Status, research focus and language lists |
| `data/impact.yml` | Professional metrics |
| `data/education.yml` | Education records |
| `data/experience.yml` | Research, leadership and teaching records |
| `data/skills.yml` | Competencies and language skills |
| `data/research.yml` | Research axes and descriptions |
| `data/publications.yml` | Publication records and BibTeX strings |
| `data/real_estate.yml` | Properties, prices, photos, features and maps |

## Translations

Edit the matching file in `i18n/`:

- `i18n/en.toml`
- `i18n/pt.toml`
- `i18n/es.toml`

Translation keys are used by `i18n` calls in templates. Menu labels,
section headings, real-estate text and form messages are all defined here.

## Static Files

- `static/cv/CV_EmilianoSilva.pdf`: linked from the homepage desktop header.
- `static/images/real-estate/`: property photos referenced by `data/real_estate.yml`.

Generated files should remain outside source control:

- `public/`: Hugo production output.
- `resources/_gen/`: Hugo asset cache.
- `.hugo_build.lock`: local Hugo build lock.

## Deployment

`.github/workflows/deploy.yml` runs on pushes to `main`:

1. Checks out the repository.
2. Installs Hugo Extended.
3. Builds with `hugo --minify`.
4. Uploads `public/` as a GitHub Pages artifact.
5. Deploys the artifact to GitHub Pages.

## Cleanup Candidates

The following items were found during the structural review:

- The `.re-*` style blocks in `assets/scss/main.scss` are unused legacy real-estate styles. Real-estate pages use the inline styles in `layouts/real-estate/baseof.html`.
- The tenant modal markup, `.tenant-modal-*` CSS and `openTenantFormModal` JavaScript in `layouts/real-estate/baseof.html` are currently unreachable; the visible CTA links directly to Google Forms.
- `.pricing-box` and `.bold-text` in `layouts/real-estate/baseof.html` are not used by current real-estate templates.
- Several i18n keys and site parameters are currently unused. Verify external links and future content plans before deleting them.
- Taxonomy pages are generated but currently empty because publications are stored in `data/publications.yml`, not as taxonomy-bearing content pages.

Default templates such as `layouts/_default/list.html` and
`layouts/_default/single.html` are valid Hugo fallbacks and should generally
be retained unless the project intentionally removes those fallback paths.
