# Index And Plan Layout Refresh Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Refresh the homepage and weekly plan pages so the homepage explains the product and highlights available weekly editions, while each plan page becomes a denser operational brief for one weekly recommendation.

**Architecture:** Keep Hugo + YAML as the source of truth. Update the homepage YAML shape and homepage template to remove waitlist flow, add product/problem/system sections, add the four gold-standard cards, and render a visually dominant current-week edition plus quieter archive edition cards. Simplify the competition plan page away from multi-intent chooser behavior into a single weekly briefing flow with an edition summary, decision outcome, main recommendation, fallback handling, and execution details.

**Tech Stack:** Hugo templates, YAML data files in `data/pages/`, partials in `layouts/partials/`, static HTML generation via `hugo`

---

## File Structure

### Existing files to modify

- Modify: `data/pages/landing_page.yaml`
  - Replace waitlist-oriented homepage content with product/problem/system/gold-standard/edition-preview/trust content.
- Modify: `layouts/index.html`
  - Rebuild homepage structure to match the approved section order and remove waitlist.
- Modify: `layouts/partials/site-nav.html`
  - Update nav links and CTA behavior to point to the editions section instead of waitlist.
- Modify: `data/pages/example_page.yaml`
  - Reshape the competition weekly plan data toward a single recommendation flow.
- Modify: `layouts/plan/single.html`
  - Remove competition reliance on intent switching and render one operational briefing flow.
- Modify: `README.md`
  - Update local documentation only if the homepage/plan behavior descriptions have materially drifted.

### Existing files to inspect during implementation

- Inspect: `data/pages/20260522-bandung.yaml`
  - Check whether the new single-plan shape should also be applied there for consistency.
- Inspect: `layouts/_default/baseof.html`
  - Confirm no global changes are required for the new sections.
- Inspect: `layouts/partials/site-shell-styles.html`
  - Reuse existing shell variables where possible before adding new local styles.

### Files not to modify for this work

- Leave: `layouts/blog/*`
- Leave: `content/blog/*`
- Leave: `.github/workflows/hugo.yml`

---

### Task 1: Reshape homepage YAML for product-first content

**Files:**
- Modify: `data/pages/landing_page.yaml`
- Test: `hugo`

- [ ] **Step 1: Replace the masthead nav with edition-oriented navigation**

Update the `masthead.nav` entries in `data/pages/landing_page.yaml` so the homepage navigates to product explanation and edition preview sections instead of waitlist.

Use this structure:

```yaml
masthead:
  title: "RamahAnak"
  tagline: "Weekend decision system untuk keluarga."
  nav:
    - label: "Problem"
      href: "#problem"
      class: "mast__nav-link"
    - label: "How it works"
      href: "#how-it-works"
      class: "mast__nav-link"
    - label: "This week's editions"
      href: "#weekly-editions"
      class: "button button--primary"
```

- [ ] **Step 2: Replace hero and remove waitlist-only content**

Replace the current `hero`, `waitlist`, and any copy that assumes signup capture with product-framing copy and CTA copy that routes to edition previews.

Use this target shape:

```yaml
hero:
  eyebrow: "Family Decision Product"
  title: "RamahAnak membantu keluarga Bandung memutuskan weekend tanpa ribet."
  lead: "Bukan direktori tempat. Bukan blog inspirasi. RamahAnak menyaring risiko dan memberi satu rekomendasi yang layak dijalankan."
  primary_cta:
    label: "This Week's Editions"
    href: "#weekly-editions"
  secondary_cta:
    label: "How it works"
    href: "#how-it-works"
```

Delete the `waitlist:` block entirely from the file once the template no longer reads it.

- [ ] **Step 3: Add problem, system, and gold-standard sections**

Add approved product-explanation blocks to `data/pages/landing_page.yaml`.

Use this exact structure:

