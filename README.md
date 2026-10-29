# BiteWise

An Indian-first, responsive Grade 11 AI capstone prototype for personalised, educational nutrition ideas. It supports ages **15–80**, includes Indian and Western meal ideas, Indian compound meals, local substitutions and preferences, and is intentionally cautious about health claims.

## Run it

Open `index.html` in a modern web browser. No installation or server is needed. The profile stays only in the current browser session and is passed to the spacious `results.html` dashboard through session storage.

## What the prototype does

1. Collects age (required), region, optional height/weight, diet pattern, activity, cooking access, budget, allergies/restrictions, foods enjoyed and a general goal.
2. Filters a small set of meal-building ideas by diet and basic allergy wording (e.g. peanut/lactose).
3. Gives regional, seasonal, value-oriented substitution prompts intended to support food diversity and reduced waste.
4. Blocks calorie targets and weight-loss recommendations for everyone; it explicitly adds an extra no-diet/no-body-size safeguard for ages 15–17.

It is a rules-based prototype, not a clinical AI system. A production version can use a transparent ranking model, but should retain hard safety rules and a dietitian review pathway.

## Data design and honesty

`data/foods.example.json` is the scalable food-record shape. It separates a raw food from a recipe, carries source and IFCT-ID fields, and prevents values from appearing until they are verified.

### Values currently shown

The results comparison table uses clearly labelled **demo reference estimates** to demonstrate the interface. They are not sourced IFCT values, not personalised targets, and must not be used for clinical or nutrition advice. The small example food records intentionally use `null` nutrient fields and `awaiting_verified_ifct_import` status. This avoids presenting guessed food composition as fact.

### Authoritative Indian source

The intended primary source is *Indian Food Composition Tables (IFCT), 2017*, published by ICMR–National Institute of Nutrition. It provides analytical composition data for 528 key foods and 151 components. It mainly represents raw foods (with stated exceptions), so a thali, curry or other cooked compound meal must be calculated from cited ingredients, raw-to-cooked yields and an explicit recipe—not treated as an IFCT food entry.

- IFCT 2017 full publication: https://www.nin.res.in/ebooks/IFCT2017.pdf
- ICMR–NIN downloads (also lists RDA/EAR material): https://www.nin.res.in/downloads/
- ICMR nutrition overview: https://www.icmr.gov.in/nutrition

### Safe expansion plan

For each verified Indian ingredient, import the exact IFCT food ID, name, group, per-100 g units, edible-portion basis, raw/cooked status, source page/table, extraction date and reviewer. For each recipe, retain gram weights, cooking method, yield and food IDs. Add a separate `source` record for Western foods (for example, a government food-composition database); never silently mix it into IFCT. Show a “source / estimate / missing” badge beside every nutrition number.

## Recommendation flow (pseudocode)

```
validate(age in 15..80)
apply hard exclusions: allergies, diet pattern, declared restrictions
if age < 18: disable weight-loss, calorie deficit and body-size outputs
if medical flag / pregnancy / eating concern: show clinician referral; avoid condition-specific advice
retrieve eligible ingredients and recipes
score by: preferences + region + affordability + seasonal/local tags + variety
construct meal templates: staple + pulse/protein + vegetables + fruit/optional dairy
rank substitutions by availability, price band and lower-waste use of leftovers
attach provenance to every computed nutrient; suppress it if provenance is incomplete
```

## Safety boundaries

BiteWise is educational meal inspiration, not medical advice, diagnosis, treatment or a prescribed diet. It should direct users with a medical condition, pregnancy, a diagnosed allergy, an eating concern, or a prescribed diet to a qualified clinician/registered dietitian. Do not collect health data beyond a clear need, and obtain appropriate consent and adult/guardian safeguards before any real-world use involving minors.

## SDG alignment

- **SDG 3 — Good Health & Well-being:** approachable, non-restrictive food-literacy and varied balanced meal ideas.
- **SDG 12 — Responsible Consumption & Production:** local/seasonal and budget-sensitive alternatives, whole pulses, and leftover-aware prompts intended to reduce avoidable household food waste.

## Suggested next capstone milestones

1. Build an audited IFCT import sheet with one reviewer and source-location fields per record.
2. Conduct usability testing with students and families using fictional profiles only.
3. Evaluate recommendation relevance, allergy-exclusion accuracy, regional fit and comprehension of the safety notice.
4. Have a qualified nutrition professional review all nutrition logic before showing calculated nutrients or health-related outputs.
