# A Dataset of Geographic Entities and Positional Relations from Southern Song Dynasty Texts on Lin'an

[![License: CC BY 4.0](https://img.shields.io/badge/License-CC%20BY%204.0-lightgrey.svg)](https://creativecommons.org/licenses/by/4.0/)
[![DOI](https://img.shields.io/badge/DOI-10.5281%2Fzenodo.18974470-blue)](https://doi.org/10.5281/zenodo.18974470)

---

## Overview

GEPR-LINAN is a manually annotated dataset for named entity recognition (NER) and relation extraction (RE) from Classical Chinese historical texts. The dataset focuses on geographical entities and spatial relationships described in texts about **Lin'an** (临安, present-day Hangzhou, Zhejiang Province, China), the capital of the Southern Song Dynasty (1127–1279 CE).

Lin'an was one of the largest and most prosperous cities in the medieval world. Its spatial organisation — encompassing palaces, government offices, Buddhist and Taoist establishments, markets, gardens, waterways, and street networks — is documented in exceptional detail across a range of surviving local gazetteers and miscellaneous notes. These texts provide a foundation for computational reconstruction of the city's historical geography but pose significant challenges for NLP systems due to archaic vocabulary, elliptical syntax, and dense spatial expressions.

The dataset provides annotations for **24 fine-grained geographical entity types** and **34 spatial and semantic relationship types**, covering 4,920 sentences drawn from 17 in-domain (IND) classical Chinese works and one out-of-distribution (OOD) source.

| Metric | Value |
|--------|-------|
| Entity types | 24 |
| Relation types | 34 |
| Total sentences | 4,920 |
| Total entities | 25,397 |
| Total relations | 17,881 |
| Source texts (IND) | 17 works / 88 volumes |
| Source texts (OOD) | 1 document / 2 volumes |
| IAA — Entity F1 | 86.38% |
| IAA — Relation F1 | 77.82% |

---

## Source Texts

### In-Domain Corpus (IND)

The 17 IND works are authoritative Song-dynasty or retrospective scholarly works specifically focused on Lin'an. All texts were obtained from the *Zhongguo Zhexue Shu Dianzihua Jihua* (Chinese Text Project, [ctext.org](https://ctext.org)) or from the *Siku Quanshu* (Wenyuange copy) via manual transcription.

| Title (Chinese) | Title (English) | Author | Date | Genre |
|-----------------|-----------------|--------|------|-------|
| 乾道临安志（卷1–2） | *Qiandao Lin'an Zhi*, vols. 1–2 | 周淙 Zhou Cong | c. 1169 CE | Local gazetteer |
| 二老堂杂志（卷4） | *Erlaotang Zazhi*, vol. 4 | 尤袤 You Mao | Late 12th c. | Miscellaneous notes |
| 六和塔记 | *Liuhe Ta Ji* | Anonymous | 12th–13th c. | Monograph record |
| 南渡大小官署考 | *Nandu Daxiao Guanshu Kao* | Anonymous (Southern Song) | 13th c. | Institutional study |
| 南渡行宫记 | *Nandu Xinggong Ji* | Anonymous | 13th c. | Palace record |
| 咸淳临安志（选卷） | *Xianchun Lin'an Zhi* (selected volumes) | 潜说友 Qian Shuoyou | 1268 CE | Local gazetteer |
| 增补武林旧事（卷4,6,7,8） | *Wulin Jiushi* (augmented edition), vols. 4, 6, 7, 8 | 周密 Zhou Mi (original); supplemented by later editors | c. 1280, suppl. 14–16th c. | Urban miscellany |
| 宋行宫考 | *Song Xinggong Kao* | Anonymous (Qing) | 18th c. | Antiquarian study |
| 方舆胜览（卷1） | *Fangyu Shenglan*, vol. 1 | 祝穆 Zhu Mu | c. 1239 CE | Geographic encyclopedia |
| 杭州宋宫考 | *Hangzhou Song Gong Kao* | 齐召南 Qi Zhaonan | 18th c. (Qianlong reign) | Antiquarian study |
| 杭州宋祀典考 | *Hangzhou Song Sidian Kao* | 齐召南 Qi Zhaonan | 18th c. (Qianlong reign) | Antiquarian study |
| 杭宋勋臣郎官宅考 | *Hang Song Xunchen Langguan Zhai Kao* | 齐召南 Qi Zhaonan | 18th c. (Qianlong reign) | Antiquarian study |
| 杭州宋官署考 | *Hangzhou Song Guanshu Kao* | 齐召南 Qi Zhaonan | 18th c. (Qianlong reign) | Antiquarian study |
| 梦梁录（卷7–16, 19） | *Meng Liang Lu*, vols. 7–16, 19 | 吴自牧 Wu Zimu | c. 1274 CE | Urban miscellany |
| 武林旧事（卷4–6） | *Wulin Jiushi* (original edition), vols. 4–6 | 周密 Zhou Mi | c. 1280–1290 CE | Urban miscellany |
| 淳祐临安志（卷5–10） | *Chunyou Lin'an Zhi*, vols. 5–10 | 施谔 Shi E | c. 1252 CE | Local gazetteer |
| 西湖老人繁盛录 | *Xihu Laoren Fansheng Lu* | 西湖老人 (anonymous pen name) | Early 13th c. | Urban miscellany |
| 都城纪胜 | *Ducheng Jisheng* | 耐得翁 Nai De Weng (anon.) | c. 1235 CE | Urban miscellany |

### Out-of-Distribution Corpus (OOD)

| Title (Chinese) | Title (English) | Author | Date | Genre |
|-----------------|-----------------|--------|------|-------|
| 舆地纪胜 (卷1–2) | *Yuedi Jisheng*, vols. 1–2 | 王象之 Wang Xiangzhi | c. 1221 CE | Geographic encyclopedia |

*Yuedi Jisheng* is a large-scale Song-dynasty geographic encyclopedia covering all prefectures of the empire (200+ volumes). Volumes 1–2, which cover Lin'an prefecture, are used as the OOD evaluation set. Although the subject matter overlaps with the IND corpus, this text represents an independent authorial and methodological tradition (empire-wide encyclopaedic compilation rather than locally focused description), producing measurably different distributional properties: mean relation density 2.11 per sentence (vs. 3.77–4.14 in IND splits), mean sentence length 69 characters (vs. 45–52), and 67 sentences with no annotated relations (vs. 0 in IND).

---

## Repository Structure

```
GEPR-LINAN-NEW/
├── README.md                        # This file
│
├── LLM/                             # LLM-format annotations (text-span based)
│   ├── train.json
│   ├── dev.json
│   ├── test.json
│   └── ood.json
│
├── PLM/
│   ├── json-format/                 # Token-index format (SPERT / PURE compatible)
│   │   ├── train.json
│   │   ├── dev.json
│   │   ├── test.json
│   │   └── ood.json
│   └── jsonl-format/                # Paragraph-level JSONL (DyGIE++ compatible)
│       ├── train.jsonl
│       ├── dev.jsonl
│       ├── test.jsonl
│       └── ood.jsonl
│
├── mappings/                        # Bidirectional ID mapping files
│   ├── doc_id_mapping.json          # Document title → DOC_XXXX
│   ├── doc_meta.json                # DOC_XXXX → {title, split, sentence_count, …}
│   ├── doc_to_sent_mapping.json     # DOC_XXXX → [SENT_XXXXXX, …]
│   ├── sent_to_doc_mapping.json     # SENT_XXXXXX → DOC_XXXX
│   ├── doc_to_para_mapping.json     # DOC_XXXX → [PARA_XXXXXX, …]
│   ├── para_to_doc_mapping.json     # PARA_XXXXXX → DOC_XXXX
│   ├── para_to_sent_mapping.json    # PARA_XXXXXX → [SENT_XXXXXX, …]
│   └── sent_to_para_mapping.json    # SENT_XXXXXX → PARA_XXXXXX
│
└── supplementary/
    ├── annotation_guidelines_zh.md  # Full annotation guidelines (Chinese, with GPT-5 prompts)
    ├── codebook_en.json             # English codebook: all 24 entity types + 34 relation types
    ├── dataset_statistics.json      # Computed statistics per split
    ├── source_texts_metadata.json   # Bibliographic metadata for all source texts
    ├── annotation_metadata.json     # Annotation pipeline, IAA results, model details
    ├── DATA_RECORD.md               # Field-level documentation for all formats
    └── SUPPLEMENTARY_INDEX.md       # Index cross-referencing reviewer comments
```

---

## Data Formats

### LLM Format (`LLM/*.json`)

A JSON array where each element represents one sentence. Entities are stored as text spans (no positional indices), making the format suitable for in-context learning and prompt-based experiments.

```json
{
  "doc_id": "DOC_0007",
  "sent_id": "SENT_000123",
  "sentence": "大内在凤皇山之东，以临安府旧治子城增筑。",
  "ner": [["大内", "宫殿"], ["凤皇山", "山脉"], ["临安府", "行政区划"]],
  "relations": [
    ["大内", "宫殿", "凤皇山", "山脉", "方位关系（东）"],
    ["大内", "宫殿", "临安府", "行政区划", "位于"]
  ]
}
```

### PLM JSON Format (`PLM/json-format/*.json`)

Sentence-level JSON array with character-level token indices (exclusive end). Compatible with SPERT, PURE, PL-Marker, and similar span-based models.

```json
{
  "doc_id": "DOC_0007",
  "sent_id": "SENT_000123",
  "tokens": ["大", "内", "在", "凤", "皇", "山", ...],
  "entities": [
    {"start": 0, "end": 2, "type": "宫殿"},
    {"start": 3, "end": 6, "type": "山脉"}
  ],
  "relations": [
    {"head": 0, "tail": 1, "type": "方位关系（东）"}
  ]
}
```

### PLM JSONL Format (`PLM/jsonl-format/*.jsonl`)

Paragraph-level JSONL where each line is a paragraph of up to 5 consecutive sentences from the same document. Token indices are cumulative across the paragraph (inclusive end). Compatible with DyGIE++ and similar document-level models.

```json
{"doc_key": "DOC_0007_PARA_000025", "sentences": [["大","内",...], ...],
 "ner": [[[0,1,"宫殿"],[3,5,"山脉"]], ...],
 "relations": [[[0,1,3,5,"方位关系（东）"]], ...]}
```

See `supplementary/DATA_RECORD.md` for complete field definitions and indexing conventions.

---

## Annotation Pipeline

Annotations were produced using a semi-automatic pipeline:

1. **Candidate generation**: GPT-5 (temperature 0.3) was prompted with a structured system prompt specifying all 24 entity types and 34 relation types, using few-shot examples drawn from the Lin'an corpus. Both joint ERE (entity + relation in one pass) and two-stage NER→RE modes were employed.
2. **Human verification (Rounds 1–3)**: Two annotators with backgrounds in computational linguistics and classical Chinese philology reviewed and corrected the GPT-5-generated candidate annotations directly in the JSON output files, correcting entity spans, types, and relation labels as needed.
3. **Iterative refinement**: Annotation guidelines were updated across four rounds based on systematic error analysis. Low-confidence and disputed cases were adjudicated by a third reviewer.
4. **IAA evaluation (Round 4)**: Approximately 5,000 sentences, stratified across all source texts, were presented to both annotators as plain text in Label Studio — with no pre-generated labels and no access to each other's annotations — and each annotator produced annotations independently from scratch. Strict F1 was computed as the inter-annotator agreement metric.

Full prompt templates and annotation decision rules are documented in `supplementary/annotation_guidelines_zh.md`.

**Note on anchoring bias**: In the production phase, annotators corrected pre-existing model proposals rather than annotating from scratch. This introduces a potential anchoring effect that the IAA experiment — conducted on raw text without model pre-labelling — does not fully capture. Users should be aware of this limitation when interpreting annotation consistency.

---

## Entity and Relation Types

### Entity Types (24)

| Chinese | English | Total |
|---------|---------|-------|
| 园林 | Garden / Park | 2,678 |
| 山脉 | Mountain / Ridge | 2,563 |
| 桥梁 | Bridge | 2,502 |
| 寺院 | Buddhist Temple | 2,480 |
| 城市内部区划 | Urban District | 1,956 |
| 官署 | Government Office | 1,644 |
| 街道 | Street / Lane | 1,566 |
| 水系 | Water Body | 1,413 |
| 经济活动设施 | Commercial Facility | 1,253 |
| 宫殿 | Palace | 1,044 |
| 行政区划 | Administrative Division | 959 |
| 乡镇 | Township / Village | 938 |
| 祭祀场所 | Ritual / Sacrificial Site | 794 |
| 古迹 | Historic Ruin | 740 |
| 水利设施 | Hydraulic Structure | 655 |
| 军事设施 | Military Facility | 541 |
| 学校 | School / Academy | 445 |
| 道观 | Taoist Temple | 367 |
| 府邸宅邸 | Official Residence / Mansion | 378 |
| 市民生活 | Civic / Entertainment Venue | 240 |
| 暂殡处所 | Temporary Mortuary | 98 |
| 交通设施 | Transportation Facility | 80 |
| 社会福利设施 | Social Welfare Facility | 32 |
| 民居 | Civilian Dwelling | 31 |

### Relation Types (34)

| Chinese | English | Total |
|---------|---------|-------|
| 隶属 | Subordinate to | 2,843 |
| 包含 | Contains | 2,842 |
| 位于 | Located at | 2,815 |
| 别名 | Alias of | 879 |
| 方位关系（外） | Outside of | 805 |
| 旧名 | Former name of | 691 |
| 通往 | Leads to | 652 |
| 方位关系（西） | West of | 641 |
| 方位关系（北） | North of | 575 |
| 方位关系（东） | East of | 563 |
| 方位关系（南） | South of | 546 |
| 距离 | Distance from | 435 |
| 方位关系（内） | Inside of | 410 |
| 方位关系（前） | In front of | 374 |
| 方位关系（后） | Behind | 357 |
| 相邻 | Adjacent to | 230 |
| 旧址位于 | Former site at | 225 |
| 方位关系（侧） | Beside | 216 |
| 方位关系（下） | Below | 201 |
| 方位关系（相对） | Opposite | 180 |
| 改建 | Rebuilt from | 179 |
| 更名 | Renamed to | 152 |
| 终点 | Endpoint at | 139 |
| 起点 | Starting point at | 123 |
| 方位关系（上） | Above | 125 |
| 交汇 | Confluence with | 112 |
| 迁移 | Relocated to | 91 |
| 方位关系（右） | Right of | 83 |
| 交界 | Borders | 81 |
| 方位关系（左） | Left of | 79 |
| 方位关系（西南） | Southwest of | 74 |
| 方位关系（东北） | Northeast of | 68 |
| 方位关系（东南） | Southeast of | 48 |
| 方位关系（西北） | Northwest of | 47 |

For type definitions, boundary rules, and annotated examples, see `supplementary/annotation_guidelines_zh.md` and `supplementary/codebook_en.json`.

---

## Data Access

The dataset is available at: **https://doi.org/10.5281/zenodo.18974470**

All files are encoded in UTF-8 and released under CC BY 4.0. The repository contains no personally identifiable information. All primary source texts are in the public domain (pre-modern works, digitised editions from ctext.org under open access).

---

## License

This dataset is released under the [Creative Commons Attribution 4.0 International (CC BY 4.0)](https://creativecommons.org/licenses/by/4.0/) license. You are free to share and adapt the material for any purpose, provided appropriate credit is given.

---

## Citation

If you use this dataset, please cite:

```bibtex
@article{zhang2025geprlinan,
  title     = {{GEPR-LINAN}: A Dataset of Geographic Entities and Positional Relations
               from Southern Song Dynasty Texts on Lin'an},
  author    = {Zhang, Chi and Zhong, Fengchen and Chen, Zuohui and Wu, Wei and Pan, Debao},
  journal   = {Scientific Data},
  year      = {2025},
  note      = {Under review}
}
```

Data citation (once DOI is assigned):

> Zhang, C., Zhong, F., Chen, Z., Wu, W., & Pan, D. (2025). *GEPR-LINAN: A Dataset of Geographic Entities and Positional Relations from Southern Song Dynasty Texts on Lin'an* [Data set]. Zenodo. https://doi.org/10.5281/zenodo.18974470

---

## Contact

Correspondence: Zuohui Chen — czuohui@zjut.edu.cn
Zhejiang University of Technology
