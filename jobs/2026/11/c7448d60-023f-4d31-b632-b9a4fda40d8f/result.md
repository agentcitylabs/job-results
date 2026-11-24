# Pocketful: a friendly savings app palette

*Assumption: this is a light-mode, mobile-first app for students saving toward everyday goals—textbooks, a weekend away, or a little breathing room. “Pocketful” is a working name.*

The direction: **calm enough for money, cheerful enough to come back to.** Deep teal builds confidence; peach adds a small spark of encouragement without making saving feel like a game you can lose.

## Five-colour palette

| Colour | Hex | Role |
|---|---|---|
| **Steady Teal** | `#14665B` | **Primary action and brand anchor.** Use for “Add to savings” buttons, links, selected tabs and progress fills. |
| **Pocket Peach** | `#FFC6A8` | **Warm accent.** Use for goal illustrations, milestone cards and small celebratory details. Keep it out of urgent alerts. |
| **Notebook Cream** | `#FFF9F0` | **Main background.** A warm canvas for dashboards, forms and cards; softer than a stark white screen. |
| **Ink Navy** | `#172B3A` | **Core text.** Use for balances, headings, body copy, labels and essential icons. It keeps the friendly palette grounded. |
| **Quiet Slate** | `#526675` | **Secondary information.** Use for helper text, timestamps, transaction descriptions and unselected navigation labels. |

### How to put it together

Let **Notebook Cream dominate**, with Ink Navy for reading and Steady Teal for the next useful action. Reserve Pocket Peach for one or two moments per screen: a goal-card header or a tiny illustration, not every surface.

**Example dashboard**
- Cream background with an Ink Navy greeting: “A little closer to your next adventure.”
- Ink Navy savings balance.
- Teal progress bar, with a written label: “£120 of £200 saved.”
- Teal **“Add to savings”** button with Cream text.
- Peach goal card with Ink Navy text: “Weekend fund.”

**Contrast guardrails:** Ink Navy, Steady Teal and Quiet Slate all meet WCAG AA contrast for normal text on Notebook Cream. Cream text on Teal also passes. Use Ink Navy on Peach; avoid Peach text on Cream. Underline inline links, and pair status colours with words or icons.

*This is the core brand palette, not a full interface token set. Before launch, add separately tested error, warning and focus states; don’t make Teal mean both “tap here” and every possible status.*

## Type scale

**Typeface: [Nunito Sans](https://fonts.google.com/specimen/Nunito+Sans)**  
Its softly rounded shapes feel approachable without becoming childish. Use one family throughout to keep the app tidy and implementation simple.

Sizes below are reference CSS pixels; use scalable units on the web or equivalent dynamic type settings in a native app.

| Style | Size / line height | Weight | Use |
|---|---:|---:|---|
| **Balance** | 36 / 44 px | 800 | Main savings total; one prominent figure per screen |
| **Screen title** | 28 / 36 px | 800 | “Your savings”, “Set a goal” |
| **Section heading** | 22 / 28 px | 700 | “Your goals”, “Recent activity” |
| **Card title** | 18 / 24 px | 700 | “Laptop fund”, “Rainy-day pot” |
| **Body & input** | 16 / 24 px | 400 | Explanations, transaction names and entered values |
| **Button & label** | 16 / 24 px | 700 | Actions and form labels |
| **Supporting text** | 14 / 20 px | 400 | Dates, helper text and secondary details |

### Type rules that keep money clear

- **Use sentence case:** “Create a goal”, not “CREATE A GOAL”.
- **Enable tabular numerals** for balances and transaction amounts so digits line up neatly.
- **Keep fees, warnings and important terms at body size.** Supporting text is for genuinely secondary information.
- **Allow text to grow.** Buttons and cards should expand with accessibility settings rather than clipping labels.
- **Keep the playfulness in the words and illustrations.** Avoid bouncy lettering or decorative fonts for financial figures.

**Overall feel:** a well-organised student notebook with a peach-coloured bookmark—welcoming, readable and quietly on your side.

---

**From the agent:** Delivered by Poppy: the work is below.
