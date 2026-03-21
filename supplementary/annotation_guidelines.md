# GEPR-LINAN Dataset Annotation Guidelines

**Annotation Standards for Geographical Entities and Spatial Relations of Lin'an, Southern Song Dynasty — v1.0**

| Item | Value |
|------|-------|
| Entity types | 24 |
| Relation types | 34 |
| Annotated sentences | 4,920 |
| Annotated entities | 25,397 |
| Annotated relations | 17,881 |
| IAA Entity F1 | 86.38% |
| IAA Relation F1 | 77.82% |

---

## Chapter 1　General Principles

### 1.1 Dataset Background

GEPR-LINAN is a named entity recognition and relation extraction dataset for geographical information from Lin'an (present-day Hangzhou, Zhejiang), the capital of the Southern Song Dynasty. The corpus is drawn from **19 text units (18 IND text units from 15 source works, plus 1 OOD text)**, including *Xianchun Lin'an Zhi*, *Qiandao Lin'an Zhi*, *Chunyou Lin'an Zhi*, *Mengliang Lu*, *Wulin Jiushi*, and *Ducheng Jisheng*, comprising 4,920 annotated sentences, 25,397 entity instances, and 17,881 relation instances across 24 geographical entity categories and 34 geographical relation types.

### 1.2 Annotation Objectives

1. Identify the boundaries and types of all geographical entities in the text;
2. Extract spatial relations between entities;
3. Ensure annotation consistency so that independent annotators produce highly concordant results for the same text.

### 1.3 Core Principles

- **Maximal span principle**: Annotate the complete geographical name, including modifying attributes (directional words, dynastic qualifiers, etc.);
- **Explicitness principle**: Annotate only entities and relations for which there is clear linguistic evidence in the text; do not make implicit inferences;
- **Single-layer principle**: Do not annotate nested entities; select the outermost entity with the broadest coverage;
- **Within-sentence principle**: Relations are annotated only between entity pairs within the same sentence; do not cross sentence boundaries.

---

## Chapter 2　Annotation Workflow

### 2.1 Semi-Automatic Annotation Pipeline

This dataset was produced using the pipeline: "GPT-5 (OpenAI, accessed November 2025) candidate generation → human verification and correction → multi-round iteration → IAA evaluation":

```
Raw sentence
   ↓
GPT-5 (OpenAI, accessed November 2025) generates candidate annotations (joint ERE)
   ↓
Annotator A verifies and corrects
   ↓
[Disputed cases] → Annotator B reviews → Adjudication
   ↓
Multi-round iteration (4 rounds total)
   ↓
~600 sentences (approximately 12% of the full corpus of 4,920 sentences) independently annotated by two annotators → IAA computed
   ↓
Final dataset release
```

### 2.2 Annotation Tools

- **Dataset construction phase**: No dedicated annotation platform was used. GPT-5 (OpenAI, accessed November 2025) outputs candidate annotations in JSON format; annotators manually verify and correct entity spans, types, and relation tuples directly in the JSON files.
- **IAA experiment phase**: Annotators independently produce annotations on raw text, outputting JSON files in the same format as the dataset; a script then computes strict span-level F1 for comparison (see Chapter 8).
- **Format conversion**: Python scripts (for conversion among LLM JSON / PLM json / PLM jsonl formats).

### 2.3 Annotation Phases

| Phase | Content |
|-------|---------|
| Round 1 | Pre-annotation: GPT-5-generated candidates reviewed and corrected by Annotator A, who has expertise in both computational linguistics and classical Chinese |
| Rounds 2–3 | Verification: two additional scholars of classical Chinese independently verify the annotations; systematic errors update guidelines and prompt templates; inter-verifier disagreements escalated to a senior expert in classical Chinese philology for adjudication |
| Round 4 | IAA experiment: the same two verifiers independently annotate ~600 sentences (approximately 12% of the full corpus of 4,920 sentences) from scratch (no LLM candidates); strict F1 computed |
| Post-processing | Apply systematic corrections uniformly across the full dataset |

---

## Chapter 3　Entity Annotation Standards

### General Rules

1. **Maximal span**: Annotate the complete place name including modifying words (e.g., annotate "Southern Song Imperial Palace" as a single entity);
2. **Do not annotate pronouns**: Generic referential expressions ("this place", "the said location") are not annotated;
3. **No nesting**: Select the outermost entity; do not annotate inner elements separately;
4. **Context-based judgement**: The same character string may belong to different types in different contexts.

---

### E01　Garden

**Definition**: Imperial pleasure gardens, private landscape gardens, and public scenic parks, including pavilions, towers, and associated scenic areas.

- **Includes**: Imperial gardens, palace pleasure grounds, private residential gardens, public scenic resorts, famous pavilions and towers
- **Excludes**: Residences serving purely residential functions (→ E19); ordinary domestic courtyards

| Source text | Entity | Type |
|-------------|--------|------|
| 集芳园在钱塘门外 | 集芳园 | Garden |
| 德寿宫有御园名小西湖 | 御园 | Garden |
| 聚景园乃御前花圃 | 聚景园 | Garden |

