---
description: AI and Coffee Deck Engine. Generates branded HTML presentations for AI and Coffee events (Circle, Lunch, Dinner, All-in), community overviews, sponsor pitches, post-event recaps, or custom decks. Slide-mode navigation, real logo embedded, on-brand colors, warm voice. Designed for one-shot generation.
---

# /aiandcoffeedeck. AI and Coffee Deck Generator

> Global command. Point it at a topic and it generates a branded HTML deck.
> One-shot by default. Interview mode only when critical info is missing.

## How to install (for team members)

```bash
# From the aiandcoffee-brand-kit repo. Assumes gh is authed.
gh repo clone yhn08/aiandcoffee-brand-kit /tmp/aic-kit
cp /tmp/aic-kit/aiandcoffeedeck.md ~/.claude/commands/aiandcoffeedeck.md
cp /tmp/aic-kit/logos/*.png ~/.claude/commands/
```

Then restart Claude Code. Type `/aiandcoffeedeck <your topic>` to generate.

## Configuration

```yaml
engine:
  mode: inline
  output_dir: "$HOME/local/AI STUFF/MeetUp - AI&Coffee/artifacts/decks"
  data_dir:   "$HOME/local/AI STUFF/MeetUp - AI&Coffee/artifacts/decks/data"

logo:
  default:     "$HOME/.claude/commands/aiandcoffee-logo.png"             # TRANSPARENT. Use this always by default. Works on any background.
  transparent: "$HOME/.claude/commands/aiandcoffee-logo-transparent.png" # explicit alias for the default
  cream:       "$HOME/.claude/commands/aiandcoffee-logo-cream.png"       # cream background baked in (rarely needed fallback)
  white:       "$HOME/.claude/commands/aiandcoffee-logo-white.png"       # white background baked in (rarely needed)
  navy:        "$HOME/.claude/commands/aiandcoffee-logo-navy.png"        # navy background baked in (rarely needed)
```

## Community defaults (bake into every deck unless user overrides)

### Facts
- **Members.** 800+ across Europe.
- **Chapters.** Luxembourg (flagship, 600+), Vienna, Bucharest, Paris, plus one more chapter joining soon.
- **Formats.**
  1. **Circle.** Sunday afternoon networking. Two to three minute intro round, then open conversation.
  2. **Lunch.** Wednesday midday. Rotating venues. Professional focus. Eat, talk shop, back to work by 2pm.
  3. **Dinner.** Intimate, capped around 20. Rotating seats mid-event.
  4. **All-in.** Monthly headliner. One or two speakers plus networking.
- **Rhythm.** Weekly (Circle Sunday, Lunch Wednesday). Monthly (Dinner, All-in).
- **Cost to member.** Free. Every event. First drink often on the house.
- **Platform.** Meetup Pro.
- **Meetup URL (default CTA).** `https://www.meetup.com/aiandcoffee-luxembourg-en/`
- **Tagline.** "Where AI-curious people meet over coffee."

### Voice
- No em-dashes or en-dashes anywhere. Use periods, commas, or restructure.
- Sentences over paragraphs.
- Numbers over adjectives. "42 RSVPs" beats "great turnout".
- Warm and direct. Not corporate. Not conference-speak.
- Address the reader as "you".

### Brand colors (INVIOLABLE)

```css
--cream:       #EBE5D8   /* main background */
--cream-light: #F5F1E8   /* card fills */
--dark:        #3A2A20   /* primary text */
--brown:       #857A6F   /* secondary text */
--gold:        #B8862E   /* eyebrow labels, accents */
--amber:       #C77832   /* logo amber notes */
--green:       #4A6B3A   /* positive, live status */
--red:         #8B3A2E   /* cost, warning */
--meetup-red:  #ED1C40   /* Meetup platform mark */
--line: rgba(58, 42, 32, 0.12);
```

Never introduce colors outside this palette.