```yaml
problem:
  eyebrow: "Problem it solves"
  cards:
    - title: "Decision fatigue"
      copy: "Setiap weekend orang tua mengulang proses memilih tempat dengan kepastian yang rendah."
    - title: "Execution risk"
      copy: "Cuaca, macet, asap rokok, dan keramaian bisa membuat outing gagal walau tempatnya terlihat menarik."
    - title: "Mismatch risk"
      copy: "Yang terlihat bagus di internet belum tentu nyaman untuk keluarga saat benar-benar dijalankan."

how_it_works:
  eyebrow: "How it works"
  steps:
    - title: "Filter venues"
      copy: "Venue disaring dulu dengan standar keluarga, bukan sekadar rating umum."
    - title: "Add weekend risks"
      copy: "Kondisi cuaca, crowd, traffic, dan event kota ditambahkan sebelum keputusan dikunci."
    - title: "Publish one weekly recommendation"
      copy: "Hasil akhirnya adalah satu keputusan utama yang jelas, plus fallback jika kondisi berubah."

gold_standard:
  eyebrow: "Gold standard"
  cards:
    - title: "Minimal smoke exposure"
      copy: "Paparan asap aktif diperlakukan sebagai hard gate, bukan kompromi kecil."
    - title: "Clean toilet"
      copy: "Kebersihan toilet adalah syarat dasar agar outing keluarga tetap layak dijalankan."
    - title: "Less crowd"
      copy: "Keramaian ditekan untuk mengurangi stres, antrean, dan waktu terbuang."
    - title: "Kids can have activities"
      copy: "Anak tetap perlu aktivitas yang cukup agar outing terasa benar-benar worth it."
```

- [ ] **Step 4: Add current-week and archive-edition data**

Add a compact edition-preview block that supports one dominant current week and quieter archive entries.

Use this exact YAML scaffold and adapt the real values from existing plan data:

```yaml
weekly_editions:
  eyebrow: "This week's editions"
  featured:
    title: "Bandung 20 Mei 2026"
    href: "/plan/bdg520-20260520-bandung/"
    summary: "Nara Park jadi pick utama minggu ini karena memberi jalur outing paling stabil sebelum risiko crowd dan traffic naik."
  archive:
    - title: "Bandung 5 Mei 2026"
      href: "/plan/g7k2r1-20260505-glide-preview/"
      summary: "Preview decision format untuk outing keluarga dengan fallback yang rapat."
```

If `data/pages/20260522-bandung.yaml` should be the featured edition instead, swap the values to match the correct current page.

- [ ] **Step 5: Add trust section data with brief Google Cloud note**

Replace or reshape the current `trust` block so it supports the approved homepage close.

Use this shape:

```yaml
trust:
  eyebrow: "Why this is reliable"
  cards:
    - title: "Manual curation"
      copy: "RamahAnak tidak menebak-nebak venue. Data dijaga konservatif dan diprioritaskan dari validasi langsung."
    - title: "Operational thinking"
      copy: "Rekomendasi tidak berhenti di venue. Timing, exit window, dan fallback adalah bagian dari produk."
    - title: "Built with Google Cloud"
      copy: "Google Cloud dipakai sebagai infrastruktur pendukung, bukan pusat cerita produk."
```

- [ ] **Step 6: Run Hugo build after homepage YAML changes**

Run:

```bash
hugo
```

Expected:

```text
Start building sites …
... Total in <time>
```

If Hugo fails with missing keys from the old homepage structure, note the failing key names and fix the template in Task 2.

- [ ] **Step 7: Commit the homepage data reshape**

Run:

```bash
git add data/pages/landing_page.yaml
git commit -m "feat: reshape homepage content for edition-first flow"
```

Expected:

```text
[main <hash>] feat: reshape homepage content for edition-first flow
```

---

### Task 2: Rebuild homepage template around product explanation and edition cards

**Files:**
- Modify: `layouts/index.html`
- Modify: `layouts/partials/site-nav.html`
- Inspect: `layouts/partials/site-shell-styles.html`
- Test: `hugo`

- [ ] **Step 1: Update homepage nav and hero CTA rendering**

In `layouts/partials/site-nav.html`, keep the existing nav shell but make sure it renders the new edition-oriented nav items from `masthead.nav` unchanged.

The important rendering loop should remain structurally like this:

