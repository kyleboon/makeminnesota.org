# Make Minnesota Redesign Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Redesign makeminnesota.org into a styled, functional directory site and deploy it to GitHub Pages.

**Architecture:** Single-page Jekyll site with a hero header, category filter pills, and responsive card grid. Data lives in `_data/makers.yaml` with a standardized schema. Vanilla JS handles category filtering. GitHub Actions deploys to GitHub Pages with a custom domain.

**Tech Stack:** Jekyll, Liquid, HTML, CSS (custom properties), vanilla JavaScript, GitHub Actions

---

### Task 1: Standardize the data model in `_data/makers.yaml`

**Files:**
- Modify: `_data/makers.yaml` (entire file rewrite)

This is the foundation — all other tasks depend on the data shape being correct.

- [ ] **Step 1: Rewrite `_data/makers.yaml` with standardized schema**

Replace the entire file with entries conforming to the schema. Every entry must have: `name`, `category`, `city`, `description`, `features` (list), `location`, `website`, `access`. Optional: `social` (map).

Categories to assign:
- `makerspace`: Twin Cities Maker, Nordeast Makers, MPLS MAKE, White Bear Makerspace, Duluth MakerSpace, Mankato Makerspace, Iron Range Makerspace, Minnesota Tool Library, Leonardo's Basement
- `education`: Century College Fab Lab, Saint Paul Public Library Innovation Lab, Minnesota Children's Museum The Studio, Apple Valley HS Fab Lab, Mahtomedi HS Fab Lab, U of M Makerspaces, U of St. Thomas create[space], SCSU Husky Make-It Space, Other Educational Maker Programs
- `program`: Maker Meetups and Events, Community Education Programs
- `organization`: Launch Minnesota, Enterprise Minnesota

Migration rules:
- Rename `membership` → `access`
- Keep existing `access` fields as-is
- For U of M: flatten `facilities` list into `features`, set `city` to `"Minneapolis & St. Paul"`, set `location` to `"Multiple campus locations (Walter Library, Mayo Building, Keller Hall, Rapson Hall)"`
- For Minnesota Tool Library: flatten `cities` to `city: "Minneapolis & St. Paul"`, flatten `locations` to `location` with both addresses joined
- For entries with `note`: fold useful content into `description`, drop `note`
- For entries with `examples` (Other Educational Maker Programs, Community Education Programs): flatten into `features` list
- Social fields: leave empty (omit `social`) for now — owner will populate later

