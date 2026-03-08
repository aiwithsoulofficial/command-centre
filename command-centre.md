---
name: command-centre
description: Generate an ADHD-friendly branded Command Centre HTML dashboard from any project folder. Scans for content calendars, email sequences, brand guides, and services - then outputs a single interactive HTML file with localStorage persistence. Use when you say command centre, build a dashboard, content tracker, or project command centre.
---

# COMMAND CENTRE GENERATOR

## What This Skill Does

Drop this skill into Claude Code and you can generate a fully branded, ADHD-friendly HTML dashboard from any project folder in seconds.

Give Claude a folder path (or a single brief file) and it will:

- Scan everything in the folder for project data
- Extract your brand colours, content plan, email sequence, services, and voice guide
- Generate one self-contained HTML file with no external dependencies
- Open it in your browser, ready to use

The dashboard tracks what you have done and what is next, persists progress in localStorage, and adapts its sections to only show what exists in your project. No fluff. No filler.

---

## Installation

1. Create the folder `~/.claude/skills/command-centre/`
2. Save this file as `~/.claude/skills/command-centre/SKILL.md`
3. Restart Claude Code (or reload skills if your setup supports hot-loading)
4. Use it by typing `/command-centre` followed by your project folder path

---

## HOW TO USE

```
/command-centre ~/path/to/your-project-folder
/command-centre ~/path/to/your-project-folder --output ~/Downloads/dashboard.html
/command-centre ~/path/to/brief.md --name "Your Name" --brand-name "Brand Name"
```

### Flags

- `--output [path]` - Override output location (default: same folder as input, named `COMMAND-CENTRE.html`)
- `--name [name]` - Client or user first name (auto-detected from brief if not provided)
- `--brand-name [name]` - Brand name (auto-detected from brief if not provided)
- `--colours [hex,hex,hex,hex]` - Override brand colours: bg, accent, cta, text (auto-detected from brief if not provided)

---

## PROCEDURE

### STEP 1: SCAN THE PROJECT FOLDER

Read ALL available files to extract data. Look for:

1. **Brand brief / CLAUDE.md / README** - client name, brand name, colours, typography, voice
2. **Content calendar** (`.md`, `.xlsx`, `.csv`) - post schedule, platforms, captions, pillar categories
3. **Email sequence** (`.md`, `.docx`) - subject lines, send days, email count
4. **SEO strategy** (`.md`, `.xlsx`) - keyword count, page targets
5. **Proposals** (`.md`, `.docx`) - services, pricing
6. **Brand voice guide** (`.md`) - tone attributes, words to use/avoid
7. **Assets folder** - logo, mood board, avatar images

Build a data inventory of what exists. Not every project will have all pieces - the dashboard adapts to what is available.

### STEP 2: EXTRACT KEY DATA

From the scanned files, extract:

| Data Point | Source | Fallback |
|-----------|--------|----------|
| Client/user name | Brief / folder name | Ask user |
| Brand name | Brief | Folder name |
| Brand colours (bg, accent, cta, text) | Brief / brand guide | Sage #8FA89A, Copper #C4836A, Linen #F5F0EB, Charcoal #3B3F45 |
| Content calendar posts | Calendar file | Empty section with setup prompt |
| Post pillars + colour coding | Calendar / brief | Generic categories |
| Email sequence | Email file | Empty section |
| Services + pricing | Proposal / brief | Hidden section |
| Brand voice attributes | Voice guide / brief | Hidden section |
| SEO page targets | SEO strategy | Hidden section |

### STEP 3: GENERATE THE HTML

Build a single HTML file with ALL CSS in `<style>` and ALL JS in `<script>`. No external dependencies except Google Fonts (Outfit + JetBrains Mono).

**Design specs:**
- Light mode only
- Mobile responsive (single column under 768px)
- ADHD-friendly: big text for key info, clear visual hierarchy, no walls of text
- Clean, calm, professional
- NEVER use emdashes - hyphens with spaces only

**Required sections (include ALL that have data):**

#### 1. HEADER
- Title: "[Name]'s Command Centre"
- Subtitle: "[Brand Name] - Your ADHD-friendly content brain"
- Auto date via JS
- Fun tagline generated based on the project niche

#### 2. RIGHT NOW (hero card)
- Gradient top border using brand accent + CTA colours
- Shows the NEXT unposted content piece from the calendar
- Day/week number, post type, platform, caption preview, best posting time
- Big "Mark as Done" button that advances to the next unposted item
- If all done, show a congratulatory message

#### 3. SCOREBOARD
- One stat card per trackable category (posts, emails, SEO pages, proposals - only show what exists)
- Each card: count done/total, progress bar in brand colour
- Auto-updates when items are toggled anywhere in the dashboard

#### 4. CONTENT CALENDAR (collapsible)
- Grid layout showing all days in the plan
- Each day cell: day number, platform tag, post type, pillar colour dot
- Done days faded with checkmark
- Rest days greyed out
- Click to toggle done status
- Week labels if calendar spans multiple weeks