### Typography
- Headings (h1, h2, h3): `Georgia, "Times New Roman", serif`
- Body: `-apple-system, BlinkMacSystemFont, "Segoe UI", system-ui, sans-serif`

## Modes of operation

### One-shot mode (DEFAULT, always try this first)

If `$ARGUMENTS` contains any topic or description, do NOT ask questions. Just:

1. Infer deck type from keywords (see "Type inference" below).
2. Load community defaults for everything not specified.
3. Load the logo (see "Logo embedding" below).
4. Build the deck using the type's default outline.
5. Save + report path.

If any of the following are actually needed but missing, ask ONE compact
question, not a series:
- Specific event date + venue (for event-announcement decks)
- Specific speaker name and topic (if this is a speaker-highlight deck)
- Sponsor tier prices (for sponsor pitch)
- Recap numbers: attendance, feedback stars (for post-event recap)

Everything else, use defaults. Do not ask about audience, chapter, format,
member counts, CTA URL, colors, or logo. All baked in.

### Type inference (choose one)

Match `$ARGUMENTS` against these keyword buckets. First match wins.

| Keywords in $ARGUMENTS | Type | Default outline |
|---|---|---|
| "recap", "after", "how it went", "went well" | post-event-recap | see below |
| "sponsor", "partnership", "sponsor pitch", "tiers" | sponsor-pitch | see below |
| "circle", "lunch", "dinner", "all-in" + date/venue | event-announcement | see below |
| "community update", "quarter", "state of" | community-update | see below |
| Anything else | community-overview | see below |

### Interview mode (fallback only)

Only enter this if `$ARGUMENTS` is empty AND the user just typed
`/aiandcoffeedeck` alone. Ask three questions in ONE message:
1. Deck type (1-5)
2. Topic in one sentence
3. Optional CTA URL if not the default Meetup URL

Then generate.

## Logo embedding (INVIOLABLE)

Every deck embeds the AI and Coffee logo as base64 PNG inside an `<img>` tag.
No text-only wordmark, no pill badges.

**Always use the transparent variant** (`aiandcoffee-logo.png`). It sits on any
slide background without a visible bounding box. Only fall back to the cream
or navy pre-baked variants if the transparent one is missing.

Before generating, run this from the shell to get the base64 data URI:

```bash
python3 -c "import base64,sys; p='$HOME/.claude/commands/aiandcoffee-logo.png'; print(f'data:image/png;base64,{base64.b64encode(open(p,\"rb\").read()).decode()}')"
```

Store the output as `LOGO_URI`. Use it in the HTML like so:

```html
<img class="logo logo-lg" src="{{LOGO_URI}}" alt="AI and Coffee">   <!-- cover -->
<img class="logo logo-sm" src="{{LOGO_URI}}" alt="AI and Coffee">   <!-- other slides -->
```

CSS:

```css
.logo { display: block; height: auto; }
.logo-lg { max-width: 540px; width: 55vw; margin-bottom: 20px; }
.logo-sm { max-width: 220px; width: 22vw; margin-bottom: 24px; }
```

The default logo PNG (`aiandcoffee-logo.png`) is transparent, so it blends on
any slide background. Do not add a wrapper or box behind it.

Only if the transparent file is missing (bad install), fall back to the
background-baked variant matching the slide background (cream, white, or
navy).

## Platform marks (only when a deck actually needs them)

**Meetup mark is NOT default.** Do not add "Powered by Meetup" captions on the
cover, and do not put the Meetup mark inside CTA buttons by default. Leave the
Meetup mention to the text of the URL/CTA label. If a specific deck needs it
(e.g., a dedicated "How we use Meetup" slide), use the mark below explicitly.

Meetup mark (available when explicitly needed):

```html
<svg width="20" height="20" viewBox="0 0 20 20" xmlns="http://www.w3.org/2000/svg">
  <rect width="20" height="20" rx="5" fill="#ED1C40"/>
  <text x="10" y="14.5" font-family="Arial, Helvetica, sans-serif" font-size="12" font-weight="900" fill="white" text-anchor="middle">M</text>
</svg>
```

