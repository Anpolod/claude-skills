---
name: asap-crewing-design
description: >
  Apply the ASAP Crewing CRM design system "Navigational Silence" to any
  frontend component, page, or layout. Use when building or modifying UI
  for the MailSenderZilla / ASAP Crewing CRM — sailors list, profile pages,
  campaigns, modals, forms, tables, badges, or any interface element.
  Triggers on: "in our style", "match the mockup", "same design system",
  "CRM component", "add to the project".
---

# ASAP Crewing — Design System "Navigational Silence"

Near-monochrome maritime aesthetic. Flat, clean, no gradients, no shadows.
Zero decorative effects. Data-dense but airy.

## Fonts
Load via Google Fonts:
```
Bricolage Grotesque — headings, large numbers, modal titles
Instrument Sans    — all UI text, labels, buttons, table cells  
JetBrains Mono     — dates, phones, codes, badges, field labels
```

## CSS Tokens
```css
:root {
  --bg:      #faf9f6;
  --white:   #ffffff;
  --surface: #F0EFEA;
  --border:  #E2E0D9;
  --text-1:  #0D0D0B;
  --text-2:  #46463F;
  --text-3:  #9C9B95;
  --accent:  #185FA5;

  /* Status */
  --active-bg:     #EAF3DE; --active-text:    #2E6209;
  --bounced-bg:    #FCEBEB; --bounced-text:   #9B2B2B;
  --inactive-bg:   #F0EFEA; --inactive-text:  #9C9B95;
  --expiring-bg:   #FAEEDA; --expiring-text:  #B37216;
  --scheduled-bg:  #E6F1FB; --scheduled-text: #185FA5;
  --draft-bg:      #F0EFEA; --draft-text:     #9C9B95;
}
```

## Avatar Colors (by charCode(name[0]) % 6)
```
0: bg #E6F1FB  text #0C447C
1: bg #EAF3DE  text #27500A
2: bg #FAEEDA  text #633806
3: bg #EEEDFE  text #3C3489
4: bg #E1F5EE  text #085041
5: bg #FAECE7  text #712B13
```

## Border Radius
```
inputs / buttons / selects : 5px
badges / pills             : 3px
cards / modals             : 12px
```

## Borders
All borders: `0.5px solid #E2E0D9`  
Never use 1px borders.

## Components

### Button variants
```css
/* Ghost */
.btn { height:32px; padding:0 14px; border:0.5px solid #E2E0D9;
       border-radius:5px; background:#fff; font-family:'Instrument Sans';
       font-size:12px; color:#46463F; cursor:pointer; }
.btn:hover { background:#F0EFEA; }

/* Primary */
.btn-primary { background:#0D0D0B; color:#fff; border-color:#0D0D0B; }
.btn-primary:hover { opacity:.88; }

/* Danger */
.btn-danger { border-color:#F09595; color:#9B2B2B; }
.btn-danger:hover { background:#FCEBEB; }
```

### Form input / select
```css
input, select {
  height: 34px; width: 100%;
  background: #F0EFEA;
  border: 0.5px solid #E2E0D9;
  border-radius: 5px;
  padding: 0 10px;
  font-family: 'Instrument Sans'; font-size: 12px; color: #0D0D0B;
  outline: none;
}
input:focus, select:focus {
  border-color: #185FA5;
  background: #fff;
}
```

### Field label
```css
label {
  font-family: 'JetBrains Mono'; font-size: 10px; color: #9C9B95;
  display: block; margin-bottom: 4px;
}
```

### Status badge
```html
<span class="badge badge-active">active</span>
<!-- sentence case, no uppercase, no dot indicator -->
```
```css
.badge { font-family:'JetBrains Mono'; font-size:10px; font-weight:700;
         padding:2px 8px; border-radius:3px; display:inline-block; }
.badge-active   { background:#EAF3DE; color:#2E6209; }
.badge-bounced  { background:#FCEBEB; color:#9B2B2B; }
.badge-inactive { background:#F0EFEA; color:#9C9B95; border:0.5px solid #E2E0D9; }
.badge-expiring { background:#FAEEDA; color:#B37216; }
```

### Avatar (initials circle)
```html
<div class="avatar" style="background:#E1F5EE; color:#085041;">AZ</div>
```
```css
.avatar {
  width:36px; height:36px; border-radius:50%;
  display:flex; align-items:center; justify-content:center;
  font-family:'JetBrains Mono'; font-size:11px; font-weight:700;
}
```