```yaml
- name: "Twin Cities Maker"
  category: "makerspace"
  city: "Minneapolis"
  description: "A nonprofit, volunteer-driven makerspace operating a shared workshop ('The Hack Factory') with tools for woodworking, metalworking, machining, electronics, textiles and more."
  features:
    - "24/7 access for members"
    - "Classes (Arduino, welding, CAD)"
    - "Public open nights"
  location: "3119 E. 26th St., Minneapolis, MN"
  website: "https://tcmaker.org"
  access: "~$55/month for full 24/7 access (discounted open nights for non-members)"

- name: "Nordeast Makers"
  category: "makerspace"
  city: "Minneapolis"
  description: "A membership-based makerspace in Northeast Minneapolis focused on woodworking and fabrication."
  features:
    - "10,000 sq ft workshop"
    - "CNC router, laser cutter/engraver, CNC mill, 3D printers"
    - "Full wood shop"
    - "One-on-one training and knowledge sharing"
  location: "451 Taft St. NE, Suite 14, Minneapolis, MN 55413"
  website: "http://nordeastmakers.com"
  access: "~$200/month for round-the-clock access and training support"

- name: "MPLS MAKE"
  category: "makerspace"
  city: "Minneapolis"
  description: "A professional-grade community workshop in NE Minneapolis for wood and metal crafts."
  features:
    - "8,500 sq ft space"
    - "State-of-the-art woodworking and metalworking equipment"
    - "Dust collection and 20-ft ceilings"
    - "Culture geared toward artisan craftsmanship and small business incubation"
  location: "NE Minneapolis (Tours by appointment)"
  website: "https://mplsmake.com"
  access: "~$220/month for unlimited access to the shop's advanced tools"

- name: "White Bear Makerspace"
  category: "makerspace"
  city: "White Bear Lake"
  description: "A 10,000 sq ft 'woodworkers' gym' and community workshop opened in 2022 to serve the northeast metro area."
  features:
    - "Wood shop, laser cutter, 3D printer"
    - "Crafting studio for sewing, vinyl cutting, scrapbooking"
    - "Rentable studio/desk spaces"
    - "Staff on-site whenever open to assist and ensure safe tool use"
  location: "5966 Hwy 61 N, White Bear Lake, MN 55110"
  website: "https://whitebearmakerspace.com"
  access: "Memberships or day passes available (e.g. 5 day-use passes for $125)"

- name: "Duluth MakerSpace"
  category: "makerspace"
  city: "Duluth"
  description: "A nonprofit cooperative workshop in Duluth's Lincoln Park Craft District."
  features:
    - "11,000 sq ft space"
    - "Tech tools, electronics workstations, 3D printers, laser cutters"
    - "Full wood shop, metal shop, pottery studio"
    - "Welding and blacksmithing equipment"
  location: "3001 West Superior St., Duluth, MN 55806"
  website: "http://duluthmakerspace.com"
  access: "Monthly membership grants 24/7 access; free public tours offered"

- name: "Mankato Makerspace"
  category: "makerspace"
  city: "Mankato"
  description: "A nonprofit creative studio providing workspace, tools, and knowledge for makers, artists, hobbyists, and entrepreneurs in Southern Minnesota."
  features:
    - "6,000+ sq ft facility"
    - "Wood shop, metal shop, pottery and glass area"
    - "Textiles, 3D printing, and classroom space"
    - "Volunteer-run with classes and events open to all ages"
  location: "1700 3rd Ave., Mankato, MN 56001"
  website: "https://mankatomakerspace.org"
  access: "Monthly and 6-month memberships; members get full shop access and discounts on classes"

- name: "Iron Range Makerspace"
  category: "makerspace"
  city: "Hibbing"
  description: "A makerspace, co-working, and incubator facility serving Minnesota's Iron Range region."
  features:
    - "Metal working bay, automotive lift, wood shop"
    - "Textile area, recording studio, electronics lab"
    - "Commercial kitchen"
  location: "704 W. 41st Street, Hibbing, MN 55746"
  website: "https://www.facebook.com/Iron-Range-Makerspace-LLC"
  access: "Membership plans for individuals and businesses, granting access to the facility and equipment"

- name: "Minnesota Tool Library"
  category: "makerspace"
  city: "Minneapolis & St. Paul"
  description: "A cooperative, volunteer-driven nonprofit that promotes 'access over ownership' of tools."
  features:
    - "Inventory of over 8,000 tools for public borrowing"
    - "Workshop space on-site at both locations"
    - "Equipment like table saws, lathes, workbenches"
    - "DIY classes and Fix-It clinics"
  location: "NE Minneapolis (1620 Central Ave NE) and St. Paul North End (1010 Dale St. N)"
  website: "https://www.mntoollibrary.org"
  access: "Annual memberships on a sliding scale, giving unlimited tool checkouts and shop access during open hours"

- name: "Leonardo's Basement"
  category: "makerspace"
  city: "Minneapolis"
  description: "An innovative educational nonprofit (founded 1998) that many consider the 'oldest all-ages makerspace' in the world. Activities are structured as classes or sponsored sessions, not drop-in."
  features:
    - "Year-round inventive workshops and camps"
    - "Rich assortment of materials and tools for engineering, art, and technology"
    - "From welding and woodworking to coding and crafting"
    - "After-school classes, summer camps, family build events, school partnerships"
  location: "2 Malcolm Ave SE, Minneapolis, MN 55414"
  website: "https://leonardosbasement.org"
  access: "Registration required; activities are structured as classes, camps, or sponsored sessions"

- name: "Century College Fab Lab"
  category: "education"
  city: "White Bear Lake"
  description: "A 'hi-tech tinkerer's workshop' located on Century College's East Campus."
  features:
    - "Laser cutters, 3D printers, CNC machines, vinyl cutters"
    - "Computer-aided design and desktop manufacturing tools"
    - "Training through the college's Continuing Education programs"
  location: "3300 Century Ave N, White Bear Lake, MN 55110"
  website: "https://www.century.edu/academics/learning-opportunities/digital-fabrication-lab/"
  access: "Open to Century College students and to the public via workshops or special events"

- name: "Saint Paul Public Library – Innovation Lab"
  category: "education"
  city: "St. Paul"
  description: "A free public makerspace inside the George Latimer Central Library downtown."
  features:
    - "3D printing, vinyl cutting, laser engraving"
    - "Sewing, media conversion, and more"
    - "Software, supplies, and staff support"
  location: "90 W. 4th St., St. Paul, MN 55102"
  website: "https://sppl.org/innovation-lab/"
  access: "Free – library membership and orientation required for independent use (open to the public during staffed hours)"

- name: "Minnesota Children's Museum – The Studio"
  category: "education"
  city: "St. Paul"
  description: "A makerspace studio within the Children's Museum, aimed at encouraging hands-on creativity in kids and families."
  features:
    - "Tools and materials for woodworking, circuit building, tinkering and art"
    - "Sewing machines, simple circuits, and other supplies"
    - "Play-focused environment"
  location: "10 W 7th St., St. Paul, MN 55102"
  website: "https://mcm.org"
  access: "Included with museum admission; children must be accompanied by an adult"

- name: "Apple Valley High School Fab Lab"
  category: "education"
  city: "Apple Valley"
  description: "A fully equipped fabrication laboratory integrated into a public high school, opened in 2014 as part of a STEM pathway. One of the first high-school Fab Labs in Minnesota."
  features:
    - "3D printers, laser cutters, CNC machines"
    - "Follows the MIT Fab Lab model"
    - "Supports courses and clubs for prototyping and engineering"
  location: "14450 Hayes Rd., Apple Valley, MN 55124"
  website: "https://avhs.district196.org/academics/fab-lab"
  access: "Available to enrolled students through STEM courses and clubs"

- name: "Mahtomedi High School Fab Lab"
  category: "education"
  city: "Mahtomedi"
  description: "The first Fab Lab in a K-12 setting in Minnesota (opened 2011). Nationally recognized for bringing world-class engineering opportunities to students."
  features:
    - "State-of-the-art design/make center for students"
    - "Industry-grade equipment (laser cutters, CNC routers, etc.)"
    - "Curriculum from toy engineering in elementary to advanced manufacturing in high school"
  location: "8000 75th St N, Mahtomedi, MN 55115"
  website: "https://www.mahtomedi.k12.mn.us"
  access: "Available to enrolled students through curriculum programs"

- name: "University of Minnesota – Makerspaces"
  category: "education"
  city: "Minneapolis & St. Paul"
  description: "The U of M operates numerous makerspaces and fabrication labs across its campuses for student and faculty use."
  features:
    - "Anderson Student Innovation Labs – prototyping for CSE students"
    - "Bakken Medical Devices Center – prototyping lab open to all university innovators"
    - "Toaster Innovation Hub – library makerspace in Walter Library"
    - "3D printers, laser cutters, electronics benches, sewing machines"
  location: "Multiple campus locations (Walter Library, Mayo Building, Keller Hall, Rapson Hall)"
  website: "https://makers.umn.edu"
  access: "Generally restricted to U of M students, staff, and faculty (free with required training)"

- name: "University of St. Thomas – create[space]"
  category: "education"
  city: "St. Paul"
  description: "A hub of creativity and innovation for St. Thomas students, located in the Anderson Student Center on campus."
  features:
    - "Dynamic makerspace for students from any discipline"
    - "From sewing clothing to 3D printing prototypes"
    - "Staffed by student workers and a program manager"
  location: "Anderson Student Center, University of St. Thomas, St. Paul, MN"
  website: "https://www.stthomas.edu"
  access: "All St. Thomas students and faculty/staff; no additional cost"

- name: "St. Cloud State University – Husky Make-It Space"
  category: "education"
  city: "St. Cloud"
  description: "SCSU's makerspace and fabrication lab, housed in Headley Hall, supports students and community partners."
  features:
    - "3D printing (ABS plastic)"
    - "Laser cutting/engraving, vinyl sticker cutting"
    - "Large-format printing"
    - "CNC plasma cutting and routing for woodworking/metalworking"
  location: "Headley Hall 116, SCSU, St. Cloud, MN"
  website: "https://www.stcloudstate.edu/ets/facilities.aspx"
  access: "Primarily for SCSU students and faculty; external collaborations possible with permission"

- name: "Other Educational Maker Programs"
  category: "education"
  city: "Various"
  description: "Many Minnesota schools and colleges incorporate maker labs into their curriculum for STEM/STEAM education."
  features:
    - "Shakopee High School Innovation Hub – Fab Lab for hands-on design and fabrication"
    - "Century College – Fab Labs as part of technical and arts education"
    - "Mounds Park Academy – AnnMarie Thomas Makerspace"
  location: "Various locations across Minnesota"
  website: "https://www.shakopee.k12.mn.us/Page/8620"
  access: "Typically curriculum-supported; available to enrolled students"

- name: "Maker Meetups and Events"
  category: "program"
  city: "Various"
  description: "Various nonprofits and volunteer groups support the maker community through events, providing venues to showcase projects, learn, and inspire the public."
  features:
    - "Mini Maker Faires"
    - "Maker Fair Minnesota in St. Peter"
    - "Opportunities to showcase projects and learn from other makers"
  location: "Various locations across Minnesota"
  website: "https://makerfaire.com"
  access: "Open to the public; check individual event listings for details"

- name: "Community Education Programs"
  category: "program"
  city: "Various"
  description: "Many city and county community education departments run maker-focused programs or clubs to broaden access to maker skills and tools."
  features:
    - "Minneapolis Community Ed – adult classes in 3D printing and electronics"
    - "Rochester Public Library – teen makerspace program"
    - "Willmar GLARS – makerspace for youth"
  location: "Various locations across Minnesota"
  website: "https://www.minneapolisparks.org"
  access: "Typically nonprofit or government-funded; open to the public"

- name: "Launch Minnesota (State of MN – DEED)"
  category: "organization"
  city: "Statewide"
  description: "A statewide initiative to accelerate startups and innovation, spearheaded by the Minnesota Department of Employment and Economic Development."
  features:
    - "Innovation Grants up to $35,000 for R&D, prototyping, and product development"
    - "Over 60% of grant recipients from greater MN or underrepresented groups"
    - "Network of startup incubators, education programs, and mentorship"
    - "SBIR/STTR training and connectivity to investors"
  location: "Statewide (administered by MN DEED)"
  website: "https://mn.gov/deed/launchmn"
  access: "Grants, educational events, and programs open to Minnesota entrepreneurs and innovators"

- name: "Enterprise Minnesota"
  category: "organization"
  city: "Statewide"
  description: "A nonprofit consulting organization chartered as Minnesota's official Manufacturing Extension Partnership (MEP) affiliate, helping small and medium manufacturers compete and grow."
  features:
    - "Process improvement and quality management (ISO certification)"
    - "Technology adoption including additive manufacturing/3D printing"
    - "Enterprise Minnesota Magazine and annual State of Manufacturing survey"
    - "Business consultations and workshops"
  location: "Statewide (headquartered in Minneapolis)"
  website: "https://www.enterpriseminnesota.org"
  access: "Business consultations and workshops (fees often subsidized by state/federal MEP funds)"
```