## Other platform marks (add only when a deck asks for them)

- **LinkedIn.** `#0A66C2` rounded square, white "in".
- **Instagram.** `#E4405F` rounded square, white camera glyph.
- **YouTube.** `#FF0000` rounded rect, white play triangle.
- **X.** Black square, white X.
- **GitHub.** `#24292F` circle, white cat.

Rule. Platform monograms only. Never place a full third-party logotype next
to our logo without an explicit co-brand lockup.

## Default outlines per deck type

### community-overview (default, external audience, evergreen)

8 slides.
1. **Cover.** Logo large. Eyebrow "Community Overview. <date>. 5 chapters across Europe." Headline "Where AI-curious people meet over coffee." Lead sentence.
2. **Who we are.** Eyebrow "01. Who we are." Heading "A community, not a conference." Lead "No pitches. No lead generation. No badges on lanyards." Two cards: who shows up, how we run.
3. **By the numbers.** Eyebrow "02. By the numbers." Heading "Eight hundred members. Five chapters. Two years in." Stat grid: 800+ members, 5 chapters, 4 formats, 0 fee.
4. **Four ways we meet.** Eyebrow "03. Four ways we meet." Heading "Pick the one that fits your week." Numbered list of Circle, Lunch, Dinner, All-in.
5. **Where we are.** Eyebrow "04. Where we are." Heading "Five cities across Europe." Numbered list of chapters.
6. **The rhythm.** Eyebrow "05. The rhythm." Heading "Weekly cadence. Monthly headliners." Two cards.
7. **What you get.** Eyebrow "06. What you get." Heading "The network you actually keep." 4 bullets.
8. **Come in.** Logo small. Eyebrow "07. Come in." Heading "First event is on us." CTA with Meetup mark.

### event-announcement (upcoming event)

6-7 slides.
1. Cover. Logo. "Format. Date. City."
2. What this is. One paragraph on the format.
3. Speaker or theme (if applicable).
4. Agenda. Numbered.
5. Venue and time. Two cards: address + how to get there.
6. RSVP. CTA to specific event URL.

### post-event-recap

6 slides.
1. Cover. Logo. "Recap. Event. Date."
2. Numbers. Stat grid (attendance, RSVPs, rating, first-timers).
3. What went well.
4. What we heard.
5. Photos or a member quote.
6. Next up. CTA to next event.

### sponsor-pitch

7-8 slides.
1. Cover. Logo. "Sponsor Overview. <date>."
2. Who we are.
3. Numbers. Stat grid (members, chapters, events/year, engagement).
4. Formats.
5. Chapters.
6. Sponsorship tiers. Cards or grid.
7. Closing. Contact CTA.

### community-update (state of the community)

6 slides.
1. Cover. Logo. "State of the community. Month YYYY."
2. Numbers this quarter.
3. Highlights.
4. Roadmap.
5. Thanks.
6. What is next.

## Complete HTML template

Fill placeholders. Use the slide patterns below to assemble `{{SLIDES}}`.
The `{{LOGO_URI}}` placeholder gets the base64 data URI from the shell command
above.

