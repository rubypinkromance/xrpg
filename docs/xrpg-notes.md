# XRPG Revision Notes

Working scratchpad for the edited rewrite of `xrpg-rules.md`. The current rules
doc is a lightly-cleaned version of an unmaintained, poorly-edited PDF. These are
notes on the rough edges we've reasoned through, what the rules-as-written (RAW)
actually say, what's *missing*, and the best inferred readings to fold into the
revision. Sloppy by design — append freely.

Legend:
- **RAW** = stated in the current doc.
- **Inferred** = our best reading; not explicit in the text.
- **GAP** = genuinely missing from the doc; needs to be written.
- **AMBIGUOUS** = the text says something, but it's unclear/contradictory.

---

## 0. Decisions already made (from the consistency pass)

Derived from the git history of `xrpg-rules.md`, **verified against current HEAD**.
Note: a late commit, `5cd216d "revert some opinionated changes"`, deliberately undid
much of the earlier *cleanup* commit's restructuring — so some changes I first read
as "settled" were rolled back. The list below reflects what's *actually* in the doc
now.

**Terminology (confirmed in current HEAD — carry forward)**
- **"attribute growth"** — the original PDF's "attribute *scaling*" was renamed
  throughout. Use "growth" everywhere. (Survived the revert.)
- **"nonplayer character"** (was "non-character").
- **"Encounter Player"** standardized.

**Formatting / headings (confirmed current)**
- Top-level sections use `#`, subsections `##`. Equipment tables titled "Armour
  Archetypes" / "Weapon Archetypes" with trimmed columns.
- En-dash numeric ranges (1–9, 10–19); real minus signs (−) in maluses; em-dash
  punctuation; comma cleanups.
- "Calm" art-credit line dropped from the header; CalliopeX credit + Kuroinu thanks
  blockquote kept.
- TOC is **doctoc-generated** — don't hand-edit; re-run
  `npx doctoc --notitle --maxlevel 2 docs/xrpg-rules.md`.
  ⚠️ It's **currently stale**: after the revert the TOC still lists Hitpoints +
  Damage + Sample Combat under *Encounters*, but in the body Damage + A Sample
  Combat now sit up near Resources. Re-run doctoc to resync.

**Structure — REVERTED to the original PDF layout (important signal)**
- `5cd216d` rolled back the *cleanup* reorg. Current HEAD:
  - **Hitpoints is NOT its own section** — folded back into the **Damage** paragraph.
  - **Damage** + **A Sample Combat** sit up near Resources (in the Characters
    block), not under Encounters.
  - The empty **"### Basics"** subheading under Magic is back.
- **Takeaway:** the author is wary of aggressive restructuring and prefers to keep
  the original organization. For the rewrite, *propose* opinionated structural
  changes and get a yes before making them wholesale — don't assume.

**Still inconsistent — NOT decided (ties into §4/§5)**
- **"Bonus" vs "bonus die":** the prose definition (Basic Mechanics) currently reads
  "a **bonus**" — no "die" — while the Weapon Archetype table lists Dual =
  "**Bonus die**". Pick one term and apply it everywhere — and while you're there,
  define "minus" as its inverse (see §5).

---

## 1. Contests & the combat model

### It's an opposed-clash model, NOT D&D to-hit
- **RAW:** A contest is one opposed roll — both parties roll `Level Die + relevant
  attribute`, higher total wins, ties broken by higher decimal.
- **Inferred:** There is no "miss = nothing happens." Each contest is a single
  *simultaneous exchange* where the **loser suffers the consequence**. The sample
  combat shows this: "Layla slashes… *while the troll tries to knock her away
  first*." She loses the clash, so the troll's blow lands. The damage isn't a
  whiff — she got beaten in the exchange.
- **Design consequence:** You can't poke-and-retreat. Engaging at all means
  entering a mutual exchange you might lose. This is deliberate and fits the
  erotic-defeat theme.
- **No initiative / no turns.** Forget "my turn / your turn." The unit of play is
  the **exchange** (= a "round"; note materials have durability measured in
  rounds). One opposed roll resolves both offense and defense at once.

### Anatomy of one exchange (round) — for the rewrite
1. Player declares an action (in-fiction).
2. EP declares the monster's action.
3. Identify the contest type / which two attributes oppose (set by what each side
   chose to do).
4. Both roll `Level Die + attribute` (+ any spell/item bonus dice spent this
   exchange).