#### 5. EMAIL SEQUENCE (collapsible, only if emails exist)
- Card per email: number, subject line, send day, sent/pending toggle
- Click to toggle sent status

#### 6. CONTENT PILLARS (only if pillars defined)
- Horizontal bars showing percentage split
- Colour-coded per pillar
- Shows done vs remaining count

#### 7. QUICK WINS (collapsible)
- Header: "Forgot what you're doing? Start here"
- Toggleable checklist of micro-tasks, customised to the project workflow:
  - Find templates / assets
  - Check which day you're up to
  - Copy today's caption
  - Post at optimal time
  - Engage (reply to comments/DMs)
  - Save insights / screenshots
- Additional items based on the project specifics

#### 8. SERVICES CHEAT SHEET (only if pricing exists, collapsible)
- Compact cards: service name, price
- "Copy" button per item (copies to clipboard)

#### 9. BRAND VOICE REMINDER (only if voice guide exists, collapsible)
- Voice DNA ranked attributes
- Words we USE (green tags)
- Words we NEVER USE (red tags)
- Tone check question

#### 10. FOOTER
- "Built with Claude Code"
- Credit line: "by AI with Soul | aiwithsoul.com.au"

### JS REQUIREMENTS (vanilla, no frameworks)

1. **localStorage persistence** - all toggle states saved under key `cc-[brand-name-slug]`
2. **Auto-updating scoreboard** - recalculates on every toggle
3. **Collapsible sections** - click header to expand/collapse
4. **Hero auto-advance** - "Mark as Done" moves to next unposted item
5. **Copy-to-clipboard** - on services prices with "Copied!" feedback
6. **Dynamic date** - current date shown in header

### STEP 4: SAVE AND OPEN

1. Save the HTML file to the output path
2. Open it in the browser: `open "[output-path]"`
3. Report back:

```
Command Centre built.
Project:  [name]
Brand:    [brand name]
Output:   [file path]
Sections: [list of included sections]
Posts tracked: [N]
Emails tracked: [N]
```

---

## COLOUR SYSTEM

The dashboard uses CSS custom properties for easy theming. Map brand colours to these roles:

```css
:root {
  --bg: [lightest colour - background];
  --surface: #FFFFFF;
  --border: [mid-light colour - borders/dividers];
  --border-light: [lighter version of border];
  --text: [dark colour - body text];
  --text-dark: [darkest colour - headlines];
  --text-dim: [muted version of text];
  --accent: [primary brand colour - buttons, headers, progress bars];
  --accent-bg: [accent at 8% opacity];
  --accent-border: [accent at 25% opacity];
  --cta: [secondary/CTA colour - highlights, hover states];
  --cta-bg: [CTA at 8% opacity];
  --cta-border: [CTA at 25% opacity];
}
```

If the project has fewer than 4 colours, generate complementary shades from what is available. Use the brand's primary colour as `--accent` and derive a warm or contrasting tone for `--cta`.

---

## ADAPTIVE SECTIONS

The dashboard only shows sections that have data. Use this logic:

| Section | Show if... |
|---------|-----------|
| Header | Always |
| Right Now | Content calendar exists |
| Scoreboard | Any trackable items exist |
| Content Calendar | Calendar data found |
| Email Sequence | Email sequence found |
| Content Pillars | Pillar categories defined |
| Quick Wins | Always (customise items to project) |
| Services | Pricing data found |
| Brand Voice | Voice guide found |
| Footer | Always |

If a project only has a content calendar and brand voice, the dashboard shows: Header + Right Now + Scoreboard + Calendar + Quick Wins + Voice + Footer. Clean and focused. No empty sections. No clutter.

---

## EDGE CASES

- **No content calendar found:** Show an empty calendar with a message: "No content plan loaded yet. Add a content-calendar.md to your project folder."
- **Excel/XLSX files:** Read them with a tool if available, otherwise prompt the user to export as CSV or MD before running.
- **Very large calendars (90+ days):** Group by month instead of week, show month tabs.
- **Multiple brands in one folder:** Ask the user which brand to build for before proceeding.
- **Project folder is just a single brief.md:** Extract everything from that one file - it is enough to generate a full dashboard.

---

## TROUBLESHOOTING

| Issue | Fix |
|-------|-----|
| Colours look wrong | Check hex values - ensure they include the `#` prefix |
| localStorage not persisting | Check browser privacy settings - some block localStorage on `file://` URLs. Try opening from a local server instead. |
| Calendar grid misaligned | Ensure rest days are included in the grid count |
| Sections won't collapse | Check JS toggle function targets the correct element IDs |
| Dashboard opens blank | Check the output path - the file may have saved to an unexpected location |

---

## EXAMPLE USAGE

```
/command-centre ~/Projects/luna-health/
```

Claude will scan the folder, find a brand brief, a 30-day content calendar, and a 5-email welcome sequence, then generate:

`~/Projects/luna-health/COMMAND-CENTRE.html`

Open automatically in browser. Done.

---

Created by AI with Soul | aiwithsoul.com.au
Free forever. Go build something beautiful.