```html
<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>{{TITLE}}. AI and Coffee.</title>
<style>
  :root {
    --cream: #EBE5D8;
    --cream-light: #F5F1E8;
    --dark: #3A2A20;
    --brown: #857A6F;
    --gold: #B8862E;
    --amber: #C77832;
    --green: #4A6B3A;
    --red: #8B3A2E;
    --meetup-red: #ED1C40;
    --line: rgba(58, 42, 32, 0.12);
  }
  * { box-sizing: border-box; margin: 0; padding: 0; }
  html, body { height: 100%; overflow: hidden; }
  body {
    font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", system-ui, sans-serif;
    background: var(--cream);
    color: var(--dark);
    line-height: 1.6;
    cursor: pointer;
  }
  section.slide {
    position: fixed; top: 0; left: 0;
    width: 100vw; height: 100vh;
    padding: 6vh 8vw;
    display: none;
    flex-direction: column;
    justify-content: center;
    overflow-y: auto;
    opacity: 0;
    transition: opacity 0.3s ease;
  }
  section.slide.active { display: flex; opacity: 1; }
  .inner { max-width: 980px; width: 100%; margin: 0 auto; }
  h1, h2, h3 { font-family: Georgia, "Times New Roman", serif; color: var(--dark); font-weight: normal; }
  h1 { font-size: clamp(36px, 5.5vw, 64px); line-height: 1.1; margin-bottom: 16px; letter-spacing: -0.02em; }
  h2 { font-size: clamp(28px, 4vw, 44px); line-height: 1.15; margin-bottom: 12px; letter-spacing: -0.01em; }
  h3 { font-size: 22px; margin-bottom: 8px; }
  p { font-size: 17px; color: var(--dark); margin-bottom: 16px; }
  .lead { font-size: clamp(17px, 1.8vw, 22px); color: var(--brown); margin-bottom: 32px; }
  .eyebrow { text-transform: uppercase; font-size: 12px; letter-spacing: 0.18em; color: var(--gold); font-weight: 600; margin-bottom: 24px; }
  .logo { display: block; height: auto; }
  .logo-lg { max-width: 380px; width: 42vw; margin-bottom: 32px; }
  .logo-sm { max-width: 160px; width: 16vw; margin-bottom: 24px; }
  .card { background: var(--cream-light); border: 1px solid var(--line); border-radius: 12px; padding: 24px; margin-bottom: 16px; }
  .grid-2 { display: grid; grid-template-columns: 1fr 1fr; gap: 16px; }
  .grid-3 { display: grid; grid-template-columns: repeat(3, 1fr); gap: 16px; }
  @media (max-width: 720px) { .grid-2, .grid-3 { grid-template-columns: 1fr; } }
  ul.clean { list-style: none; padding: 0; }
  ul.clean li { padding: 14px 0; border-bottom: 1px solid var(--line); font-size: 16px; display: flex; align-items: flex-start; gap: 14px; }
  ul.clean li:last-child { border-bottom: none; }
  .num { flex-shrink: 0; width: 30px; height: 30px; border-radius: 50%; background: var(--dark); color: var(--cream); font-family: Georgia, serif; font-size: 14px; display: inline-flex; align-items: center; justify-content: center; }
  .item-title { font-weight: 600; color: var(--dark); }
  .item-desc { color: var(--brown); font-size: 14px; margin-top: 2px; }
  .stat { text-align: center; padding: 16px; }
  .stat-num { font-family: Georgia, serif; font-size: 56px; color: var(--gold); line-height: 1; }
  .stat-label { font-size: 13px; color: var(--brown); margin-top: 8px; text-transform: uppercase; letter-spacing: 0.1em; }
  .progress { position: fixed; top: 0; left: 0; height: 3px; background: var(--gold); transition: width 0.3s ease; z-index: 100; }
  .counter { position: fixed; bottom: 20px; right: 24px; background: var(--dark); color: var(--cream); padding: 6px 14px; border-radius: 999px; font-size: 12px; letter-spacing: 0.05em; font-family: Georgia, serif; z-index: 100; opacity: 0.8; }
  .hint { position: fixed; bottom: 20px; left: 24px; color: var(--brown); font-size: 11px; letter-spacing: 0.1em; text-transform: uppercase; z-index: 100; opacity: 0.6; }
  a.cta { display: inline-flex; align-items: center; gap: 12px; background: var(--dark); color: var(--cream); padding: 18px 34px; border-radius: 999px; text-decoration: none; font-size: 17px; margin-top: 24px; letter-spacing: 0.02em; transition: transform 0.15s ease, background 0.2s ease, box-shadow 0.2s ease; box-shadow: 0 4px 14px rgba(58, 42, 32, 0.15); font-weight: 500; }
  a.cta:hover { background: var(--amber); transform: translateY(-1px); box-shadow: 0 6px 20px rgba(199, 120, 50, 0.25); }
  a.cta .arrow { display: inline-block; transition: transform 0.2s ease; }
  a.cta:hover .arrow { transform: translateX(4px); }
  blockquote { font-family: Georgia, serif; font-style: italic; color: var(--brown); font-size: clamp(20px, 2.5vw, 28px); line-height: 1.4; border-left: 3px solid var(--gold); padding-left: 24px; margin: 24px 0; }
</style>
</head>
<body>
<div class="progress" id="progress"></div>
<div class="counter" id="counter">1 / {{COUNTER_MAX}}</div>
<div class="hint">Space, click, arrows to navigate</div>

{{SLIDES}}

<script>
  const slides = document.querySelectorAll('.slide');
  const counter = document.getElementById('counter');
  const progress = document.getElementById('progress');
  let current = 0;
  function go(to) {
    if (to < 0 || to >= slides.length) return;
    slides[current].classList.remove('active');
    current = to;
    slides[current].classList.add('active');
    slides[current].scrollTop = 0;
    counter.textContent = (current + 1) + ' / ' + slides.length;
    progress.style.width = ((current + 1) / slides.length * 100) + '%';
  }
  document.addEventListener('keydown', (e) => {
    if (e.key === 'ArrowRight' || e.key === ' ' || e.key === 'PageDown') { e.preventDefault(); go(current + 1); }
    else if (e.key === 'ArrowLeft' || e.key === 'PageUp') { e.preventDefault(); go(current - 1); }
    else if (e.key === 'Home') { go(0); }
    else if (e.key === 'End') { go(slides.length - 1); }
  });
  document.addEventListener('click', (e) => {
    if (e.target.closest('a, button, code')) return;
    go(current + 1);
  });
  document.addEventListener('contextmenu', (e) => { e.preventDefault(); go(current - 1); });
  go(0);
</script>
</body>
</html>
```