```go-html-template
{{ range $navLinks }}
  <a href="{{ if $homeOnly }}/{{ else }}{{ .href }}{{ end }}" class="nav__link">{{ .label }}</a>
{{ end }}
{{ range $ctaLinks }}
  <a href="{{ if $homeOnly }}/{{ else }}{{ .href }}{{ end }}" class="nav__cta">{{ .label }}</a>
{{ end }}
```

In `layouts/index.html`, stop deriving hero actions from the old nav-only waitlist CTA. Render explicit CTA buttons from `hero.primary_cta` and `hero.secondary_cta` instead.

Use this target block:

```go-html-template
<div class="hero__actions">
  <a href="{{ $page.hero.primary_cta.href }}" class="btn-hero">{{ $page.hero.primary_cta.label }}</a>
  <a href="{{ $page.hero.secondary_cta.href }}" class="btn-hero-ghost">{{ $page.hero.secondary_cta.label }}</a>
</div>
```

- [ ] **Step 2: Replace the current homepage body sections**

Remove the current `what_you_get`, `sample_plan`, `waitlist`, and FAQ-driven homepage flow from `layouts/index.html`.

Replace it with this section skeleton:

```go-html-template
<section id="problem">...</section>
<section id="how-it-works">...</section>
<section id="gold-standard">...</section>
<section id="weekly-editions">...</section>
<section id="trust">...</section>
```

Inside those sections:

- `problem` renders `problem.cards`
- `how-it-works` renders `how_it_works.steps`
- `gold-standard` renders `gold_standard.cards`
- `weekly-editions` renders one featured card and archive cards
- `trust` renders `trust.cards`

- [ ] **Step 3: Render the problem and how-it-works sections**

Add compact grid-based blocks for the product explanation sections.

Use this rendering pattern:

```go-html-template
<section id="problem" class="problem-section">
  <div class="section-header">
    <div>
      <p class="section-header__index">{{ $page.problem.eyebrow }}</p>
    </div>
  </div>
  <div class="cards-row">
    {{ range $page.problem.cards }}
      <article class="card">
        <div class="card__body">
          <h3 class="card__title">{{ .title }}</h3>
          <p class="card__desc">{{ .copy }}</p>
        </div>
      </article>
    {{ end }}
  </div>
</section>
```

Mirror the same compact pattern for `how_it_works.steps`, but preserve the step order visually with a small numbered label.

- [ ] **Step 4: Replace the old trust/golden-rules treatment with the new gold-standard cards**

Remove any current homepage treatment that makes the standards feel secondary.

Render `gold_standard.cards` as four compact cards with label + short description.

Use this rendering pattern:

```go-html-template
<section id="gold-standard" class="trust-section">
  <div class="trust-header">
    <div>
      <p class="trust-header__index">{{ $page.gold_standard.eyebrow }}</p>
    </div>
  </div>
  <div class="trust-grid">
    {{ range $page.gold_standard.cards }}
      <article class="trust-card">
        <h3 class="trust-card__title">{{ .title }}</h3>
        <p>{{ .copy }}</p>
      </article>
    {{ end }}
  </div>
</section>
```

- [ ] **Step 5: Add featured and archive edition cards**

Add a homepage editions section where the featured edition is visually dominant and archive cards are quieter.

Use this rendering structure:

```go-html-template
<section id="weekly-editions" class="editions-section">
  <div class="section-header">
    <div>
      <p class="section-header__index">{{ $page.weekly_editions.eyebrow }}</p>
    </div>
  </div>
  <div class="edition-featured">
    <h3>{{ $page.weekly_editions.featured.title }}</h3>
    <p>{{ $page.weekly_editions.featured.summary }}</p>
    <a href="{{ $page.weekly_editions.featured.href }}" class="btn-hero">Open plan</a>
  </div>
  <div class="edition-archive-grid">
    {{ range $page.weekly_editions.archive }}
      <article class="edition-archive-card">
        <h4>{{ .title }}</h4>
        <p>{{ .summary }}</p>
        <a href="{{ .href }}" class="card__link">Open plan</a>
      </article>
    {{ end }}
  </div>
</section>
```