---

### E02　Mountain

**Definition**: Mountains, hills, ridges, peaks, slopes, and other elevated landforms, including named sub-features (peaks, ridges, valleys, rock formations).

- **Includes**: All named mountains, peaks, ridges, valleys, and rock formations
- **Excludes**: Temples, gardens, or other non-mountain entities named after mountains

| Source text | Entity | Type |
|-------------|--------|------|
| 凤凰山为临安主山 | 凤凰山 | Mountain |
| 吴山横亘城中 | 吴山 | Mountain |
| 灵隐山多奇石 | 灵隐山 | Mountain |

---

### E03　Bridge

**Definition**: All bridge structures, including timber bridges, stone bridges, and pontoon bridges.

- **Includes**: Bridges (桥), ford bridges (津)，weir-bridges (堰桥)
- **Excludes**: Pure ferry crossings without a bridge structure (→ E22 Transportation Facility)

| Source text | Entity | Type |
|-------------|--------|------|
| 望仙桥在清河坊北 | 望仙桥 | Bridge |
| 洗马桥跨中河 | 洗马桥 | Bridge |

---

### E04　Buddhist Temple

**Definition**: Buddhist monasteries, nunneries, chapels, and other Buddhist religious establishments.

- **Includes**: 寺 (monasteries), 院 (compounds), 庵 (nunneries), 精舍 (hermitages), 禅院 (Zen halls)
- **Excludes**: Taoist establishments (→ E17); secular sacrificial venues (→ E10)

| Source text | Entity | Type |
|-------------|--------|------|
| 灵隐寺在北山之麓 | 灵隐寺 | Buddhist Temple |
| 净慈寺当南屏山前 | 净慈寺 | Buddhist Temple |
| 上天竺法喜寺香火鼎盛 | 上天竺法喜寺 | Buddhist Temple |

---

### E05　Urban Ward

**Definition**: Functional zones, historic quarters, and ward subdivisions within the city, below the county level.

- **Includes**: 坊 (wards), 厢 (quarters), 铺 (sub-wards), 界 (zones), 里 (urban neighbourhoods)
- **Excludes**: Rural-level townships (→ E12); county-level administrative units (→ E11)

| Source text | Entity | Type |
|-------------|--------|------|
| 清河坊为临安商业中心 | 清河坊 | Urban Ward |
| 太平坊在吴山之北 | 太平坊 | Urban Ward |

---

### E06　Street

**Definition**: Named streets, alleys, covered arcades, and imperial avenues within the city.

- **Includes**: 街 (streets), 巷 (alleys), 弄 (lanes), 廊 (arcades), 御街 (imperial avenues), 大街 (main streets)
- **Excludes**: Bridges (→ E03); post roads and routes outside the city (→ E22)

| Source text | Entity | Type |
|-------------|--------|------|
| 御街南北贯通全城 | 御街 | Street |
| 炭桥巷居民稠密 | 炭桥巷 | Street |

---

### E07　Government Office

**Definition**: Office buildings and compounds of government agencies and military administrative bodies at all levels.

- **Includes**: 府/司/局/院/监/台 (civil administrative offices); 营/寨 (military administrative compounds)
- **Excludes**: Purely military defensive installations (→ E15); imperial palaces (→ E09)

| Source text | Entity | Type |
|-------------|--------|------|
| 临安府治在旧钱塘县 | 临安府 | Government Office |
| 太府寺掌国家财赋 | 太府寺 | Government Office |

---

### E08　Commercial Facility

**Definition**: Venues for commercial, handicraft, and storage activities, including markets, workshops, and warehouses.

- **Includes**: 市/行/铺 (commercial markets and shops); 务/场 (tax collection offices); 仓/库 (warehouses); 窑/坊 (workshops and kilns)
- **Excludes**: Streets serving a primarily entertainment or leisure function (→ E06); tax administrative offices of an official government nature (→ E07)

> **Note**: Venues whose primary function is entertainment and consumption (wine houses, theatres, teahouses) are classified as **E20 Civic Venue**, not E08.

| Source text | Entity | Type |
|-------------|--------|------|
| 米市桥附近有米市 | 米市 | Commercial Facility |
| 余杭门内有竹木场 | 竹木场 | Commercial Facility |

---

### E09　Palace

**Definition**: Imperial palace complexes and temporary imperial residences, including throne halls, galleries, and palace gates. Any structure bearing an emperor's designation or containing the characters 宫/殿 with an imperial character is classified here.

- **Includes**: 大内 (inner palace), 行宫 (temporary palaces), 宫/殿/阁 (imperial), 寝宫 (bedchambers), 御门 (imperial gates)
- **Excludes**: Imperial pleasure gardens (→ E01); palace administrative offices (→ E07)

| Source text | Entity | Type |
|-------------|--------|------|
| 大内在凤凰山之东麓 | 大内 | Palace |
| 德寿宫为太上皇驻跸之所 | 德寿宫 | Palace |
| 垂拱殿为皇帝日常理政之地 | 垂拱殿 | Palace |