5. Compare; higher wins, decimal breaks ties.
6. Resolve consequences (see below).
7. Narrate, repeat until a loss condition.

### Play format
- Conversational, built for instant messaging: player vs Encounter Player, taking
  turns *describing*, but resolving via opposed rolls.
- Where player agency lives: (a) **which contest you force** (a caster can make it
  Sorcery-vs-Sorcery instead of Athleticism-vs-Athleticism by opening with a
  spell); (b) **spending spells/items into a specific exchange** (bonus die,
  potion, re-roll, advantage); (c) **negation** spells (Blink, Afterimage) that
  cancel one incoming hit's HP consequence at a Mana cost.

### The four standard contests (RAW)
- Sorcery vs Sorcery — magic duel.
- Athleticism vs Athleticism — physical combat / "surviving" an ordeal.
- Charisma vs Charisma — flirt/trick your way past someone's guard to bed them
  (mutual seduction game).
- Charisma vs Willpower — entice someone who *ought to resist*.

### Loss conditions (RAW)
Defeat = reduced to **0 in ANY pool**: Hitpoints, Mana, Resolve, or Stamina.
Defeat ≠ death. 1st loss → **Injured**, 2nd → **Wounded**, 3rd uncured → bad end
(GM discretion). Players have only **3 HP** and rarely gain more.

---

## 2. Damage: Attack vs Defence (the HP layer)

### Two separate systems, often conflated
- **The contest roll** decides *who wins* → `Level Die + attribute`. This is where
  **percentage** attribute bonuses/maluses from gear archetypes land.
- **Attack vs Defence** decides *how many Hitpoints the loser sheds* → these are
  **flat integers**, not percentages or the roll totals.

### Damage formula (Inferred, but matches the sample exactly)
`HP lost = 1 + max(0, Attack − Defence)`
- Attack = the winner's flat **weapon** value (archetype flat mods + upgrade level
  + accessories).
- Defence = the loser's flat **armour** value.
- Attributes do NOT enter the HP calculation. They only decide who wins the
  contest. Margin can't go negative (floor of 1 HP).
- Sample checks:
  - Layla kills troll: `1 + (sword +2 − rags 0) = 3` → one-shots 3 HP. ✓
  - Troll hits Layla: `1 + (club +2 − armour +2) = 1`. ✓
  - Without her armour it'd be `1 + (2 − 0) = 3` ("would have finished her off"). ✓
- Worked confirmation: boar with +1 armour vs Layla's +2 sword → `1 + (2−1) = 2 HP`.

### AMBIGUOUS: what ARE Attack/Defence numerically?
The doc never defines them. Inferred = flat gear values (above), because that's
the only reading that reproduces the sample. The rejected alternative: Attack/
Defence = the roll totals (e.g., 21.52 vs 3.5 → ~19 HP, instant kill). Rejected
because the sample credits the bonus damage to the +2 sword, not the roll margin.
**For the rewrite: pick one explicitly.**

### HP fights are short; resource fights are long
A +2 weapon vs an unarmoured 3-HP foe = instant kill. So straight HP combat is
brutally swingy/fast unless *both* sides have Defence. The drawn-out grind is
**Attrition** (resource pools), not HP. By design.

---

## 3. Equipment modifiers — two axes

### Percentage bonuses (affect attributes → the contest roll)
- Archetypes give ±10% of an attribute (bonus in one, malus in another to balance).
- **Weak early, strong late:** 10% of a level-2 attribute (~1.5) is ~0.15
  (negligible); 10% of a level-end attribute (~50) is ~5 (meaty).

### Flat Attack/Defence (affect the damage layer)
Sources of flat Attack/Defence are RARE, especially early:
- **Plate** armour: +1 Defence.
- **Barbarian** armour: +1 Attack, −1 Defence (the only archetype touching Attack
  without an upgrade).
