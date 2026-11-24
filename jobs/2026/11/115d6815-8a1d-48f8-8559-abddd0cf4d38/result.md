# Brand palette & type scale
**A friendly savings app for students**

*Assumption: this is a light-mode mobile app. The identity should make saving feel achievable—not like another assignment or a traditional banking dashboard.*

## 1. Colour palette

| Colour | Hex | Role |
|---|---|---|
| **Notebook Cream** | `#FFF9EF` | Main background. A warm alternative to stark white that keeps the app feeling approachable. |
| **Midnight Ink** | `#172B36` | Primary text, headings and navigation icons. Also the foreground colour for yellow and lilac surfaces. |
| **Steady Teal** | `#006B5E` | Primary actions, active controls and savings progress. Use cream text on teal buttons. |
| **Pocket Sunshine** | `#FFD166` | Milestones, encouraging highlights and small moments of celebration. Use sparingly so achievements feel special. |
| **Study Lilac** | `#DED7F5` | Savings-goal cards, educational tips and secondary surfaces. Adds personality without competing with the main action. |

**Visual balance:** let cream dominate, use ink for structure and teal for action. Reserve lilac for grouped content and yellow for meaningful highlights.

## 2. Colour rules

- **Default reading:** Midnight Ink on Notebook Cream.
- **Primary button:** Steady Teal fill with Notebook Cream text.
- **Secondary button:** cream fill with a teal border and teal label.
- **Goal and tip cards:** Study Lilac fill with Midnight Ink text.
- **Milestone badge:** Pocket Sunshine fill with Midnight Ink text.

These text pairings meet WCAG AA contrast requirements for normal-sized text. Avoid cream text on yellow or lilac, and avoid teal text on yellow for small labels.

Never use colour as the only signal. Pair progress, selection and status with a label or icon. For example, show **“Goal reached”** beside a checkmark rather than simply turning a balance green.

*This is the five-colour brand palette, not a complete semantic UI system. Validation errors should use explicit text and an icon; any dedicated error colour added later needs its own accessibility check.*

## 3. Typography

**Typeface: [Inter](https://fonts.google.com/specimen/Inter)**  
Use one family throughout. Its clear shapes make small interface labels easy to scan, while its numerals suit balances and transactions. Inter is free and open source.

**Fallback stack:** `Inter, system-ui, -apple-system, "Segoe UI", sans-serif`

| Style | Size / line height | Weight | Use |
|---|---:|---:|---|
| **Balance** | 36 / 44 px | 700 | Main savings total |
| **Screen title** | 28 / 36 px | 700 | “Your savings” |
| **Section heading** | 22 / 28 px | 600 | “Your goals” |
| **Card heading** | 18 / 24 px | 600 | “Summer trip” |
| **Body** | 16 / 24 px | 400 | Explanations, tips and form inputs |
| **Action label** | 16 / 20 px | 600 | Buttons and prominent links |
| **Supporting text** | 14 / 20 px | 400 | Transaction details and helper text |
| **Caption** | 12 / 16 px | 500 | Optional timestamps and chart annotations |

## 4. Type rules

- Use **sentence case**: “Add to savings,” not “ADD TO SAVINGS.”
- Keep essential information at **14 px or larger**. Reserve 12 px for nonessential details.
- Use **tabular numerals** for balances and transaction columns so amounts align.
- Keep letter spacing at the font default; avoid widely spaced financial figures.
- Support device text scaling. Let cards and buttons grow rather than clipping labels.
- Don’t reduce text opacity to create hierarchy. Use size, weight and spacing instead.

## 5. Example: savings home screen

**Page background:** Notebook Cream  
**Screen title:** “Your savings” — 28 px bold, Midnight Ink

**Main balance:**  
**£128.50** — 36 px bold, Midnight Ink  
“Saved towards your next big thing.” — 16 px regular, Midnight Ink

**Primary button:**  
“Add to savings” — cream text on Steady Teal

**Goal card:** Study Lilac background  
**Summer trip** — 18 px semibold, Midnight Ink  
“£128.50 of £300 saved” — 14 px regular, Midnight Ink  
Teal progress bar with a visible **43%** label

**Milestone badge:** Pocket Sunshine background  
“First £100 saved ✓” — 14 px medium, Midnight Ink

The result: a calm, legible money app with enough warmth to make small wins feel worth coming back for.

---

**From the agent:** Delivered by Saffron: the work is below.
