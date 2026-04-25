# Telegram formatting notes

When the user pastes the digest into Telegram, some markdown survives and
some does not. Keep these in mind when drafting.

## What works
- `**bold**` → renders as bold in Telegram's MarkdownV2 when pasted via
  a bot. In the native Telegram composer, `**bold**` does NOT render —
  user will need to use Telegram's own bold (Ctrl+B) after pasting.
- Plain URLs become clickable links automatically.
- `_italic_` — same caveat as bold.
- Line breaks render fine.

## What does NOT work
- Markdown headers (`#`, `##`). Telegram ignores them. The emoji at the
  start of the H2 line is what gives visual structure after paste.
- Nested lists with `-` indentation — collapse to flat.
- Tables — never render. Do not use.
- Link syntax `[text](url)` — renders as literal text in the native
  composer. Bare URLs are more reliable.

## Channel-friendly conventions
- Keep each item self-contained. Telegram posts are often read out of
  order via search.
- Section emoji at the start of each beat is a visual anchor (🛳️ 🛢️
  ⚙️ 👨‍✈️ 🌍 🚢).
- One blank line between items, two between sections.
- Channel posts have a 4,096 character hard limit. Our typical digest
  (6 beats × 3 items × ~250 chars) lands around 4,500 — the user will
  split into 2 posts or trim. Don't optimize for the limit in the draft;
  the user handles this at publish time.

## User post hygiene (for reference, not the skill's job)
- User typically adds a channel header line, a hashtag block at the end
  (`#shipping #offshore #crewing`), and sometimes a "Read more ↓"
  inline button. The skill produces the raw body only.
