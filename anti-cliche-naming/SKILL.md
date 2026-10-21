---
name: anti-cliche-naming
description: Prevent the use of overused AI-generated personal names, surnames, place names, brand names, and morphological naming tropes. Trigger whenever generating fictional characters, worldbuilding, stories, roleplay scenarios, case studies, user personas, synthetic datasets, or mock company/product names.
---

# Anti-Cliche Naming

Eliminate repetitive, default LLM naming conventions across creative writing, worldbuilding, user personas, and mock business examples by blocking high-frequency AI names and replacing them with grounded, etymologically specific alternatives.

## When to Use

- Writing fiction, screenplays, roleplay scenarios, or creative stories requiring character or location names
- Building fantasy, sci-fi, historical, or contemporary worlds
- Creating user personas, UX case studies, hypothetical scenarios, or mock datasets
- Brainstorming or assigning placeholder brand, startup, corporate, or shop names

## Naming Guardrails and Banned Lists

Large language models default to a narrow phonetic cluster (liquid consonants like L, R, M, N paired with open vowels or sharp V/K/Z plosives). Avoid the following tokens and morphological formulas unless explicitly requested by the user.

### 1. Banned Personal First Names

- **The E / El- Cluster:** Elara, Elena, Elias, Elian, Elowen, Elodie, Elijah, Evelyn, Evangeline, Ezra, Ember, Erik
- **The A / Ar- Cluster:** Aris, Aria, Aurelia, Alistair, Astrid, Arthur, Anya, Amara, Adrian, Adrien, Asher, Alaric
- **The L / Ly- Cluster:** Lyra, Lysander, Lysandra, Liam, Leo, Lena, Lilith, Lucian, Lukas, Liora
- **The K / C- Cluster:** Kael, Kaelen, Kaia, Kira, Kai, Kestrel, Knox, Cassian, Caspian, Caelum, Callum, Clara, Chloe, Caleb
- **The S / M- Cluster:** Silas, Seraphina, Rowan, Ronan, Maya, Mara, Marcus, Mateo, Malakai, Maeve, Mira
- **The V / Z / N- Cluster:** Valeria, Vesper, Viktor, Zara, Zephyr, Zane, Nyx, Nadia, Nolan, Nora, Orion, Orin
- **Default Corporate / Persona First Names:** Sarah, Sara, Alex, Jordan, Sam, Priya, Sofia, Sophia, Julian, Lucas, Rachel, David

### 2. Banned Surnames

- **Monosyllabic / Punchy Surnames:** Thorne, Vance, Voss, Vane, Cross, Hale, Cole, Finch, Reed, Grant, Hayes, Ward, Blake, Steele, Frost, Knox, Nash, Mercer, Sterling, Pierce, Grave, Graves, Locke, Kade, Rook
- **Compound Gothic / Fantasy Surnames:** Blackwood, Blackthorne, Ravenscroft, Nightshade, Ironwood, Hollowell, Ashworth, Hawthorne, Stormborn, Silverhand, Shadowmere, Winterbourne
- **Default Contemporary / Academic Surnames:** Chen, Patel, Lin, Zhang, Miller, Carter, Brooks, Jenkins, Rivera, Morales, Kowalski, Volkov, Petrova, Sato, Takahashi, Al-Mansoor, Okafor, Rossi, Moreau

### 3. Banned Archetype Pairings

- **The Weary Keeper / Detective / Archivist:** Never pair Elias, Silas, or Arthur with Thorne, Vance, or Pendelton.
- **The Lead Scientist / Ethicist:** Never pair Dr. Elara, Dr. Aris, or Dr. Maya with Voss, Vance, Thorne, or Chen.
- **The Corporate Executive / Antagonist:** Never pair Marcus, Julian, Viktor, or Alistair with Vance, Sterling, Voss, or Blackwood.

### 4. Banned Place Names