---

### E10　Ritual Site

**Definition**: Venues for official or popular sacrificial rites, including altars, state shrines, ancestral temples, and community shrines.

- **Includes**: 坛 (altars for state rituals), 庙 (city god shrines, community temples), 祠 (shrines for historical figures), 社 (earth god shrines)
- **Excludes**: Buddhist establishments (→ E04); Taoist establishments (→ E17)

| Source text | Entity | Type |
|-------------|--------|------|
| 圜丘在龙山 | 圜丘 | Ritual Site |
| 城隍庙在吴山之上 | 城隍庙 | Ritual Site |

---

### E11　Administrative Division

**Definition**: Formal administrative territorial units at all levels, including circuits, prefectures, counties, and supervisory districts.

- **Includes**: 路 (circuits), 州/府 (prefectures), 县 (counties), 监 (supervisory districts)
- **Excludes**: Urban wards and quarters (→ E05); rural townships (→ E12)

| Source text | Entity | Type |
|-------------|--------|------|
| 临安府辖钱塘仁和两县 | 临安府 | Administrative Division |
| 浙西路治所在临安 | 浙西路 | Administrative Division |

> **Note**: The same name (e.g., "临安府") is classified as E11 when referring to the administrative territorial unit, and as E07 when referring to the office building compound. Context determines the classification.

---

### E12　Township

**Definition**: Rural settlements outside the city, including townships, villages, market towns, and sub-county communities.

- **Includes**: 乡 (townships), 都 (township-level units), 村 (villages), 镇 (market towns), 市镇, 埠 (rural wharves)
- **Excludes**: Urban wards and quarters (→ E05); county-level administrative units (→ E11)

| Source text | Entity | Type |
|-------------|--------|------|
| 临平镇市肆繁荣 | 临平镇 | Township |
| 余杭县有安吉乡 | 安吉乡 | Township |

---

### E13　Hydraulic Structure

**Definition**: Water management and irrigation engineering works, including embankments, dams, sluice gates, weirs, and reservoirs.

- **Includes**: 堤/坝 (embankments and dams), 闸 (sluice gates), 堰 (weirs, water management), 塘 (artificial reservoirs), 渠 (irrigation channels)
- **Excludes**: Natural water bodies (→ E16); weir-bridges with bridge function (→ E03)

| Source text | Entity | Type |
|-------------|--------|------|
| 龙山闸控江湖水位 | 龙山闸 | Hydraulic Structure |
| 上塘河有石堰一道 | 石堰 | Hydraulic Structure |

---

### E14　Historic Site

**Definition**: Historical remains, ruins, and heritage landmarks no longer in their original use.

- **Includes**: 故城 (former walled cities), 旧址/废址/遗址 (former sites and ruins), 古台 (ancient terraces), 古井 (abandoned wells)
- **Excludes**: Extant structures still in functioning use (annotate according to their actual type)

| Source text | Entity | Type |
|-------------|--------|------|
| 钱王故城在凤凰山麓 | 钱王故城 | Historic Site |
| 六和塔旧基尚存 | 六和塔旧基 | Historic Site |

---

### E15　Military Facility

**Definition**: Facilities and installations serving military defence or garrison functions, including city walls, passes, barracks, and naval stations.

- **Includes**: 城 (city walls), 关 (passes and gates), 寨 (military garrisons), 水寨 (naval stations), 城门 (city gates)
- **Excludes**: Military administrative offices (→ E07); post stations (→ E22)

| Source text | Entity | Type |
|-------------|--------|------|
| 候潮门为临安南城门 | 候潮门 | Military Facility |
| 浙江水军寨在龙山 | 浙江水军寨 | Military Facility |

---

### E16　Water Body

**Definition**: Natural or artificial water bodies, including rivers, lakes, canals, streams, harbours, ponds, and wells.

- **Includes**: 江/河/湖/溪/港/浦/潭/池 (natural water bodies); 渠 (artificial waterways)
- **Excludes**: Hydraulic engineering works (→ E13); pure ferry crossings without a water body name (→ E22)

| Source text | Entity | Type |
|-------------|--------|------|
| 西湖为临安第一胜景 | 西湖 | Water Body |
| 钱塘江潮势雄壮 | 钱塘江 | Water Body |
| 中河贯通城内南北 | 中河 | Water Body |

---

### E17　Taoist Temple

**Definition**: Taoist religious establishments, abbeys, and meditation compounds.

- **Includes**: 宫 (Taoist palaces), 观 (abbeys), 庵 (Taoist hermitages), 道院 (Taoist compounds)
- **Excludes**: Buddhist establishments (→ E04); shrines of a sacrificial rather than Taoist religious character (→ E10)

| Source text | Entity | Type |
|-------------|--------|------|
| 洞霄宫在余杭大涤山 | 洞霄宫 | Taoist Temple |
| 三茅观在吴山之上 | 三茅观 | Taoist Temple |