Archive styling should look clearly subordinate through lighter borders, quieter background, or smaller heading scale.

- [ ] **Step 6: Remove the waitlist section and any dead homepage references**

Delete the waitlist form block from `layouts/index.html`.

Delete any related styles that exist only for:

- `.waitlist-section`
- `.waitlist-inner`
- `.waitlist-form`
- `.btn-waitlist`

Leave shared utility styles alone if they are reused elsewhere.

- [ ] **Step 7: Run Hugo build to catch missing keys or broken anchors**

Run:

```bash
hugo
```

Expected:

```text
Start building sites …
... Total in <time>
```

Then inspect the generated homepage quickly:

```bash
sed -n '1,220p' public/index.html
```

Expected:

- `#problem`
- `#how-it-works`
- `#gold-standard`
- `#weekly-editions`
- no waitlist form markup

- [ ] **Step 8: Commit the homepage template refresh**

Run:

```bash
git add layouts/index.html layouts/partials/site-nav.html public/index.html
git commit -m "feat: refresh homepage around editions and product framing"
```

Expected:

```text
[main <hash>] feat: refresh homepage around editions and product framing
```

If `public/index.html` is ignored in your workflow, omit it from `git add`.

---

### Task 3: Reshape the weekly plan data into a single-brief competition format

**Files:**
- Modify: `data/pages/example_page.yaml`
- Inspect: `data/pages/20260522-bandung.yaml`
- Test: `hugo`

- [ ] **Step 1: Replace multi-intent top-level structure with single-plan fields**

In `data/pages/example_page.yaml`, stop relying on `default_intent_key` and `intents` for the competition page.

Replace that structure with this single-brief scaffold:

```yaml
summary:
  eyebrow: "Edition summary"
  title: "Persib convoy dan crowd sore membuat window outing minggu ini lebih sempit."
  copy: "Rekomendasi dikunci ke venue dengan eksekusi paling stabil sebelum traffic dan crowd naik."

decision_outcome:
  label: "Worth It"
  tone: "positive"

main_recommendation:
  title: "Pergi ke Nara Park jam 10.30."
  area_cluster: "Bandung Utara (<=30 min radius)"
  identity: "Area makan keluarga dengan ruang gerak anak yang cukup dan fallback rapat."
  maps_url: "https://www.google.com/maps/search/?api=1&query=Nara+Park+Bandung"
  execution:
    arrival: "10.30"
    exit_window: "Selesai sebelum 14.30"
    degradation: "Setelah 15.30 crowd dan traffic pulang mulai menaik."
  why_it_works:
    - "Window pagi hingga early afternoon masih paling stabil."
    - "Anak tetap punya aktivitas tanpa orang tua terus bergerak."

fallbacks:
  nearby:
    title: "Baker Street"
    trigger: "Pakai jika orang tua butuh outing yang lebih tenang."
  weather:
    title: "Hompimplay"
    trigger: "Pakai jika hujan membuat jalur outdoor tidak lagi layak."
  overcrowded:
    title: "Laloba"
    trigger: "Pakai jika parkir dan crowd di venue utama terlalu padat."

execution_details:
  parking: "Datang sebelum 11.00 untuk menjaga transisi parkir tetap ringan."
  toilet: "Toilet layak dipakai keluarga dan harus disebutkan eksplisit."
  prayer: "Mushola atau akses ibadah disebutkan jika tersedia."
  kids_activity: "Aktivitas utama anak harus dijelaskan singkat."
  cutoff: "Jika belum masuk area utama sebelum 12.00, nilai outing mulai turun."
```

Keep the existing labels block only if `layouts/plan/single.html` still uses it after Task 4.

- [ ] **Step 2: Mirror the same shape into the active weekly plan data if needed**

Open `data/pages/20260522-bandung.yaml` and compare it with the updated `example_page.yaml`.

If the site should show both pages in the new format, convert `20260522-bandung.yaml` to the same single-brief structure.

If not, record in a code comment or commit message that only the example page was migrated intentionally.

- [ ] **Step 3: Remove unused competition-only chooser fields from the migrated page**