- **Fantasy Realms and Kingdoms:** Eldoria, Aethelgard, Valoria, Valorian, Vaeloria, Sylvaria, Sylvaris, Zephyria, Thalassia, Kaeloria, Nyxoria, Solaris, Aerthos, Drakenhold, Luminaria, Avaloria
- **Towns, Cities, and Outposts:** Oakhaven, Silverkeep, Ironhold, Ironforge, Ravenspire, Shadowfen, Shadowmere, Whispering Woods, Whispering Pines, Glimmerbrook, Frostford, Emberfall, Stormwatch, Mistwood, Sunforge, Stonecrest, Highreach, Fairhaven, Grimhold, Briarwood, Millbrook, Willow Creek, Blackwater, Mapleshade, Cedar Cove, Pinecrest, Harborview, Hollow Creek
- **Taverns and Shops:** The Rusty Anchor, The Silver Stag, The Gilded Lily, The Sleeping Dragon, The Shattered Crown, The Wayward Compass, The Whispering Willow
- **Sci-Fi and Cyberpunk Settings:** Neo-Veridia, Neo-Tokyo, New Arcadia, Neo-Kyoto, New Eden, Terminus, Haven, Aegis Station, Outpost 7, Sector 7G, Elysium, Citadel, Nova Prime, Xylos, Kryos, Veridia Prime, Aethel, Orion's Reach
- **Starships:** The Odyssey, The Vanguard, The Horizon, The Endeavour, The Prometheus, The Astral Wind, The Rustbucket, The Starlight

### 5. Banned Brand, Tech, and Company Names

- **Megacorps and Defense Contractors:** OmniCorp, Nexus Corp, Nexus Dynamics, Aether, Aetherius Corp, Vanguard Industries, Apex Global, Apex Dynamics, Zenith Corporation, Chronos, Helix Biotech, Veridian Dynamics, Obsidian Security, Prometheus Labs, Chimera Corp, Titan Industries, Valiant
- **SaaS, AI, and Tech Startups:** Lumina, Pulse, Aura, Nova, Flux, Prism, Vertex, Elevate, Synergy, FlowState, MindScape, TaskFlow, CloudSync, FinSight, VeriData, Strata, Echo, Velo, Synthetix, Quantix
- **Local Shops and Boutiques:** The Daily Grind, Bean and Leaf, The Velvet Bean, Hearth and Home, Bloom and Wild, The Copper Kettle, Crumb and Co., Paper and Pine, Wanderlust Books

## Banned Morphological Patterns

Avoid substituting a banned name with a phonetic neighbor (for example, replacing Elara with Alara or Elowen). Block the underlying structural formulas:

1. **Liquid-Vowel Fantasy Suffixes:** Do not construct realm or character names starting with `El-`, `Ae-`, `Kae-`, `Syl-`, `Val-`, or `Zeph-` combined with `-ia`, `-or`, `-oria`, `-gard`, `-is`, or `-en`.
2. **Compound Noun-Verb Town Names:** Do not build settlement names using `[Nature/Metal/Shadow Noun] + [haven/keep/hold/wood/brook/crest/spire/watch/fall/fen/forge]`.
3. **Soft-First / Hard-Last Cadence:** Do not default to pairing a multi-syllable vowel-heavy first name with a sharp, single-syllable consonant surname (such as `[3-syllable vowel name] + [1-syllable V/T/K surname]`).

## Positive Replacement Workflow

Follow these steps whenever generating names:

1. **Select a Specific Real-World Linguistic Anchor:** Ground names in a concrete historical, regional, or cultural register appropriate to the setting (for example, 14th-century Flemish, rural Appalachian, Lusophone, Old Norse, Marathi, Basque, Swahili, or 1970s regional municipal directories).
2. **Vary Phonetic Texture and Rhythm:** Include plosives, fricatives, and unglamorous consonant clusters (such as B, D, G, P, T, W, H). Vary syllable counts so first and last names do not follow a predictable poetic meter.
3. **Use Mundane and Historical Toponymy for Places:** Real places are often named after mundane geographical features, obscure founders, agricultural products, or bureaucratic designations (for example, Great Snoring, Badgers Mount, Oskaloosa, Kankakee, Worms, Drogheda, or Culpeper) rather than dramatic poetic compounds.
4. **Ground Brands in Specific Founders or Concrete Nouns:** Instead of Latinate buzzwords (Lumina, Nexus, Vertex), use founder surnames, regional initials, industrial terms, or dry descriptive phrases (for example, Vance-Goddard Milling -> change to Halvorsen & Priddy Logistics, Standard Trenching Co., or Kestrel -> Bramblett Analytics).

## Gotchas

- **Phonetic Drift:** Watch out for subtle variations of banned names (such as turning *Thorne* into *Thornfield* or *Vance* into *Vanson*). Pick an entirely distinct root.
- **Over-Correcting into Unpronounceable Names:** Avoiding cliches does not mean stringing random consonants together. Names should still feel natural and pronounceable within their chosen linguistic tradition.