## Slide patterns

### Cover slide (always first)

```html
<section class="slide active">
  <div class="inner">
    <img class="logo logo-lg" src="{{LOGO_URI}}" alt="AI and Coffee">
    <p class="eyebrow">{{TYPE_LABEL}}. {{DATE}}. {{CHAPTER}}.</p>
    <h1>{{TITLE}}</h1>
    <p class="lead">{{LEAD}}</p>
  </div>
</section>
```

### Content slide with numbered list

```html
<section class="slide">
  <div class="inner">
    <p class="eyebrow">{{EYEBROW}}</p>
    <h2>{{HEADING}}</h2>
    <p class="lead">{{LEAD}}</p>
    <ul class="clean" style="margin-top: 24px;">
      <li><span class="num">1</span><div style="flex:1;"><div class="item-title">{{ITEM_TITLE}}</div><div class="item-desc">{{ITEM_DESC}}</div></div></li>
    </ul>
  </div>
</section>
```

### Two-column card slide

```html
<section class="slide">
  <div class="inner">
    <p class="eyebrow">{{EYEBROW}}</p>
    <h2>{{HEADING}}</h2>
    <div class="grid-2" style="margin-top:24px;">
      <div class="card"><h3>{{A_TITLE}}</h3><p style="color:var(--brown);">{{A_BODY}}</p></div>
      <div class="card"><h3>{{B_TITLE}}</h3><p style="color:var(--brown);">{{B_BODY}}</p></div>
    </div>
  </div>
</section>
```

### Stat grid

```html
<section class="slide">
  <div class="inner">
    <p class="eyebrow">{{EYEBROW}}</p>
    <h2>{{HEADING}}</h2>
    <div class="grid-2" style="margin-top:24px;">
      <div class="stat"><div class="stat-num">{{N1}}</div><div class="stat-label">{{L1}}</div></div>
      <div class="stat"><div class="stat-num">{{N2}}</div><div class="stat-label">{{L2}}</div></div>
      <div class="stat"><div class="stat-num">{{N3}}</div><div class="stat-label">{{L3}}</div></div>
      <div class="stat"><div class="stat-num">{{N4}}</div><div class="stat-label">{{L4}}</div></div>
    </div>
  </div>
</section>
```