### Table
```css
table { width:100%; border-collapse:collapse; }
thead tr { background:#F0EFEA; height:34px; }
th { font-family:'JetBrains Mono'; font-size:10px; color:#9C9B95;
     font-weight:500; padding:0 16px; /* NO uppercase */ }
tbody tr { height:52px; border-bottom:0.5px solid #E2E0D9; }
tbody tr:nth-child(odd)  { background:#fff; }
tbody tr:nth-child(even) { background:#FAFAF7; }
tbody tr:hover { background:#F0EFEA; }
/* Edit/Del: opacity:0 on row, opacity:1 on hover */
```

### Card
```css
.card { background:#fff; border:0.5px solid #E2E0D9; border-radius:12px; overflow:hidden; }
.card-header {
  height:42px; display:flex; align-items:center; justify-content:space-between;
  padding:0 16px; border-bottom:0.5px solid #E2E0D9;
}
/* Card title: "▸ " prefix + Instrument Sans 11px 500 #0D0D0B, sentence case */
```

### Page header (58px sticky)
```css
.page-header {
  height:58px; background:#fff; border-bottom:0.5px solid #E2E0D9;
  display:flex; align-items:center; justify-content:space-between;
  padding:0 24px; position:sticky; top:0; z-index:50;
}
```

### Tab navigation
```css
.tabs { display:flex; height:44px; gap:32px; border-bottom:0.5px solid #E2E0D9; }
.tabs a { height:100%; display:flex; align-items:center; font-size:13px;
          color:#9C9B95; text-decoration:none; font-family:'Instrument Sans'; }
.tabs a:hover { color:#0D0D0B; }
.tabs a.active { font-weight:700; color:#0D0D0B; border-bottom:2px solid #0D0D0B; }
/* Active underline is BLACK (#0D0D0B), never blue */
```

### Modal
```css
.modal-overlay { position:fixed; inset:0; background:rgba(13,13,11,0.45);
                 display:flex; align-items:center; justify-content:center; z-index:9999; }
.modal { background:#fff; border:0.5px solid #E2E0D9; border-radius:12px; width:520px; }
.modal-header { height:52px; display:flex; align-items:center;
                justify-content:space-between; padding:0 20px;
                border-bottom:0.5px solid #E2E0D9; }
.modal-title { font-family:'Bricolage Grotesque'; font-size:16px;
               font-weight:700; color:#0D0D0B; }
.modal-footer { padding:14px 20px; border-top:0.5px solid #E2E0D9;
                display:flex; justify-content:flex-end; gap:8px; }
```

### Sidebar (fixed, dark)
```css
#sidebar { width:220px; height:100vh; background:#111827;
           position:fixed; left:0; top:0; }
/* Nav items: Instrument Sans 13px #9CA3AF, hover bg #1F2937, radius 6px */
/* Active:    bg #2563EB, color #fff, font-weight 600 */
/* Logo:      Bricolage Grotesque 17px bold white */
```

### Metric stat card (for Certificates, Sea Service, Campaigns)
```css
.stat-card { border:0.5px solid #E2E0D9; border-radius:8px; padding:12px 16px; }
/* Number: Bricolage Grotesque 28px bold */
/* Label:  JetBrains Mono 11px #9C9B95 */
```

### Toolbar (above table, inside card)
```css
.toolbar { background:#FAFAF7; border-bottom:0.5px solid #E2E0D9;
           padding:8px 16px; display:flex; align-items:center; gap:8px; }
/* Filter pills: height 30px, bg #F0EFEA, border 0.5px #E2E0D9, radius 5px */
/* Active pill:  bg #0D0D0B, color #fff */
```

### Pagination
```css
/* Per-page: segmented [10|20|50|100], active bg #0D0D0B text #fff */
/* Page btn: 32px sq, border 0.5px #E2E0D9, radius 4px */
/* Active page: bg #0D0D0B text #fff */
/* Info: JetBrains Mono 10px #9C9B95 */
```

## Hard Rules — NEVER violate
1. No gradients anywhere
2. No box-shadows (only focus rings allowed)
3. No backdrop-blur / blur effects  
4. No uppercase on card titles, table headers, tab labels
5. No rounded-full on status badges — always border-radius: 3px
6. No dot indicators inside badges
7. No Material Icons — use text buttons (Edit / Del) with borders
8. No italic text in Notes/comments fields
9. No font weights above 700 (no font-black, font-extrabold in UI)
10. All borders exactly 0.5px — never 1px
11. Active tab underline is #0D0D0B (black), never blue
12. Edit/Del actions always hidden, reveal only on row hover