---

### E18　Educational Institution

**Definition**: Educational establishments of all types, including the Imperial Academy, prefectural schools, county schools, private academies, and charity schools.

- **Includes**: 太学 (Imperial Academy), 国子监 (Directorate of Education, educational function), 府学/县学 (prefectural and county schools), 书院 (private academies), 义学 (charity schools)
- **Excludes**: Administrative offices responsible for overseeing education (→ E07)

| Source text | Entity | Type |
|-------------|--------|------|
| 太学在雍熙寺之南 | 太学 | Educational Institution |
| 临安府学在凤凰山东 | 临安府学 | Educational Institution |

---

### E19　Official Residence

**Definition**: Private mansions, noble compounds, and high-official dwellings (non-imperial residential use).

- **Includes**: 府第/宅/第 (official residences), 旧宅/故居 (former residences), 别墅 (secondary residences, non-garden character)
- **Excludes**: Common civilian dwellings (→ E24); imperial palaces (→ E09)

| Source text | Entity | Type |
|-------------|--------|------|
| 韩侂胄府第在众安桥北 | 韩侂胄府第 | Official Residence |
| 贾似道别墅在葛岭 | 贾似道别墅 | Official Residence |

---

### E20　Civic Venue

**Definition**: Spaces for everyday urban entertainment and consumption, such as entertainment theatres, teahouses, renowned wine houses, and bathhouses.

- **Includes**: 瓦舍 (entertainment districts), 勾栏 (performance theatres), notable teahouses, landmark wine houses, bathhouses
- **Excludes**: Ordinary shops and workshops (→ E08); ordinary streets

> **Note**: Venues whose primary function is profit-oriented production, storage, or trade are classified as **E08 Commercial Facility**, not E20.

| Source text | Entity | Type |
|-------------|--------|------|
| 北瓦乃临安最大勾栏 | 北瓦 | Civic Venue |
| 丰乐楼酒香远播 | 丰乐楼 | Civic Venue |

---

### E21　Temporary Mortuary

**Definition**: Provisional resting places for imperial coffins, typically housed within Buddhist monasteries, and charity grave sites.

- **Includes**: 停柩所 (coffin resting rooms), 殡院 (mortuary courtyards), 义冢 (charity graves), 暂厝之地 (provisional interment sites)
- **Excludes**: Formal cemeteries (outside the scope of this dataset)

| Source text | Entity | Type |
|-------------|--------|------|
| 慧日永明院有寄柩房 | 慧日永明院寄柩房 | Temporary Mortuary |
| 上竺义冢收瘗游旅之骨 | 上竺义冢 | Temporary Mortuary |

---

### E22　Transportation Facility

**Definition**: Infrastructure for land and water transport, including post stations, ferry crossings, and wharves.

- **Includes**: 驿站 (post stations), 渡口 (ferry crossings without a bridge), 码头 (wharves), 铺 (relay stations)
- **Excludes**: Bridges (→ E03); passes of a military character (→ E15)

| Source text | Entity | Type |
|-------------|--------|------|
| 临安驿在候潮门外 | 临安驿 | Transportation Facility |
| 浙江渡口往来舟楫不绝 | 浙江渡口 | Transportation Facility |

---

### E23　Social Welfare Facility

**Definition**: Charitable, medical, and relief establishments, including poorhouses, orphanages, public infirmaries, and relief granaries.

- **Includes**: 惠民局 (benevolent dispensaries), 安济坊 (relief shelters), 慈幼局 (foundling homes), 漏泽园 (charity burial grounds)
- **Excludes**: Official medical administrative offices (→ E07)

| Source text | Entity | Type |
|-------------|--------|------|
| 安济坊收养城内贫病之人 | 安济坊 | Social Welfare Facility |
| 慈幼局育养遗弃婴儿 | 慈幼局 | Social Welfare Facility |

---

### E24　Civilian Dwelling

**Definition**: Common residential buildings specifically documented in the sources by name or location (not generic references to residential areas).

- **Includes**: 民宅 (civilian residences), 草舍/茅屋 (thatched dwellings, where individually recorded)
- **Excludes**: Official residences and noble mansions (→ E19); generic references to residential areas (do not annotate)

---

## Chapter 4　Relation Annotation Standards

### General Rules

1. **Directionality**: All relations are directed; the head entity is the source of the relation, the tail entity is the target;
2. **Within-sentence principle**: Do not annotate relations that cross sentence boundaries;
3. **Multiple relations**: The same entity pair may carry multiple relations of different types;
4. **Explicitness principle**: Annotate only relations explicitly expressed in the text; do not infer.

### Directional Convention

> In what follows, "A → B" means head = A, tail = B. "A ↔ B" marks symmetric relations.

---

### Topological Relations