### Quote

```html
<section class="slide">
  <div class="inner">
    <p class="eyebrow">{{EYEBROW}}</p>
    <blockquote>{{QUOTE}}</blockquote>
    <p style="color:var(--brown);font-size:15px;">{{ATTRIBUTION}}</p>
  </div>
</section>
```

### Closing / CTA (always last)

```html
<section class="slide">
  <div class="inner">
    <img class="logo logo-sm" src="{{LOGO_URI}}" alt="AI and Coffee">
    <p class="eyebrow">{{EYEBROW}}</p>
    <h2>{{CLOSING_HEADING}}</h2>
    <p class="lead">{{CLOSING_BODY}}</p>
    <a class="cta" href="{{CTA_URL}}">
      {{CTA_LABEL}}
      <span class="arrow">&rarr;</span>
    </a>
  </div>
</section>
```

## Generation process

### One-shot (default)

1. Get logo data URI. Run the base64 shell command from the Logo section.
2. Infer deck type from `$ARGUMENTS` keywords.
3. Pick the default outline for that type.
4. Fill placeholders using community defaults + anything specific in `$ARGUMENTS`.
5. Slugify title. Save JSON to `<data_dir>/<slug>-<yyyy-mm-dd>.json`.
6. Render HTML to `<output_dir>/<slug>-<yyyy-mm-dd>.html`.
7. Report:
   - Absolute path
   - Suggested `open "<path>"` command
   - One-line summary of the outline used

### When to break out and ask

Only ask if the deck TYPE requires specific info the defaults cannot fill:
- event-announcement: date + venue + optional speaker
- sponsor-pitch: tier prices (else use TBD)
- post-event-recap: attendance + rating (else skip stats slide)

Ask ALL missing info in ONE compact message with numbered questions.
Never sequential questions.

## Rules

ALWAYS:
- Embed the logo via base64 data URI. Real image, not text wordmark.
- Save to `output_dir`. Report absolute path.
- Suggest `open "<path>"` after saving.
- Voice check every string for em-dashes / en-dashes before saving.
- Use the Meetup mark inside every CTA that links to Meetup.
- Include cover + closing at minimum. Content 3 to 8 slides in between.

NEVER:
- Ask multi-turn questions in one-shot mode.
- Use text-only wordmark ("AI & Coffee" spans in HTML). Only the embedded PNG.
- Fabricate specific stats (dates, attendance, tier prices). Ask instead.
- Add gradients, drop shadows, decorative emoji as UI chrome.
- Send, upload, or post the deck anywhere.
- Reference external CSS/JS/images. Everything inline or base64.

## Example invocations (real one-shots)

```
/aiandcoffeedeck lets present ai and coffee at an event
# -> community-overview, defaults, 8 slides, generated silently

/aiandcoffeedeck circle sunday 13 july cafe bora luxembourg
# -> event-announcement. Missing speaker. Assumed no speaker, generate 6 slides.

/aiandcoffeedeck sponsor pitch for a fintech partner
# -> sponsor-pitch. Asks tier prices in one compact message, then generates.

/aiandcoffeedeck recap of the may all-in with alan nicholas
# -> post-event-recap. Asks attendance + rating, then generates.

/aiandcoffeedeck q2 state of the community
# -> community-update. 6 slides, asks for quarter numbers, generates.
```

## For team members

- **Logo files.** `~/.claude/commands/aiandcoffee-logo.png` (transparent, default) plus `-transparent`, `-cream`, `-white`, `-navy` variants. Copy from the brand-kit repo.
- **Updating the skill.** Pull latest from GitHub, re-cp the .md to `~/.claude/commands/`.
- **Suggesting changes.** Open a PR on the brand-kit repo.

## Input

$ARGUMENTS