Delete fields that will become dead after Task 4, such as:

- `default_intent_key`
- `intent_menu`
- `intents`

Only keep data that the simplified competition page will render.

- [ ] **Step 4: Run Hugo build after plan-data migration**

Run:

```bash
hugo
```

Expected:

```text
Start building sites …
... Total in <time>
```

If the plan page fails because the template still expects `edition.intents`, proceed to Task 4 and return to the build afterward.

- [ ] **Step 5: Commit the plan-data simplification**

Run:

```bash
git add data/pages/example_page.yaml data/pages/20260522-bandung.yaml
git commit -m "feat: simplify weekly recommendation data for competition brief"
```

Expected:

```text
[main <hash>] feat: simplify weekly recommendation data for competition brief
```

If `20260522-bandung.yaml` was intentionally not changed, omit it from `git add`.

---

### Task 4: Rebuild the weekly plan template as a single operational brief

**Files:**
- Modify: `layouts/plan/single.html`
- Test: `hugo`
- Test: `sed -n '1,220p' public/plan/<path>/index.html`

- [ ] **Step 1: Remove dependency on intent tabs and runtime plan switching**

In `layouts/plan/single.html`, remove the competition chooser behavior that depends on:

- `edition.intents`
- `activeIntent`
- `renderTabs()`
- `renderToc()` based on intent variants
- `renderPanel()` choosing among multiple intent branches

The page should render one recommendation path directly from page data.

Delete the script-driven tab initialization pattern:

```javascript
const tabsEl = document.getElementById("intentTabs");
let activeIntent = edition.default_intent_key || edition.intents[0].key;
```

and replace it with direct rendering against the single-page data object.

- [ ] **Step 2: Replace the hero top with edition summary and decision outcome**

At the top of the main content, render the approved summary-first structure.

Use this target Go template block near the main content start:

```go-html-template
<section class="hero">
  <div>
    <span class="eyebrow">{{ .Params.summary.eyebrow }}</span>
    <h1 class="hero__title">{{ .Title }}</h1>
    <p class="hero__lead">{{ .Params.summary.title }}</p>
    <p class="hero__body">{{ .Params.summary.copy }}</p>
  </div>
  <aside class="hero__aside">
    <section class="hero-card hero-card--dark">
      <p class="hero-card__label">Decision outcome</p>
      <p class="hero-card__text">{{ .Params.decision_outcome.label }}</p>
    </section>
  </aside>
</section>
```

Tone-specific styling may be added later, but the first pass only needs the structure.

- [ ] **Step 3: Render main recommendation, fallbacks, and execution details as fixed sections**

Replace the current dynamic section builder with explicit sections.

Use this section skeleton:

```go-html-template
<section id="section-master" class="section-block section-block--primary">...</section>
<section id="section-fallback" class="section-block">...</section>
<section id="section-execution" class="section-block">...</section>
```

Inside:

- `section-master` renders `.Params.main_recommendation`
- `section-fallback` renders `.Params.fallbacks`
- `section-execution` renders `.Params.execution_details`

Render `why_it_works` as a short list:

```go-html-template
<ul class="detail-list">
  {{ range .Params.main_recommendation.why_it_works }}
    <li>{{ . }}</li>
  {{ end }}
</ul>
```

- [ ] **Step 4: Add a compact TOC for the fixed sections**

Replace the current script-built TOC with static links that match the new page sections.

Use:

```go-html-template
<ul class="toc__list">
  <li><a class="toc__link" href="#section-master">Plan utama</a></li>
  <li><a class="toc__link" href="#section-fallback">Fallback</a></li>
  <li><a class="toc__link" href="#section-execution">Execution details</a></li>
</ul>
```

If you keep a risk section, add:

```go-html-template
<li><a class="toc__link" href="#section-risk">Risk</a></li>
```

- [ ] **Step 5: Remove dead script helpers and verify no stale DOM ids remain**

Delete helper functions that only exist for the old chooser flow, including any functions equivalent to:

- `getIntent`
- `renderTabs`
- `renderPanel`

After cleanup, search for stale references:

```bash
rg -n "intentTabs|activeIntent|edition.intents|renderTabs|renderPanel|default_intent_key" layouts/plan/single.html
```

Expected:

```text
no matches
```

- [ ] **Step 6: Run Hugo build and inspect a generated plan page**

Run:

```bash
hugo
```

Expected:

```text
Start building sites …
... Total in <time>
```

Then inspect the generated plan output. If the current page path is `public/plan/g7k2r1-20260505-glide-preview/index.html`, run:

```bash
sed -n '1,220p' public/plan/g7k2r1-20260505-glide-preview/index.html
```

Expected:

- summary copy appears near the top
- decision outcome text is present
- no intent-tab markup
- no chooser script remnants

- [ ] **Step 7: Commit the weekly plan template refresh**

Run:

```bash
git add layouts/plan/single.html public/plan
git commit -m "feat: turn weekly plan into single operational brief"
```

Expected:

```text
[main <hash>] feat: turn weekly plan into single operational brief
```

If `public/plan` is ignored in your workflow, omit it from `git add`.

---

### Task 5: Final verification and documentation alignment

**Files:**
- Modify: `README.md` (only if needed)
- Test: `hugo`
- Test: `git diff --stat`

- [ ] **Step 1: Verify the homepage no longer exposes waitlist behavior**

Run:

```bash
rg -n "waitlist|Formspree|Masuk waitlist" layouts/index.html data/pages/landing_page.yaml public/index.html
```

Expected:

```text
no matches
```

If there are intentional historical mentions outside the homepage flow, confirm they are acceptable before removing them.

- [ ] **Step 2: Verify the homepage and plan responsibilities are clearly separated**

Run:

```bash
rg -n "weekly_editions|featured|archive|summary|decision_outcome|main_recommendation" data/pages layouts/index.html layouts/plan/single.html
```

Expected:

- homepage references `weekly_editions`
- plan page references `summary`, `decision_outcome`, and `main_recommendation`
- homepage does not inline the full plan structure

- [ ] **Step 3: Run final Hugo build**

Run:

```bash
hugo
```

Expected:

```text
Start building sites …
... Total in <time>
```

- [ ] **Step 4: Update README only if current behavior description is misleading**

If the README still describes the homepage as waitlist-led or does not mention edition previews + weekly plan outputs clearly, add a small clarification.

Use a minimal update like:

```md
- homepage explains the product and previews weekly editions
- plan pages contain the full weekly operational recommendation
```

If the README is already directionally correct, skip this step.

- [ ] **Step 5: Review final diff footprint**

Run:

```bash
git diff --stat HEAD~4..HEAD
```

Expected:

- homepage YAML changed
- homepage template changed
- plan YAML changed
- plan template changed
- optional README update

If the range is wrong because commit count differs, use:

```bash
git diff --stat
```

- [ ] **Step 6: Commit final verification or README touch-up**

If Step 4 changed `README.md`, run:

```bash
git add README.md
git commit -m "docs: align readme with homepage and plan behavior"
```

Expected:

```text
[main <hash>] docs: align readme with homepage and plan behavior
```

If Step 4 was skipped, do not create an extra no-op commit.

---

## Self-Review

### Spec coverage

- Homepage product framing is covered in Tasks 1 and 2.
- Gold-standard cards are covered in Tasks 1 and 2.
- Current-week versus archive edition treatment is covered in Tasks 1 and 2.
- Plan-page summary-first structure is covered in Tasks 3 and 4.
- Single-recommendation competition flow is covered in Tasks 3 and 4.
- Brief Google Cloud mention is covered in Task 1 and rendered in Task 2.

### Placeholder scan

The plan avoids `TODO`, `TBD`, and generic “handle appropriately” instructions. Each task names exact files, concrete data shapes, commands, and expected output.

### Type consistency

The implementation uses the same homepage keys throughout:

- `problem`
- `how_it_works`
- `gold_standard`
- `weekly_editions`
- `trust`

The plan-page shape is also consistent throughout:

- `summary`
- `decision_outcome`
- `main_recommendation`
- `fallbacks`
- `execution_details`