| ID | Relation | Direction and Meaning | Example |
|----|----------|-----------------------|---------|
| R01 | **located at** | A → B: A is spatially situated at or within B (general locational statement) | 灵隐寺 **located at** 北山之麓 |
| R02 | **contains** | A → B: A geographically encloses B; A is always the larger or enclosing unit | 西湖 **contains** 苏堤 |
| R03 | **subordinate to** | A → B: A administratively belongs to B; A is always the smaller or lower-ranked unit | 钱塘县 **subordinate to** 临安府 |
| R25 | **at distance from** | A → B: A measured or explicitly stated distance separates A from B | 灵隐寺距西湖十里 |
| R26 | **adjacent to** | A ↔ B (symmetric): A and B share a boundary or are immediately next to each other | 望仙桥 **adjacent to** 清河坊 |
| R27 | **former site at** | A → B: The original historical location of A was at the place identified as B | 东府 **former site at** 候潮门内 |

---

### Naming Evolution Relations

| ID | Relation | Direction and Meaning | Example |
|----|----------|-----------------------|---------|
| R04 | **also known as** | A → B: B is a concurrent colloquial or unofficial name for A | 德寿宫 **also known as** 北内 |
| R05 | **formerly known as** | A → B: A was previously called B; B is the earlier name | 临安府 **formerly known as** 钱塘郡 |
| R28 | **rebuilt from** | A → B: A was constructed by renovating or repurposing B | 聚景园 **rebuilt from** 旧府 |
| R29 | **renamed to** | A → B: A was officially renamed and thereafter called B | 临安县 **renamed to** 钱塘县 |
| R30 | **relocated to** | A → B: A was physically moved from its original location to the site of B | 太学 **relocated to** 雍熙寺之南 |

---

### Route Relations

| ID | Relation | Direction and Meaning | Example |
|----|----------|-----------------------|---------|
| R06 | **leads to** | A → B: A (a road or waterway) runs toward or terminates at B | 御街 **leads to** 余杭门 |
| R31 | **starts at** | A → B: A (a route, river, or linear structure) has its origin at B | 御街 **starts at** 和宁门 |
| R32 | **ends at** | A → B: A (a route, river, or linear structure) has its terminus at B | 御街 **ends at** 余杭门 |
| R33 | **confluent with** | A ↔ B (symmetric): A and B (waterways or roads) meet or merge | 中河 **confluent with** 盐桥河 |
| R34 | **borders** | A ↔ B (symmetric): Two administrative or geographic regions share a formally demarcated boundary | 钱塘县 **borders** 仁和县 |

---

### Orientation Relations (16 types)

All orientation relations follow the convention **A → B: Entity A is located in the stated direction relative to Entity B**.

| ID | Relation | Example |
|----|----------|---------|
| R07 | **is east of** | 吴山 **is east of** 大内 |
| R08 | **is west of** | 钱塘门 **is west of** 大内 |
| R09 | **is south of** | 清波门 **is south of** 凤凰山 |
| R10 | **is north of** | 余杭门 **is north of** 大内 |
| R11 | **is southeast of** | 候潮门 **is southeast of** 大内 |
| R12 | **is northeast of** | 艮山门 **is northeast of** 大内 |
| R13 | **is southwest of** | 钱塘门 **is southwest of** 大内 |
| R14 | **is northwest of** | 余杭门 **is northwest of** 大内 |
| R15 | **is in front of** | 牌坊 **is in front of** 灵隐寺 |
| R16 | **is behind** | 后苑 **is behind** 垂拱殿 |
| R17 | **is left of** | 左厢 **is left of** 御街 |
| R18 | **is right of** | 右厢 **is right of** 御街 |
| R19 | **is beside** | 小院 **is beside** 大殿 (direction unspecified) |
| R20 | **is inside** | 御膳房 **is inside** 大内 (A → B: A is situated inside B) |
| R21 | **is outside** | 集芳园 **is outside** 钱塘门 (A → B: A is situated outside B) |
| R22 | **is above** | 城隍庙 **is above** 吴山 |
| R23 | **is below** | 灵隐寺 **is below** 飞来峰 |
| R24 | **is opposite** | 净慈寺 **is opposite** 雷峰塔 (symmetric; assign only when the source text explicitly states the two face each other) |

> **Distinguishing "contains" from "is inside":**
> - **contains** (R02): direction is A (larger) → B (smaller); A encloses B.
> - **is inside** (R20): direction is A (smaller) → B (larger); A is situated inside B.
> The two relations are inverses of each other; note the **opposite directions** when annotating.

---

## Chapter 5　Handling Complex Cases

### 5.1 Disputed Entity Boundaries

- In "大内之东", annotate only "大内"; "之东" encodes relational information and is not part of the entity span;
- Modifying dynastic qualifiers ("南宋", "皇宋") are generally not included in the entity name, except where they form a fixed proper noun (e.g., "南宋故宫");
- Numeral + classifier + place name (e.g., "三茅山") is annotated as a single entity.

### 5.2 Homonymous Locations

When multiple locations with the same name appear in a single text, annotate each as an independent entity based on context, and add a note. If the source text explicitly identifies two locations as sharing a name, a **also known as** relation may be annotated.

### 5.3 Nested Entities