- [ ] **Step 2: Verify the YAML is valid**

Run: `ruby -ryaml -e "YAML.load_file('_data/makers.yaml'); puts 'Valid YAML'"`

Expected: `Valid YAML`

- [ ] **Step 3: Verify Jekyll can build with the new data**

Run: `bundle exec jekyll build 2>&1 | tail -5`

Expected: Build succeeds with no errors.

- [ ] **Step 4: Commit**

```bash
git add _data/makers.yaml
git commit -m "refactor: standardize makers.yaml to consistent schema

Unify all entries to use the same fields: name, category, city,
description, features, location, website, access. Rename membership
to access, flatten nested structures, add category to every entry."
```

---

### Task 2: Create the North Star Gear SVG logo

**Files:**
- Create: `assets/images/logo.svg`

- [ ] **Step 1: Create the assets/images directory**

Run: `mkdir -p assets/images`

- [ ] **Step 2: Create the SVG logo file**

Create `assets/images/logo.svg` — a gear/cog shape with a four-pointed North Star in the center. Colors: white gear outline and star points, light blue (#4B9CD3) center dot. Viewbox 64x64, no external dependencies.

```svg
<svg xmlns="http://www.w3.org/2000/svg" width="64" height="64" viewBox="0 0 64 64" fill="none">
  <!-- Gear teeth (12 teeth around a circle) -->
  <path d="
    M29 4 L29 9 Q26 10 24 12 L19 9 L16 13 L20 17 Q18 19 17 22 L12 21 L11 26 L16 28 Q16 30 16 32 Q16 34 16 36 L11 38 L12 43 L17 42 Q18 45 20 47 L16 51 L19 55 L24 52 Q26 54 29 55 L29 60 L35 60 L35 55 Q38 54 40 52 L45 55 L48 51 L44 47 Q46 45 47 42 L52 43 L53 38 L48 36 Q48 34 48 32 Q48 30 48 28 L53 26 L52 21 L47 22 Q46 19 44 17 L48 13 L45 9 L40 12 Q38 10 35 9 L35 4 Z
  " fill="none" stroke="white" stroke-width="1.5" stroke-linejoin="round"/>
  <!-- North Star: four-pointed star -->
  <path d="M32 18 L34 29 L32 31 L30 29 Z" fill="white"/>
  <path d="M32 46 L34 35 L32 33 L30 35 Z" fill="white"/>
  <path d="M18 32 L29 30 L31 32 L29 34 Z" fill="white"/>
  <path d="M46 32 L35 30 L33 32 L35 34 Z" fill="white"/>
  <!-- Center dot -->
  <circle cx="32" cy="32" r="3" fill="#4B9CD3"/>
</svg>
```

- [ ] **Step 3: Verify the SVG renders**

Run: `open assets/images/logo.svg`

Expected: SVG opens in browser/preview showing a gear with a four-pointed star.

- [ ] **Step 4: Commit**

```bash
git add assets/images/logo.svg
git commit -m "feat: add North Star Gear SVG logo"
```

---

### Task 3: Restyle the CSS with MN flag palette

**Files:**
- Modify: `assets/css/main.css` (complete rewrite)

- [ ] **Step 1: Replace `assets/css/main.css` with the full redesigned stylesheet**

```css
:root {
    --navy: #002855;
    --navy-light: #003d7a;
    --star-blue: #4B9CD3;
    --bg: #fafbfc;
    --card-bg: #ffffff;
    --border: #e0e4e8;
    --text: #1a1a2e;
    --text-light: #666;
    --pill-bg: #f0f4f8;
    --filter-bg: #f5f7fa;
    --max-width: 1100px;
}

/* Reset & Base */
*, *::before, *::after {
    box-sizing: border-box;
    margin: 0;
    padding: 0;
}

html {
    overflow-y: scroll;
    -webkit-text-size-adjust: 100%;
}

body {
    font-family: -apple-system, system-ui, 'Segoe UI', sans-serif;
    line-height: 1.6;
    color: var(--text);
    background: var(--bg);
    -webkit-font-smoothing: antialiased;
}

a {
    color: var(--star-blue);
    text-decoration: none;
}

a:hover {
    text-decoration: underline;
}

/* Hero */
.hero {
    background: linear-gradient(135deg, var(--navy) 0%, var(--navy-light) 100%);
    padding: 3rem 1.5rem;
    text-align: center;
    color: white;
}

.hero-logo {
    width: 72px;
    height: 72px;
    margin-bottom: 1rem;
}

.hero h1 {
    font-size: 2.5rem;
    font-weight: 700;
    letter-spacing: -0.01em;
    margin-bottom: 0.5rem;
}

.hero p {
    font-size: 1.1rem;
    opacity: 0.85;
    max-width: 500px;
    margin: 0 auto;
    line-height: 1.5;
}

/* Category Filter Bar */
.filter-bar {
    background: var(--filter-bg);
    padding: 1rem 1.5rem;
    display: flex;
    gap: 0.5rem;
    justify-content: center;
    flex-wrap: wrap;
    border-bottom: 1px solid var(--border);
}

.filter-btn {
    background: var(--card-bg);
    color: var(--navy);
    border: 1px solid var(--border);
    padding: 0.4rem 1rem;
    border-radius: 20px;
    font-size: 0.875rem;
    font-weight: 500;
    cursor: pointer;
    font-family: inherit;
    transition: background 0.15s, color 0.15s, border-color 0.15s;
}

.filter-btn:hover {
    border-color: var(--navy);
}

.filter-btn.active {
    background: var(--navy);
    color: white;
    border-color: var(--navy);
}

/* Card Grid */
.card-grid {
    max-width: var(--max-width);
    margin: 0 auto;
    padding: 1.5rem;
    display: grid;
    grid-template-columns: repeat(2, 1fr);
    gap: 1.25rem;
}

@media (max-width: 720px) {
    .card-grid {
        grid-template-columns: 1fr;
    }

    .hero h1 {
        font-size: 1.8rem;
    }
}

/* Cards */
.card {
    background: var(--card-bg);
    border: 1px solid var(--border);
    border-radius: 8px;
    padding: 1.25rem;
    display: flex;
    flex-direction: column;
}

.card-category {
    font-size: 0.7rem;
    text-transform: uppercase;
    letter-spacing: 0.1em;
    color: var(--star-blue);
    font-weight: 600;
    margin-bottom: 0.25rem;
}

.card h3 {
    font-size: 1.1rem;
    font-weight: 700;
    color: var(--navy);
    margin-bottom: 0.15rem;
}

.card-city {
    font-size: 0.85rem;
    color: var(--text-light);
    margin-bottom: 0.5rem;
}

.card-description {
    font-size: 0.875rem;
    color: #444;
    line-height: 1.5;
    margin-bottom: 0.75rem;
}

.card-features {
    display: flex;
    flex-wrap: wrap;
    gap: 0.35rem;
    margin-bottom: 0.75rem;
    list-style: none;
}

.card-features li {
    background: var(--pill-bg);
    color: var(--navy);
    padding: 0.15rem 0.6rem;
    border-radius: 10px;
    font-size: 0.75rem;
}

.card-access {
    font-size: 0.8rem;
    color: var(--navy);
    font-weight: 500;
    margin-bottom: 0.75rem;
}

.card-links {
    margin-top: auto;
    display: flex;
    align-items: center;
    gap: 0.75rem;
    font-size: 0.8rem;
}

.card-links a {
    color: var(--star-blue);
    font-weight: 500;
}

.card-links .social-links {
    display: flex;
    gap: 0.5rem;
    align-items: center;
}

.card-links .social-links a {
    color: var(--text-light);
    font-size: 0.75rem;
}

.card-links .social-links a:hover {
    color: var(--navy);
    text-decoration: none;
}

/* Footer */
footer {
    background: var(--navy);
    padding: 1.5rem;
    text-align: center;
    color: rgba(255, 255, 255, 0.6);
    font-size: 0.8rem;
}

footer a {
    color: rgba(255, 255, 255, 0.8);
}

footer a:hover {
    color: white;
}
```

- [ ] **Step 2: Verify the CSS is valid**

Run: `bundle exec jekyll build 2>&1 | tail -5`

Expected: Build succeeds.

- [ ] **Step 3: Commit**

```bash
git add assets/css/main.css
git commit -m "feat: restyle site with MN flag color palette

Replace skeleton CSS with full design system using navy (#002855),
North Star blue (#4B9CD3), and light gray. Includes hero, filter bar,
card grid, and footer styles with responsive breakpoint at 720px."
```

---

### Task 4: Update the layout and includes

**Files:**
- Modify: `_layouts/base.html`
- Modify: `_includes/head.html`
- Modify: `_includes/footer.html`

- [ ] **Step 1: Update `_layouts/base.html`**

Remove the `common-css` front matter (CSS is already linked in head.html). Keep the structure simple:

```html
<!DOCTYPE html>
<html lang="en">
  {% include head.html %}
  <body>
    {{ content }}
    {% include footer.html %}
  </body>
</html>
```

- [ ] **Step 2: Update `_includes/head.html`**

Update the title to use the site title from `_config.yml` and keep the existing meta tags:

```html
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>{{ site.title }}</title>

    <!-- Favicon -->
    <link rel="icon" href="/assets/favicon.svg" type="image/svg+xml">

    <!-- Primary Meta Tags -->
    <meta name="title" content="{{ site.title }}">
    <meta name="description" content="{{ site.description }}">

    <!-- Open Graph / Facebook -->
    <meta property="og:type" content="website">
    <meta property="og:url" content="{{ site.url }}/">
    <meta property="og:title" content="{{ site.title }}">
    <meta property="og:description" content="{{ site.description }}">

    <!-- Twitter -->
    <meta name="twitter:card" content="summary">
    <meta name="twitter:url" content="{{ site.url }}/">
    <meta name="twitter:title" content="{{ site.title }}">
    <meta name="twitter:description" content="{{ site.description }}">

    <link rel="stylesheet" href="/assets/css/main.css">
</head>
```

- [ ] **Step 3: Update `_includes/footer.html`**

```html
<footer>
    makeminnesota.org &copy; 2025 Kyle Boon &bull;
    <a href="https://creativecommons.org/licenses/by/4.0/">CC BY 4.0</a> &bull;
    <a href="mailto:kyle.f.boon@gmail.com">Contact</a>
</footer>
```

- [ ] **Step 4: Verify build**

Run: `bundle exec jekyll build 2>&1 | tail -5`

Expected: Build succeeds.

- [ ] **Step 5: Commit**

```bash
git add _layouts/base.html _includes/head.html _includes/footer.html
git commit -m "refactor: update layout and includes for redesign

Simplify base layout, use site config variables in meta tags,
restyle footer to match navy theme."
```

---

### Task 5: Rebuild `index.html` with hero, filter bar, and card grid

**Files:**
- Modify: `index.html`

- [ ] **Step 1: Replace `index.html` with the new page structure**

```html
---
layout: base
---
<header class="hero">
    <img src="/assets/images/logo.svg" alt="Make Minnesota logo" class="hero-logo">
    <h1>Make Minnesota</h1>
    <p>Find your shop. Learn a craft. Build something real.</p>
</header>

<nav class="filter-bar">
    <button class="filter-btn active" data-filter="all">All</button>
    <button class="filter-btn" data-filter="makerspace">Makerspaces</button>
    <button class="filter-btn" data-filter="education">Education</button>
    <button class="filter-btn" data-filter="program">Programs</button>
    <button class="filter-btn" data-filter="organization">Organizations</button>
</nav>

<div class="card-grid">
    {% for space in site.data.makers %}
    <div class="card" data-category="{{ space.category }}">
        <span class="card-category">{{ space.category }}</span>
        <h3>{{ space.name }}</h3>
        <p class="card-city">{{ space.city }}</p>
        <p class="card-description">{{ space.description }}</p>
        <ul class="card-features">
            {% for feature in space.features %}
            <li>{{ feature }}</li>
            {% endfor %}
        </ul>
        <p class="card-access">{{ space.access }}</p>
        <div class="card-links">
            <a href="{{ space.website }}" target="_blank" rel="noopener">Visit website &rarr;</a>
            {% if space.social %}
            <span class="social-links">
                {% if space.social.facebook %}<a href="{{ space.social.facebook }}" target="_blank" rel="noopener" title="Facebook">FB</a>{% endif %}
                {% if space.social.instagram %}<a href="{{ space.social.instagram }}" target="_blank" rel="noopener" title="Instagram">IG</a>{% endif %}
                {% if space.social.youtube %}<a href="{{ space.social.youtube }}" target="_blank" rel="noopener" title="YouTube">YT</a>{% endif %}
                {% if space.social.twitter %}<a href="{{ space.social.twitter }}" target="_blank" rel="noopener" title="Twitter">TW</a>{% endif %}
                {% if space.social.linkedin %}<a href="{{ space.social.linkedin }}" target="_blank" rel="noopener" title="LinkedIn">LI</a>{% endif %}
            </span>
            {% endif %}
        </div>
    </div>
    {% endfor %}
</div>

<script>
document.addEventListener('DOMContentLoaded', function () {
    var buttons = document.querySelectorAll('.filter-btn');
    var cards = document.querySelectorAll('.card');

    buttons.forEach(function (btn) {
        btn.addEventListener('click', function () {
            buttons.forEach(function (b) { b.classList.remove('active'); });
            btn.classList.add('active');
            var filter = btn.getAttribute('data-filter');

            cards.forEach(function (card) {
                if (filter === 'all' || card.getAttribute('data-category') === filter) {
                    card.style.display = '';
                } else {
                    card.style.display = 'none';
                }
            });
        });
    });
});
</script>
```

- [ ] **Step 2: Verify the site builds and renders**

Run: `bundle exec jekyll serve`

Open `http://localhost:4000` in a browser. Verify:
- Hero displays with logo, title, and tagline
- Filter pills are visible and clickable
- Cards render in a 2-column grid with all data fields
- Clicking a filter pill shows only matching cards
- "All" shows all cards
- Footer appears at the bottom

- [ ] **Step 3: Commit**

```bash
git add index.html
git commit -m "feat: rebuild index with hero, filter bar, and card grid

Add hero header with logo and tagline, category filter pills with
vanilla JS filtering, and responsive card grid rendering all entries
from makers.yaml."
```

---

### Task 6: Add GitHub Pages deployment

**Files:**
- Create: `.github/workflows/jekyll.yml`
- Create: `CNAME`

- [ ] **Step 1: Create the GitHub Actions workflow directory**

Run: `mkdir -p .github/workflows`

- [ ] **Step 2: Create `.github/workflows/jekyll.yml`**

```yaml
name: Deploy Jekyll site to Pages

on:
  push:
    branches: ["main"]
  workflow_dispatch:

permissions:
  contents: read
  pages: write
  id-token: write

concurrency:
  group: "pages"
  cancel-in-progress: false

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout
        uses: actions/checkout@v4
      - name: Setup Pages
        uses: actions/configure-pages@v5
      - name: Build with Jekyll
        uses: actions/jekyll-build-pages@v1
        with:
          source: ./
          destination: ./_site
      - name: Upload artifact
        uses: actions/upload-pages-artifact@v3

  deploy:
    environment:
      name: github-pages
      url: ${{ steps.deployment.outputs.page_url }}
    runs-on: ubuntu-latest
    needs: build
    steps:
      - name: Deploy to GitHub Pages
        id: deployment
        uses: actions/deploy-pages@v4
```

- [ ] **Step 3: Create `CNAME` file**

```
makeminnesota.org
```

- [ ] **Step 4: Commit**

```bash
git add .github/workflows/jekyll.yml CNAME
git commit -m "feat: add GitHub Pages deployment workflow and CNAME

GitHub Actions workflow builds Jekyll on push to main and deploys
to GitHub Pages. CNAME configured for makeminnesota.org."
```

---

### Task 7: Final verification

- [ ] **Step 1: Clean build**

Run: `rm -rf _site && bundle exec jekyll build 2>&1 | tail -10`

Expected: Build succeeds with no errors or warnings.

- [ ] **Step 2: Serve and verify locally**

Run: `bundle exec jekyll serve`

Open `http://localhost:4000` and verify:
- Hero renders with logo, "Make Minnesota", and tagline
- All 22 entries appear as cards in the grid
- Category filter pills work (each category filters correctly)
- Cards show: category tag, name, city, description, feature pills, access, website link
- Footer shows copyright with CC BY 4.0 link
- Responsive: resize browser below 720px and verify single-column layout

- [ ] **Step 3: Update `.gitignore`**

Add `.superpowers/` to `.gitignore` so brainstorm session files aren't committed:

```
.idea/
_site
.superpowers/
```

- [ ] **Step 4: Commit**

```bash
git add .gitignore
git commit -m "chore: add .superpowers to gitignore"
```

---

### Post-deployment: DNS Configuration (manual, by site owner)

After the first push deploys successfully, configure DNS at your domain registrar:

**Option A — CNAME record (if using www subdomain or apex via provider that supports CNAME flattening):**
```
CNAME  makeminnesota.org  kyleboon.github.io
```

**Option B — A records (for apex domain):**
```
A  makeminnesota.org  185.199.108.153
A  makeminnesota.org  185.199.109.153
A  makeminnesota.org  185.199.110.153
A  makeminnesota.org  185.199.111.153
```

Then in GitHub repo Settings → Pages → Custom domain, enter `makeminnesota.org` and enable "Enforce HTTPS".