- **Accessories** (shields, trinkets): GM-defined, open-ended ("a shield that
  assists in defence").
- **Upgrades** (+X): the big one — see GAP below.
- NOTE: Sorcerous Weapon/Shield spells add a bonus **die to the roll**, NOT flat
  Attack/Defence. Different layer.

### GAP: the equipment upgrade system is missing
- The Materials table has an **"Upgrade Cap"** column (Wood +1, Hide/Stone/Bone +3,
  Leather/Copper/Iron +6, Bronze/Steel/Obsidian +9, mythic = "Asc"/uncapped) but
  **no rules anywhere explain how upgrading works.**
- The "+2 sword / +2 armour" in the sample = an item upgraded to +2. The "+X"
  notation IS the upgrade level; weapon +X adds X to Attack, armour +X adds X to
  Defence.
- Missing: how you upgrade (cost? smith in town? materials?), what each step does,
  whether +X is found as loot or crafted.
- This matters a LOT: a single "+1" swings damage hugely (1-HP poke → near-kill).
  How upgrades are gated is one of the biggest balance levers in the game.
- **Likely intent:** a town vendor/smith. To be decided in the rewrite.

---

## 4. Bonuses (the "bonus die" mechanic)

### RAW definition
A **bonus** = "roll whatever die is dictated for the roll again and add it to your
standard roll." So `1d4` → `2d4` (results summed). It's an *operation*, not a
stored die.

### Scaling insight
- **Bonus dice are huge early, minor late.** Early the die *is* the roll (attrs
  ~1.5, % bonuses ~0.15), so a second die roughly doubles your output. Late,
  attributes (50+) dominate and one extra die is noise.
- This is the inverse of percentage bonuses (weak early, strong late). The two
  bonus systems are complementary by level band: **bonus dice carry you early,
  percentage attribute bonuses carry you late.**

---

## 5. "Minus" — the undefined inverse of bonus (Arcane Ebb etc.)

### The problem
The doc defines **bonus**, **advantage**, **disadvantage** — but Creature Feats
use "**Minus to spells cast**" (Arcane Ebb) without ever defining "minus."

### Inferred reading: minus = subtract an extra die
- Since bonus = "roll an extra die and ADD it," the clean arithmetic inverse is
  "roll an extra die and **SUBTRACT** it": `standard roll − extra die`.
- Always applicable (operates on the standard roll), so no "ran out of dice" /
  "roll 0 dice" / autofail edge case. The "roll one less die" reading is the broken
  one and is rejected.
- **Why not disadvantage?** (a) Subtract-a-die is mathematically symmetric with
  bonus (each shifts the average by E[die]); disadvantage is a weaker, asymmetric,
  distribution-dependent penalty. (b) The doc *uses* "advantage/disadvantage" by
  name elsewhere (Quick Wit, Assured, Deep Breath) — so choosing "minus," paired
  opposite "bonus," signals the bonus family, not the advantage family. Same logic
  as equipment "maluses" (defined as subtractions paired against "bonuses").
- No autofail concern: nothing treats a low/negative contest total as a special
  failure. You just compare totals; negative = near-certain loss, not a special
  state. (Only resources-at-0 and the Elasticity table care about specific
  numbers.)
- **For the rewrite:** define "minus" explicitly (subtract-a-die recommended), OR
  consciously redefine Arcane Ebb as disadvantage / flat penalty.

---

## 6. Magic & Mana

### Casting spends Mana (it's there, just buried)
- RAW, in the Magic prose: "Casting any kind of spell will draw Mana from your
  body." The per-type cost formulas live at the end of each spell-type entry. Easy
  to miss — give it its own heading in the rewrite.
- **Mana pool = 10 × Sorcery.**

### Cost formulas
- **Attack spells:** `target's Sorcery + average of the chosen die`. Caster picks
  the die (Bolt d4 → Tempest d60); bigger die = more cost + bigger contest roll.
  Die avgs: d4=2.5, d8=4.5, d12=6.5, d20=10.5, d30=15.5, d60=30.5. Cost scales with
  the *target's* Sorcery (punching through resistance).
- **Bonus spells:** `(avg of Level Die × 2) + floor of the roll`. AMBIGUOUS —
  "floor of the roll" could be floor(avg) or the actual rolled value. Read as
  floor(avg) → d10 ≈ `11 + 5 = 16`. **Needs clarification in rewrite.**
- **Miscellaneous spells:** `4 × target's level` (self-buffs = 4× your level),
  unless the spell says otherwise.

### Early pool is tiny → real throttle
- Sorcery ~1.5 → Mana 15. A Bolt at a Sorcery-2 target ≈ `2 + 2.5 = 4.5` (≈3 casts
  and you're dry). A Surge ≈ 12.5 (almost whole pool). Tempest = uncastable.
- Emptying Mana = a **defeat condition** (faint). Spamming spells loses fights.

### Other limits on "cast anything you know"
1. Mana cost (above).
2. Must *know* the spell (1 perk pt in-school, **3×** out-of-school; start w/ one
   school + one spell).
3. Tree order post-creation (prereqs first).
4. Per-spell conditions: "double out" gates (Dominate doubles Willpower; Meteor/
   Maelstrom/Banish double out Sorcery), same-level requirements (Dispel, Banish).
5. Attack spells are still **contests you can lose** — spending Mana ≠ auto-success.
6. Mana regen is slow; sustained casting needs potions or Mana/Arcanery traits.

### Casting fits the exchange structure
An attack spell *is* your contest that round; a bonus spell adds a die to a contest
you're making; negation spells cancel one incoming hit. Mana — not the spell list —
is the leash.

---

## 7. Resource drain (the attrition engine)

### Inferred reading (user's, well-supported)
- RAW clause: "Suffering an attack and defending it will **always** reduce your
  relative resources by the amount of their modifier to it. Physical → attacker's
  Athleticism, Magic → Sorcery, Mental → Willpower."
- Best reading: being attacked drains your resources **every round, win or lose**
  (the "always" + "defending it" = even on a successful defense).
- **Why this reading:** the **Attrition** encounter type only works if winning a
  contest still costs resources — otherwise you could win every roll and never be
  ground down. The mechanic *requires* it.
- So each lost exchange = HP loss (gear-gated) + resource drain; and even *won*
  exchanges against an aggressor cost resources.

### How the level gap actually kills
Level-2 char vs level-14 troll: each round bleeds ~16.5 Stamina (troll's
Athleticism). Starting Stamina (10×Endurance) ≈ 15–25, so 1–2 rounds empties the
pool → defeat by Stamina, not HP. The gap kills via attrition, not Hitpoints.

### AMBIGUOUS: who is "the attacker" in a mutual clash?
The modifier that sets resource drain is "the attacker's." In a simultaneous
exchange it's unclear who that is. Sample always frames the monster as aggressor /
player as sufferer. **Default: attacker = the monster / the one pressing the
assault.** Last loose thread in the combat math — nail down in rewrite.

---

## 8. Mismatched-action contests (e.g., "I seduce the troll" while it attacks)

AMBIGUOUS / GM-fiat — the doc doesn't resolve cross-action exchanges. Findings:
- **Step zero — can it even be charmed?** Feats **Bestial** ("immune to logic and
  Charisma attacks") and **Magic Immune** shut seduction down entirely. A feral
  troll may be Bestial → attempt auto-fails, you eat the physical exchange.
- **If charmable — cleanest ruling (recommended):** your action sets your attribute
  (Charisma); the defender resists with the stat matching the *attack* (Willpower),
  NOT a mismatched Cha-vs-Ath. So it's the defined **Charisma vs Willpower**
  contest. The troll's "swing" is just the *stakes of failure*.
  - Win → it's **Charmed** (must beat your Willpower to break free); you're in
    control, the blow never lands.
  - Lose → it shrugs off the charm AND lands its hit (normal contest-loss).
  - Collapses to a single Cha-vs-Will roll. Preserves one-roll-per-exchange.
- **Alternatives:** (a) sequence it — seduction first (Cha vs Will), then a separate
  physical exchange on failure; (b) live-attacker rule — if the troll already
  committed, you must defend (Ath) or auto-take it, seduction unavailable this
  exchange (makes seduction an opener only).

---

## Running list of GAPs / AMBIGUITIES to resolve in the rewrite
- [ ] **GAP:** equipment upgrade system (the "+X" / Upgrade Cap mechanic).
- [ ] **AMBIGUOUS:** numeric definition of Attack/Defence (flat gear vs roll total).
- [ ] **UNDEFINED:** "minus" mechanic (recommend: subtract-a-die).
- [ ] **AMBIGUOUS:** bonus-spell cost "floor of the roll."
- [ ] **AMBIGUOUS:** who counts as "the attacker" for resource drain in a clash.
- [ ] **AMBIGUOUS:** mismatched-action contests (seduce-vs-attack).
- [ ] Consider: give "casting spends Mana" + cost formulas their own clear section.
- [ ] Consider: spell-by-spell pass on "double out" / same-level cast conditions.
- [ ] Terminology: settle "bonus" vs "bonus die" (prose vs Dual-weapon table clash).
- [ ] Wording: "irregardless" (Equip Load, ~line 89) → "regardless" (nonstandard word
      introduced during the cleanup pass).