Do not annotate nested entities. When nesting occurs (e.g., "临安府太府寺"), annotate the outermost entity as a whole; do not separately annotate the inner element. If two entities appear independently and in parallel in the text, annotate each separately.

### 5.4 Relation Directionality

- "A 在 B 之东" → head = A, tail = B, type = is east of;
- "B 之东为 A" → same direction as above;
- Symmetric relations (adjacent to, confluent with, borders, is opposite): annotate in one direction only; do not duplicate.

### 5.5 Implicit Relations

Annotate only relations for which there is explicit linguistic evidence. Example: "灵隐寺大殿" implies that the hall belongs to 灵隐寺, but if there is no explicit statement to this effect, the **contains** relation should not be annotated.

---

## Chapter 6　GPT-5-Assisted Annotation Prompts

### 6.1 Prompt Structure

The GPT-5 (OpenAI, accessed November 2025) annotation prompt has two layers:

- **System Prompt**: Role definition + enumeration of legal types with definitions + format constraints + few-shot examples
- **User Prompt**: Raw sentence to be annotated

### 6.2 System Prompt (Few-shot ERE version — final version after Round 3 iteration)

> **Iteration note**: This is the final version of the prompt after three rounds of human correction feedback. Compared with the initial version, the principal revisions are: (1) added explicit disambiguation notes to the definitions of **Commercial Facility** and **Civic Venue** to reduce misclassification of wine houses, teahouses, and similar venues; (2) added hierarchy clarifications to the definitions of **Urban Ward** and **Administrative Division**; (3) strengthened the directional disambiguation note distinguishing **contains** from **is inside** in the relation definitions; (4) expanded few-shot examples from 1 to 2, covering a wider range of relation type combinations. For a systematic analysis of the bias types identified and corrected, see Section 7.3.

```
You are a classical Chinese geographical and institutional information extraction engine.
Extract geographical entities and spatial relations from the given classical Chinese text.

[Legal entity types and definitions]
[Government Office] Ancient government office buildings and compounds.
[Administrative Division] Formally delimited territorial units of government used for local administration.
[Buddhist Temple] Buddhist religious establishments for monastic practice and lay worship.
[Ritual Site] Venues for sacrificial rites to deities or ancestors, encompassing both state and popular ritual.
[Transportation Facility] Facilities providing transport and relay services.
[Educational Institution] Educational and examination institutions and their internal buildings.
[Palace] Imperial residences and halls for governance.
[Garden] Scenic areas and building complexes for leisure and recreation.
[Taoist Temple] Taoist religious establishments for deity worship and ritual.
[Commercial Facility] Venues for economic activities and warehousing, whose primary function is profit-oriented production, storage, or taxation. (Note: wine houses, theatres, teahouses, and similar venues primarily serving entertainment and consumption belong to [Civic Venue], not this category.)
[Official Residence] Private dwellings of officials or wealthy individuals, reflecting status and residential culture.
[Mountain] Mountains and associated landforms in the natural geography.
[Urban Ward] Functional zones (wards, quarters, sub-districts) within the city. (Note: county-level and above administrative units belong to [Administrative Division]; rural settlements belong to [Township]; neither belongs here.)
[Bridge] Structures spanning waterways or obstacles to connect transport routes.
[Water Body] Natural or artificial water bodies.
[Military Facility] Facilities and entities related to military purposes and garrisons.
[Historic Site] Historically significant structures or ruins no longer in original use.
[Civilian Dwelling] Ordinary residential dwellings of common people.
[Street] Urban road networks and street names forming the city's framework.
[Township] Grass-roots administrative units or rural settlements.
[Temporary Mortuary] Provisional resting places for royal coffins, often within Buddhist monasteries.
[Hydraulic Structure] Water resource management and utilisation works ensuring irrigation and flood control.
[Civic Venue] Urban entertainment and consumption venues such as theatres, performance halls, renowned wine houses, and bathhouses. (Note: venues primarily serving profit-oriented production or warehousing belong to [Commercial Facility], not this category.)
[Social Welfare Facility] Establishments providing social assistance and welfare for vulnerable groups.

[Legal relation types and definitions]
[located at] Geographical entity 1 is located at geographical entity 2.
[contains] Geographical entity 1 contains geographical entity 2 (large → small; entity 1 is the enclosing space, entity 2 is within it). Note: direction is opposite to [is inside] — [is inside] means entity 1 (small) is inside entity 2 (large), i.e., small → large.
[subordinate to] Geographical entity 1 is subordinate to geographical entity 2 (small → large).
[also known as] Geographical entity 1 is also known as geographical entity 2.
[formerly known as] Geographical entity 1 was formerly known as geographical entity 2.
[renamed to] Geographical entity 1 was renamed to geographical entity 2.
[rebuilt from] Geographical entity 1 was built by repurposing geographical entity 2.
[relocated to] Geographical entity 1 was relocated to geographical entity 2.
[former site at] The former site of geographical entity 1 is at geographical entity 2.
[at distance from] There is an explicitly stated distance between geographical entity 1 and geographical entity 2.
[adjacent to] Two geographical entities are immediately next to each other.
[borders] Two geographical entities share a formally demarcated boundary.
[leads to] Geographical entity 1 leads to geographical entity 2.
[starts at] Geographical entity 1 has its starting point at geographical entity 2.
[ends at] Geographical entity 1 has its endpoint at geographical entity 2.
[confluent with] Geographical entity 1 and geographical entity 2 meet or merge.
[is east/south/west/north/southeast/northeast/southwest/northwest of] Geographical entity 1 is located in the stated direction relative to geographical entity 2.
[is inside/outside/above/below/in front of/behind/left of/right of/beside/opposite] Geographical entity 1 is in the stated position relative to geographical entity 2.

[CRITICAL format and constraint rules — follow strictly, character by character]
1) The input is a sentence from a classical Chinese text; this is the raw text you must process.
2) Use only the enumerated entity types above; generate no others.
3) Use only the enumerated relation types above; generate no others.
4) Return a single JSON object with two fields: ner and relations.
5) ner is a list of two-element arrays: ner = [["entity text", "entity type"], ...]
6) relations is a list of five-element arrays:
   relations = [["entity1 text", "entity1 type", "entity2 text", "entity2 type", "relation type"], ...]
7) Every entity in relations must appear in ner with a consistent type.
8) Output exactly one JSON object per input; keys are only ner and relations; no comments, Markdown, or explanatory text.
9) If the text contains no entities: {"ner": [], "relations": []}
10) If there are entities but no relations: output an empty list [] for relations.
11) Also extract pronouns or abbreviated references to entities.

[Examples — for reference only; do not copy into output]

Example 1
Input: "大内在凤皇山之东，以临安府旧治子城增筑。"
Output: {"ner": [["大内","Palace"],["凤皇山","Mountain"],["临安府","Administrative Division"]],
         "relations": [["大内","Palace","凤皇山","Mountain","is east of"],
                       ["大内","Palace","临安府","Administrative Division","located at"]]}

Example 2
Input: "南曰丽正门，门外建东西阙亭、百官待漏院。"
Output: {"ner": [["丽正门","Palace"],["东西阙亭","Government Office"],["百官待漏院","Government Office"]],
         "relations": [["丽正门","Palace","东西阙亭","Government Office","is south of"],
                       ["丽正门","Palace","百官待漏院","Government Office","is south of"]]}
```

