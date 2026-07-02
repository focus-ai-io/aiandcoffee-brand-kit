# AI and Coffee brand kit

Everything you need to generate on-brand AI and Coffee presentations from Claude Code.

## What's in here

- `aiandcoffeedeck.md`. The Claude Code slash command. Handles the whole flow: interviews if needed, embeds the logo, generates a self-contained HTML deck with slide-mode navigation.
- `logos/`. Seven logo variants of the AI and Coffee wordmark. Three are pre-named for direct use by the skill (`aiandcoffee-logo-cream.png`, `-white.png`, `-navy.png`).

## Install (2 minutes)

Requires [Claude Code](https://docs.claude.com/en/docs/claude-code) installed.

```bash
git clone https://github.com/focus-ai-io/aiandcoffee-brand-kit.git /tmp/aic-kit
cp /tmp/aic-kit/aiandcoffeedeck.md ~/.claude/commands/aiandcoffeedeck.md
cp /tmp/aic-kit/logos/aiandcoffee-logo-*.png ~/.claude/commands/
```

Restart Claude Code. Done.

## How to use

Open Claude Code in any folder and type:

```
/aiandcoffeedeck <topic>
```

Examples:

```
/aiandcoffeedeck present ai and coffee to a new audience
/aiandcoffeedeck circle sunday 13 july cafe bora luxembourg
/aiandcoffeedeck sponsor pitch for a fintech partner
/aiandcoffeedeck recap of the may all-in
```

The skill infers the deck type from your topic, uses baked-in community defaults (800+ members, 5 chapters, 4 formats, Meetup Pro), embeds the logo, and saves an HTML file. It asks for missing critical info in one compact message. It does not ask about brand colors, fonts, chapter names, or the Meetup URL. All of that is baked in.

Open the generated file in any browser. Navigate with Space, arrows, or click. Right-click for previous slide.

## Deck types the skill knows

| Trigger keywords | Type | Length |
|---|---|---|
| "recap", "how it went" | Post-event recap | 6 slides |
| "sponsor", "partnership" | Sponsor pitch | 7-8 slides |
| Format + date + venue | Event announcement | 6-7 slides |
| "community update", "state of" | Community update | 6 slides |
| Anything else | Community overview | 8 slides |

## Brand facts baked into every deck

- **Members.** 800+ across Europe.
- **Chapters.** Luxembourg (flagship, 600+), Vienna, Bucharest, Paris, plus one more joining soon.
- **Formats.** Circle (Sunday), Lunch (Wednesday), Dinner (monthly), All-in (monthly headliner).
- **Rhythm.** Weekly Circle + Lunch. Monthly Dinner + All-in.
- **Cost.** Free to attend. Always.
- **Platform.** Meetup Pro. Default CTA links to `meetup.com/aiandcoffee-luxembourg-en`.
- **Tagline.** "Where AI-curious people meet over coffee."

## Brand colors

```
--cream:       #EBE5D8   /* main background */
--cream-light: #F5F1E8   /* card fills */
--dark:        #3A2A20   /* primary text */
--brown:       #857A6F   /* secondary text */
--gold:        #B8862E   /* accents */
--amber:       #C77832   /* logo amber accents */
--meetup-red:  #ED1C40   /* Meetup platform mark */
```

## Voice rules

- No em-dashes or en-dashes. Ever. Use periods, commas, or restructure.
- Sentences over paragraphs.
- Numbers over adjectives. "42 RSVPs" beats "great turnout".
- Warm and direct. Not corporate.

## Fonts

Headings use `Georgia, "Times New Roman", serif`. Body uses system UI. This keeps decks self-contained (no external font requests).

The primary display font per brandbook is [Fraunces](https://fonts.google.com/specimen/Fraunces). Georgia is the fallback. If you want the true Fraunces look, add the font file to the deck manually.

## Logo variants

- **`aiandcoffee-logo-cream.png`** (aka `logos/1.png`). Cream background. **Default. Use this one.** Matches the brand cream `#EBE5D8`.
- **`aiandcoffee-logo-white.png`** (aka `logos/2.png`). White background. For pure-white surfaces.
- **`aiandcoffee-logo-navy.png`** (aka `logos/3.png`). Navy background. For dark accent slides.
- `logos/4.png` through `logos/7.png`. Additional color variants.

The skill auto-selects the cream variant. To use a different variant, edit the `LOGO_URI` step in the skill.

## Updating

- **Skill.** Pull latest from this repo, re-copy the `.md` to `~/.claude/commands/`.
- **Logos.** Pull latest from this repo, re-copy the `.png` files to `~/.claude/commands/`.

## Contributing

Open a PR. Keep the voice rules in mind. No em-dashes.

## License

Internal to the AI and Coffee organizing team. Not for public distribution of the brand assets.
