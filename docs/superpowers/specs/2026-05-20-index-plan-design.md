# Index And Plan Layout Design

Date: 2026-05-20

## Scope

This design covers the public homepage (`index`) and the weekly recommendation page (`plan`) for the competition version of RamahAnak.

The goal is to improve information density and judge comprehension without breaking the existing Hugo + YAML content flow.

## Product Framing

RamahAnak should present as a family decision product first.

The site should communicate that it reduces weekend planning uncertainty for families in Bandung by turning many weak options into one operationally confident recommendation.

Google Cloud implementation may be mentioned, but only briefly as supporting infrastructure. It must not dominate the message.

## Competition Constraints

This design follows the existing product docs:

- The homepage explains what the product is.
- The weekly plan page is the actual recommendation artifact.
- The competition version should emphasize one authoritative recommendation, not a multi-path chooser.
- Waitlist is removed from the homepage.

## Homepage Goals

The homepage should make a judge understand:

1. What problem RamahAnak solves.
2. How the product works.
3. What quality standard it applies.
4. What weekly outputs are available right now.

The homepage should not contain the full operational recommendation. It should preview editions and route users into the weekly plan pages.

## Homepage Structure

Recommended section order:

1. Hero
2. Problem It Solves
3. How It Works
4. Gold Standard
5. This Week's Editions
6. Trust

### Hero

The hero should position RamahAnak as a family decision product for Bandung weekend planning.

It should communicate:

- family decision support
- Bandung-specific context
- smoke-free and risk-aware filtering
- one clear weekly recommendation

Primary CTA:

- Jump to `This Week's Editions`

The hero should not push signup or waitlist behavior.

### Problem It Solves

This section replaces any "why current tools fail" framing.

It should focus on the product problem directly:

- decision fatigue from repeated weekly planning
- execution risk from weather, traffic, crowd, and smoke exposure
- mismatch between online expectation and real family experience

This section should feel product-led, not argumentative.

### How It Works

This section should explain the operating model in three compact steps:

1. Filter venues
2. Add weekend risks
3. Publish one weekly recommendation

This is the main system explanation block on the homepage. It should be denser than the current landing page and should read like operational logic, not marketing fluff.

### Gold Standard

This section is a first-class trust and filtering block and should appear before the weekly edition cards.

It should contain four compact cards with a label and short explanation:

1. Minimal smoke exposure
2. Clean toilet
3. Less crowd
4. Kids can have activities

These standards explain what RamahAnak optimizes for before the user opens a recommendation.

### This Week's Editions

This section should preview available weekly outputs without embedding the full plan details.

Behavior:

- The current week's edition is visually dominant.
- Past editions remain accessible inline below or beside it.
- Past editions use quieter styling so they do not compete with the current week.

Each edition card should stay compact and clickable.

Each card should contain:

- edition title
- date
- short one-line summary
- primary action: `Open plan`

Summary rule:

- Default to emphasizing the main venue.
- If an unusual high-salience risk defines the week, the summary may emphasize that risk instead.

Examples of high-salience risks:

- Persib convoy
- severe rain window
- major congestion cutoff

Tags are removed from the edition cards unless they become truly necessary and self-explanatory.

### Trust

This section should explain why the output is credible:

- manual curation
- operational thinking
- risk-aware recommendation logic

Google Cloud should appear here only as a brief supporting note or strip, not as a headline brag block.

## Weekly Plan Page Goals

The plan page is the real product artifact.

It should make a judge think:

- this is useful
- this is decisive
- this is operational, not inspirational

The page should not behave like an intent chooser for the competition version.

## Weekly Plan Page Structure

Recommended order:

1. Edition summary
2. Decision outcome
3. Main recommendation
4. Fallbacks and risk handling
5. Execution details

### Edition Summary

The page should open with a short summary block that explains the decisive condition shaping this week's recommendation.

Purpose:

- frame why the recommendation is locked
- highlight unusual weekend conditions
- create trust before the venue is shown

Example summary inputs:

- Persib convoy risk
- rain after a specific hour
- congestion threshold
- smoke exposure concerns in competing venues

### Decision Outcome

Immediately after the edition summary, the page should show a DFS-style outcome:

- Worth It
- High Risk
- Not Worth It Today

This gives the user a fast read on recommendation confidence before reading details.

### Main Recommendation

This section should contain:

- one primary recommendation
- exact arrival time
- exit window
- why this works today

The reasoning should be explicit and scannable, not a long narrative paragraph.

### Fallbacks And Risk Handling

This section should contain:

- nearby fallback
- weather fallback
- overcrowding fallback
- clear trigger for when each fallback should be used

This preserves operational usefulness without turning the page into a branching selector.

### Execution Details

This section should contain compact, mobile-readable notes for:

- parking
- toilet
- prayer accessibility
- child activity suitability
- movement cutoff or leave-before time

It should feel like a field-use brief, not an article.

## Data And Template Implications

The design should preserve the current Hugo templating model:

- Homepage content continues to come from YAML page data.
- Weekly recommendations continue to come from YAML plan page data.
- Homepage edition cards should route into plan pages, not duplicate their full contents.

The content structure may need small YAML shape updates to support:

- homepage current-vs-past edition presentation
- gold standard descriptions
- plan edition summary block
- decision outcome label

No change is required to the core idea of YAML-backed Hugo rendering.

## Interaction And Density Principles

- Prefer scannable blocks over large paragraphs.
- Prioritize explicit reasoning over decorative filler.
- Keep visual emphasis on the current week's recommendation.
- Use quieter treatment for archive editions.
- Keep Google Cloud present but subordinate.

## Out Of Scope

This design does not include:

- waitlist capture
- blog release work
- personalized onboarding
- multi-intent recommendation branching for the competition version
- complex infrastructure showcase sections

## Recommended Implementation Direction

Implementation should simplify the current plan-page competition flow toward one authoritative weekly recommendation and align the homepage around product explanation plus edition preview cards.

The current homepage and plan page should both become denser, but their responsibilities must stay separate:

- homepage explains the product
- plan page delivers the weekly recommendation