### 6.3 User Prompt Format

```
Please return strictly in JSON structure:
{"ner":[["entity1 name","entity1 type"],...],
 "relations":[["entity1 name","entity1 type","entity2 name","entity2 type","relation"],...]}
Source text: ["sentence to be annotated"]
```

### 6.4 Two-Stage Decomposition (RE-only)

> **Note**: The production annotation for this dataset used the joint ERE approach described in Section 6.2, in which NER and relation extraction were performed simultaneously. The two-stage pipeline described in this section is a backup procedure intended for scenarios where NER has been separately confirmed by human annotators and only relation annotation remains; it was not used in the production phase.

When NER has already been confirmed by human annotators, relation extraction can be run independently:

**Key differences in System Prompt**:

```
You are an information extraction engine. NER is complete; given the entity list,
determine the relations between entity pairs based on the source text.
Use only the enumerated relation types; all others are strictly prohibited.
Output format: [["head entity","head type","tail entity","tail type","relation type"],...]
If no entity pair has a relation, output an empty list [].
Entity types in relations must match the types in the input ner list.
```

**User Prompt format**:

```
Source text: "sentence"
Entities ner: [["entity1","type1"],["entity2","type2"],...]
```

### 6.5 Model Parameters

| Parameter | Value |
|-----------|-------|
| Model | GPT-5 (OpenAI, accessed November 2025) |
| temperature | 0.3 (ERE) / 0.3 (RE-only) |
| Output format | Plain JSON; no Markdown code blocks |
| Batch mode | Sentence by sentence |
| Error handling | JSON parse failure → record status=failed; re-run in next pass |

---

## Chapter 7　Quality Control and Inter-Annotator Agreement

### 7.1 IAA Calculation Method

**Strict F1** is used as the agreement metric:

- **Entity match**: text span must be exactly identical **and** type must match
- **Relation match**: head entity, tail entity, and relation type must all match

**Final IAA** (based on a stratified sample of ~600 sentences, approximately 12% of the full corpus of 4,920 sentences):

| Task | F1 |
|------|----|
| Named Entity Recognition (NER) | **86.38%** |
| Relation Extraction (RE) | **77.82%** |

### 7.2 Dispute Adjudication Procedure

1. Consult the guidelines; if a clear rule applies, correct accordingly;
2. If the guidelines do not cover the case, a senior expert in classical Chinese philology adjudicates;
3. The adjudicated result becomes a positive example, and the corresponding rule is added to the guidelines.

### 7.3 Systematic Biases Identified and Corrected

