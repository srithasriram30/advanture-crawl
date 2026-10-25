# Decisions Log

Design decisions for the Adventure Crawl site, newest additions at the bottom.
Each entry records what was decided and why.

## MVP scope

- **Date:** 2026-10-24
- **Decision:** The MVP covers the full crawl, chapters 0–2 only, playable as the Simple Human Commoner.
- **Reason:** Proves the whole flow (dice, saves, combat, rewards, save files) on a small slice before adding races, classes and feats.

## Simple Human Commoner starts at 10 HP

- **Date:** 2026-10-24
- **Decision:** The Simple Human Commoner starts with 10 HP, not 11.
- **Reason:** The crawl says this option gains "no class features", so the Commoner class's +1 HP does not apply.

## Chapter = day

- **Date:** 2026-10-24
- **Decision:** Abilities described as "once per day" are treated as "once per chapter".
- **Reason:** The original crawl released one chapter per day; players on this site move at their own pace.

## Honor system for tasks

- **Date:** 2026-10-24
- **Decision:** Players mark writing tasks complete themselves. Tasks that depend on the player's manuscript (e.g. "Sprint to the next thousand") are honor-only.
- **Reason:** Writing happens in the player's own tools, so the site cannot verify word counts.

## Dice rolls are locked

- **Date:** 2026-10-24
- **Decision:** Dice are rolled in the browser. Once rolled, a result is locked and saved with the player's progress, unless an ability allows a reroll.
- **Reason:** Prevents rerolling until a favourable result appears, while leaving room for reroll abilities (Lucky Duck, Warlock).

## Database: Prisma Postgres on Vercel

- **Date:** 2026-10-24
- **Decision:** Read-only game content is stored in Prisma Postgres, provisioned through the Vercel Marketplace.
- **Reason:** Vercel runs server code in short-lived serverless functions that don't keep files, so a file-based database like SQLite doesn't fit. Prisma Postgres integrates directly with Prisma.

## Task goals stored as JSON

- **Date:** 2026-10-25
- **Decision:** Shared task fields (id, order, text) are normal columns, `kind` is a Prisma enum, and kind-specific goal details are stored in a `goal` JSON column. Goals are validated with Zod at seed time and when read.
- **Reason:** Goal shapes are deeply nested (amounts, dice formulas, choices) and the content is read-only and never searched by goal fields. JSON handles nesting and new mechanics without database migrations; Zod provides the validation the database can't.

## Save data format

- **Date:** 2026-10-25
- **Decision:** Save data supports multiple characters and includes a `version` field.
- **Reason:** Allows save slots later without migrating existing saves. The version field lets the app recognise older saves and upgrade them instead of rejecting them.

## Character death

- **Date:** 2026-10-25
- **Decision:** When a character reaches 0 HP, the player restarts from Chapter 0 with a new character; gold, items, allies and progress are lost. Total XP is tracked per player as the sum across all characters, living and retired.
- **Reason:** Follows the crawl's rule that a new character starts at 0 XP while total XP keeps counting. Retired characters are kept so total XP can be calculated rather than stored separately.

## Rules help

- **Date:** 2026-10-25
- **Decision:** The MVP includes a `/rules` page. Inline explanations of terms (AC, DC, saves) come later.
- **Reason:** New players need the basics up front; inline help is a polish feature.

## Source typos

- **Date:** 2026-10-24
- **Decision:** Typos and inconsistencies in the source documents are logged here as each chapter is seeded, with how each was resolved.
- **Reason:** Keeps seed data traceable back to the source.
