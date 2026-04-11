# Make Minnesota Site Redesign & Deployment

## Overview

Redesign makeminnesota.org from its current unstyled skeleton into a clean, minimal directory site and deploy it to GitHub Pages. The site is a general resource for anyone interested in Minnesota's maker community, with a primary job of helping people find the right resource for them.

## Design Decisions

- **Minimalist visual style** inspired by the maker community
- **Color palette** drawn from the new Minnesota state flag: dark navy (#002855), North Star blue (#4B9CD3), white, light gray (#fafbfc)
- **No frameworks or build tools** — plain HTML, CSS, Liquid, and vanilla JS
- **Single-page layout** with hero header, category filter pills, and a responsive card grid

## Data Model

Standardize `_data/makers.yaml` so every entry uses the same fields. Current data has inconsistent fields (`membership` vs `access`, `locations` vs `location`, nested `facilities`/`examples` structures). All entries will conform to this schema:

```yaml
- name: "Twin Cities Maker"
  category: "makerspace"          # one of: makerspace, education, program, organization
  city: "Minneapolis"             # single string, even for multi-location entries
  description: "A nonprofit..."
  features:
    - "24/7 access for members"
    - "Classes (Arduino, welding, CAD)"
  location: "3119 E. 26th St., Minneapolis, MN"
  website: "https://tcmaker.org"
  access: "~$55/month for full 24/7 access"
  social:                         # optional, omit if none
    facebook: "https://facebook.com/tcmaker"
    instagram: "https://instagram.com/tcmaker"
```

### Field definitions

| Field       | Required | Description                                                                 |
|-------------|----------|-----------------------------------------------------------------------------|
| name        | yes      | Display name of the resource                                                |
| category    | yes      | One of: `makerspace`, `education`, `program`, `organization`                |
| city        | yes      | City or area (e.g. "Minneapolis & St. Paul", "Statewide")                   |
| description | yes      | 1-2 sentence description                                                   |
| features    | yes      | List of key features/offerings                                              |
| location    | yes      | Street address or general location description                              |
| website     | yes      | URL                                                                         |
| access      | yes      | How to access: cost, membership, restrictions (replaces `membership` field) |
| social      | no       | Map of platform name to URL (e.g. `facebook`, `instagram`, `youtube`)       |

### Migration from current data

- Rename `membership` to `access` on entries that use it
- Entries that currently use `access` keep it as-is
- Flatten nested structures (`facilities`, `examples`, `locations`, `cities`) into the standard fields
- Fold `note` field content into `description` where useful, then drop `note`
- Add `category` to every entry based on its type
- Ensure `city` is always a single string
- Ensure `location` is always a single string

### Categories

| Category     | What it includes                                                        |
|--------------|-------------------------------------------------------------------------|
| makerspace   | Dedicated makerspaces, hackerspaces, tool libraries, fab labs           |
| education    | University labs, K-12 fab labs, school maker programs                   |
| program      | Meetups, events, community education programs, youth programs           |
| organization | State initiatives, nonprofits supporting makers (Launch MN, Enterprise MN) |

## Page Structure

### Hero

- Dark navy gradient background (`#002855` to `#003d7a`)
- North Star Gear SVG logo mark (the MN flag's North Star rendered as a gear/cog shape, in white and light blue)
- "Make Minnesota" title in large white text
- Tagline: "Find your shop. Learn a craft. Build something real."
- No "Discover" label

### Category Filter Bar

- Horizontal row of pill-shaped buttons below the hero
- Light gray background bar (#f5f7fa) with bottom border
- Pills: "All" (default active, filled navy), "Makerspaces", "Education", "Programs", "Organizations"
- Active pill: filled navy with white text
- Inactive pills: white background, navy text, light border
- Clicking a pill filters the card grid to show only matching `data-category` entries
- Implemented with ~15 lines of vanilla JS toggling `display:none` on non-matching cards

### Card Grid

- Responsive CSS grid: 2 columns on desktop, 1 column on mobile
- Light gray page background (#fafbfc)
- Each card is white with a light border and rounded corners
- Card contents top to bottom:
  - Category tag (light blue, uppercase, small)
  - Name (navy, bold)
  - City (gray)
  - Description (dark gray, 1-2 lines)
  - Feature pills (light blue-gray background, small rounded tags)
  - Access/cost info (navy, medium weight)
  - Bottom row: "Visit website" link (light blue) + social media icons (linking to URLs from `social` field)

### Footer

- Dark navy background matching the hero
- Copyright: "makeminnesota.org (c) 2025 Kyle Boon" with CC BY 4.0 link
- Email contact link

## File Changes

| File                              | Action | Description                                     |
|-----------------------------------|--------|-------------------------------------------------|
| `_data/makers.yaml`              | Edit   | Standardize all entries to new schema           |
| `index.html`                      | Edit   | New hero, filter bar, card grid layout          |
| `assets/css/main.css`            | Edit   | Complete restyle with MN flag palette           |
| `_layouts/base.html`             | Edit   | Minor updates if needed                         |
| `_includes/head.html`            | Edit   | Update meta description if needed               |
| `_includes/footer.html`          | Edit   | Restyle to navy bar                             |
| `assets/images/logo.svg`         | Create | North Star Gear SVG logo                        |
| `CNAME`                          | Create | Contains `makeminnesota.org`                    |
| `.github/workflows/jekyll.yml`   | Create | GitHub Actions workflow for build & deploy      |

## Deployment

- GitHub Actions workflow triggers on push to `main`
- Uses `actions/jekyll-build-pages` and `actions/deploy-pages` (GitHub's official Jekyll actions)
- `CNAME` file with `makeminnesota.org` for custom domain
- DNS configuration (done by site owner outside this repo):
  - Either CNAME record pointing to `<username>.github.io`
  - Or A records pointing to GitHub Pages IPs

## Out of Scope

- Search functionality
- Individual detail pages per entry
- Map view
- Dark mode
- Contact form or submission flow
- CMS or admin interface
- Analytics