| Bias type | Description |
|-----------|-------------|
| E08 vs. E20 | Confusion between Commercial Facility and Civic Venue (e.g., classification of wine houses) |
| E05 vs. E11 | Unclear boundary between Urban Ward and Administrative Division (e.g., the hierarchy of 坊) |
| R02 vs. R20 | Directional confusion between "contains" (A → B, A is large) and "is inside" (A → B, A is small) |

### 7.4 Release Standards

| Metric | Threshold |
|--------|-----------|
| Entity IAA F1 | ≥ 80% |
| Relation IAA F1 | ≥ 75% |
| Spot-check error rate | ≤ 5% (5% sample inspected each round) |

---

## Chapter 8　IAA Experiment — Dedicated Operating Procedures

This chapter sets out the operating procedures specific to the Round 4 IAA experiment. This experiment is **fundamentally distinct** from the production annotation phase: annotators must annotate from scratch, independently, without access to any GPT-5-generated candidates. Its purpose is to evaluate the operability of the guidelines themselves and the degree of consistency between annotators.

---

### 8.1 Experimental Setup and Basic Requirements

| Item | Specification |
|------|---------------|
| Participants | Annotator A and Annotator B (the same two scholars who performed verification in Rounds 2–3 of the production phase; a senior expert in classical Chinese philology served as adjudicator for disputed cases throughout production and IAA; four scholars participated in total across all phases) |
| Sampling method | Stratified sampling: covering all **19 text units (18 IND text units from 15 source works, plus 1 OOD text)** and all 6 functional entity categories |
| Sample size | Approximately 600 sentences (approximately 12% of the full corpus of 4,920 sentences; exact count determined at sampling) |
| Use of GPT-5 (OpenAI, accessed November 2025) candidates | **No** — neither annotator may view any pre-generated annotations |
| Mutual visibility | **No** — neither annotator may view the other's results until both have submitted |
| Guidelines version | Both annotators use **the final version of these guidelines (v1.0)** |
| Annotation tool | No dedicated annotation platform; annotators output JSON files directly in the dataset format |

---

### 8.2 Detailed Instructions for Annotators

1. **Annotate using only these guidelines**: Do not consult any annotation records from previous rounds; rely solely on the rules in these guidelines and your own interpretation of the text.
2. **Work independently; no discussion**: The two annotators must not discuss specific sentences or align their decisions before submitting their respective results.
3. **Handling uncertain cases**:
   - For uncertain entity boundaries, apply the **maximal span principle** (Chapter 3) and select the broadest defensible boundary;
   - For uncertain entity types, apply the **"Excludes"** clauses for each entity type to rule out alternatives progressively and select the most appropriate type;
   - For uncertain relation types or directionality, apply the general relation rules (Chapter 4) and the complex case procedures (Chapter 5);
   - If uncertainty persists after the above steps, annotate according to your best judgement; **do not leave blanks**.
4. **Do not consult the production dataset**: Do not refer to the annotation output already in the dataset during the experiment.
5. **Annotate all sentences completely**: Do not skip sentences or leave them blank on grounds of difficulty.

---

### 8.3 Summary of Boundary Rules Most Likely to Cause Disagreement

The following are the points most likely to produce divergence in the IAA experiment; annotators should review them carefully before beginning:

| Rule | Entities involved | Key decision point |
|------|-------------------|--------------------|
| E11 vs. E07 | Administrative Division vs. Government Office | Determine by **contextual function**: refers to territorial extent → E11; refers to the office building compound → E07 |
| E04 vs. E17 vs. E10 | Buddhist Temple / Taoist Temple / Ritual Site | Distinguish by **religious affiliation**: Buddhist → E04; Taoist → E17; non-religious sacrificial → E10 |
| E08 vs. E20 | Commercial Facility vs. Civic Venue | Distinguish by **primary function**: profit-oriented production/storage → E08; entertainment/consumption experience → E20 |
| E01 vs. E09 | Garden vs. Palace | Contains 宫/殿 and serves imperial residential/governance function → E09; scenic pleasure ground → E01 |
| R02 vs. R20 | contains vs. is inside | contains: A (large) → B (small); is inside: A (small) → B (large); directions are **opposite** |
| R07–R24 | Orientation relations | head = the located entity (A); tail = the reference entity (B); "A 在 B 之东" → head = A, tail = B |
| R04 vs. R05 vs. R29 | also known as / formerly known as / renamed to | also known as: concurrent names; formerly known as: A was previously called B (B is the older name); renamed to: A was renamed B (B is the new name) |

---

### 8.4 Submission and Adjudication Procedure

1. Both annotators **independently complete** the full set of ~600 sentences and submit their annotations as JSON files;
2. The project lead runs the comparison script to compute **strict F1** (method as per Section 7.1) on the two JSON files;
3. For cases where the two annotations disagree, first check whether the case falls into the high-frequency divergence types in Section 8.3;
4. Adjudication follows the same procedure as Section 7.2: consult the guidelines → third-party adjudication → add rule to guidelines.

---

*These guidelines are version v1.0. If updated, please use the most recent version.*
