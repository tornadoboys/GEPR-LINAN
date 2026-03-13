# GEPR-LINAN-NEW: Data Record Documentation

This document provides field-level descriptions of every file in the GEPR-LINAN-NEW dataset.
It is intended to satisfy the Data Record requirements of Scientific Data.

---

## Repository Structure

```
GEPR-LINAN-NEW/
├── LLM/                          # Format 1: LLM-ready sentence-level JSON
│   ├── train.json
│   ├── dev.json
│   ├── test.json
│   └── ood.json
├── PLM/
│   ├── json-format/              # Format 2: Sentence-level JSON with token indices (SPERT-compatible)
│   │   ├── train.json
│   │   ├── dev.json
│   │   ├── test.json
│   │   ├── ood.json
│   │   └── types.json
│   └── jsonl-format/             # Format 3: Paragraph-level JSONL (PURE / PL-Marker compatible)
│       ├── train.jsonl
│       ├── dev.jsonl
│       ├── test.jsonl
│       └── ood.jsonl
├── mappings/                     # Document/sentence/paragraph ID cross-reference files
│   ├── doc_id_mapping.json
│   ├── doc_meta.json
│   ├── doc_to_sent_mapping.json
│   ├── sent_to_doc_mapping.json
│   ├── doc_to_para_mapping.json
│   ├── para_to_doc_mapping.json
│   ├── para_to_sent_mapping.json
│   └── sent_to_para_mapping.json
└── supplementary/                # Supplementary materials
    ├── annotation_guidelines_zh.md
    ├── bibliography.md
    ├── codebook_en.json
    ├── dataset_statistics.json
    ├── annotation_metadata.json
    └── DATA_RECORD.md  (this file)
```

---

## Data Splits

| Split   | Role                        | Source |
|---------|-----------------------------|--------|
| `train` | Model training              | IND    |
| `dev`   | Validation / hyperparameter | IND    |
| `test`  | Final in-distribution test  | IND    |
| `ood`   | Out-of-distribution test    | OOD    |

**Split strategy**: Splits are performed at the *document (volume)* level to prevent information
leakage. No sentence from the same document volume appears in more than one split.
Random seed: 42. Approximate ratio: 80 / 10 / 10 for train / dev / test.

---

## Format 1: LLM/ — Sentence-Level JSON

### File: `LLM/{train,dev,test,ood}.json`

**File format**: JSON array. Each element is one annotated sentence.

**Schema**:

```json
{
  "doc_id":    "DOC_0001",
  "sent_id":   "SENT_000001",
  "sentence":  "大内在凤皇山之东，以临安府旧治子城增筑。",
  "ner": [
    ["大内",  "宫殿"],
    ["凤皇山","山脉"],
    ["临安府","行政区划"]
  ],
  "relations": [
    ["大内", "宫殿", "凤皇山", "山脉", "方位关系（东）"]
  ]
}
```

| Field       | Type            | Description |
|-------------|-----------------|-------------|
| `doc_id`    | string          | Unique document-volume identifier in the form `DOC_XXXX` (padded 4-digit integer). Maps to `mappings/doc_id_mapping.json` and `mappings/doc_meta.json`. |
| `sent_id`   | string          | Unique sentence identifier in the form `SENT_XXXXXX` (padded 6-digit integer). Maps to `mappings/sent_to_doc_mapping.json` and `mappings/sent_to_para_mapping.json`. |
| `sentence`  | string          | Original Classical Chinese sentence as it appears in the source text. No normalisation is applied. |
| `ner`       | array of arrays | Each inner array has exactly 2 elements: `[entity_text, entity_type]`. Lists all named entities annotated in the sentence. Empty array if no entities. |
| `relations` | array of arrays | Each inner array has exactly 5 elements: `[head_entity_text, head_entity_type, tail_entity_text, tail_entity_type, relation_type]`. Empty array if no relations are annotated for this sentence. |

**Notes**:
- Entity texts are verbatim substrings of `sentence`.
- Relation head/tail texts must both appear in the corresponding `ner` list.
- Entity and relation type strings match the keys in `PLM/json-format/types.json`.
- This format is suitable for prompting LLMs directly (no tokenisation required).

---

## Format 2: PLM/json-format/ — Sentence-Level JSON with Token Indices

### File: `PLM/json-format/{train,dev,test,ood}.json`

**File format**: JSON array. Each element is one annotated sentence (SPERT/SpERT-compatible).

**Schema**:

```json
{
  "doc_id":    "DOC_0001",
  "sent_id":   "SENT_000001",
  "tokens":    ["大","内","在","凤","皇","山","之","东","，","以","临","安","府","旧","治","子","城","增","筑","。"],
  "entities":  [
    {"type": "宫殿",    "start": 0,  "end": 2},
    {"type": "山脉",    "start": 3,  "end": 6},
    {"type": "行政区划","start": 10, "end": 13}
  ],
  "relations": [
    {"type": "方位关系（东）", "head": 0, "tail": 1}
  ]
}
```

| Field       | Type            | Description |
|-------------|-----------------|-------------|
| `doc_id`    | string          | Unique document-volume identifier in the form `DOC_XXXX`. Maps to `mappings/doc_id_mapping.json` and `mappings/doc_meta.json`. |
| `sent_id`   | string          | Unique sentence identifier in the form `SENT_XXXXXX`. Maps to `mappings/sent_to_doc_mapping.json` and `mappings/sent_to_para_mapping.json`. |
| `tokens`    | array of strings| Character-level tokenisation of the sentence (one Unicode character per token). |
| `entities`  | array of objects| Each object has `type` (string), `start` (int, inclusive), `end` (int, **exclusive**). Span `[start, end)` indexes into `tokens`. |
| `relations` | array of objects| Each object has `type` (string), `head` (int), `tail` (int). `head` and `tail` are indices into the `entities` array of **this sentence**. |

**Notes**:
- `entities[i].end` is exclusive (Python slice convention): `''.join(tokens[start:end])` recovers the entity surface form.
- `relations[j].head` and `relations[j].tail` reference entity array indices, **not** token indices.
- All sentences within the same split are guaranteed to belong to different documents than sentences in other splits.

### File: `PLM/json-format/types.json`

**File format**: JSON object with two top-level keys: `"entities"` and `"relations"`.

```json
{
  "entities": {
    "宫殿": {"short": "宫殿", "verbose": "宫殿"},
    ...
  },
  "relations": {
    "位于": {"short": "位于", "verbose": "位于", "symmetric": false, "transitive": false},
    ...
  }
}
```

| Field        | Description |
|--------------|-------------|
| `short`      | Abbreviated label (used as display name in tools). |
| `verbose`    | Full label. |
| `symmetric`  | (relations only) `true` if the relation holds in both directions (e.g., 相邻 / Adjacent to). |
| `transitive` | (relations only) `true` if transitivity applies. All relations in this dataset are non-transitive. |

**English translations** for all type labels are provided in `supplementary/codebook_en.json`.

---

## Format 3: PLM/jsonl-format/ — Paragraph-Level JSONL

### File: `PLM/jsonl-format/{train,dev,test,ood}.jsonl`

**File format**: JSONL (JSON Lines). Each line is a JSON object representing one *paragraph*
(PURE / PL-Marker compatible).

**Paragraph definition**: Up to 5 consecutive sentences from the **same document volume**.
Paragraphs never cross document or volume boundaries.

**Schema** (one line):

```json
{
  "doc_id":    "DOC_0001",
  "para_id":   "PARA_000001",
  "sentences": [
    ["大","内","在","凤","皇","山","之","东","，","以","临","安","府","旧","治","子","城","增","筑","。"],
    ["南","曰","丽","正","门","，",...],
    ...
  ],
  "ner": [
    [[0, 1, "宫殿"], [3, 5, "山脉"], [10, 12, "行政区划"]],
    [[20, 21, "宫殿"], [23, 25, "山脉"], ...],
    ...
  ],
  "relations": [
    [[0, 1, 3, 5, "方位关系（东）"]],
    [[42, 44, 20, 21, "位于"], ...],
    ...
  ]
}
```

| Field       | Type                      | Description |
|-------------|---------------------------|-------------|
| `doc_id`    | string                    | Unique document-volume identifier in the form `DOC_XXXX`. Maps to `mappings/doc_id_mapping.json` and `mappings/doc_meta.json`. |
| `para_id`   | string                    | Unique paragraph identifier in the form `PARA_XXXXXX`. Maps to `mappings/para_to_sent_mapping.json` and `mappings/para_to_doc_mapping.json`. |
| `sentences` | array of arrays           | Outer array: one element per sentence. Inner array: character-level tokens of that sentence. |
| `ner`       | array of arrays of arrays | Outer: one element per sentence. Inner: list of entity spans `[start, end_incl, type]` where indices are **cumulative across all sentences in this paragraph** and `end_incl` is **inclusive**. |
| `relations` | array of arrays of arrays | Outer: one element per sentence. Inner: list of relation tuples `[head_start, head_end_incl, tail_start, tail_end_incl, relation_type]` where all indices are **cumulative across this paragraph** and end indices are **inclusive**. |

**Critical indexing note**:
- Token indices in `ner` and `relations` are **global within the paragraph**.
  - If sentence 1 has 20 tokens (indices 0–19) and sentence 2 has 21 tokens, then sentence 2's
    tokens are indexed 20–40.
- End indices are **inclusive** (unlike Format 2 where ends are exclusive).
  - To recover entity text: `''.join(all_tokens[start : end_incl + 1])`.

---

## Mappings/

All mapping files are JSON objects (dictionaries).

| File | Key | Value | Description |
|------|-----|-------|-------------|
| `doc_id_mapping.json` | Document title string (e.g., `"《乾道临安志》卷1"`) | `"DOC_XXXX"` | Maps source text title+volume to canonical document ID. |
| `doc_meta.json` | `"DOC_XXXX"` | `{title, split, sentence_count, para_count}` | Metadata for each document volume: human-readable title, assigned split, and counts. |
| `doc_to_sent_mapping.json` | `"DOC_XXXX"` | `["SENT_XXXXXX", ...]` | All sentence IDs belonging to a document, in original text order. |
| `sent_to_doc_mapping.json` | `"SENT_XXXXXX"` | `"DOC_XXXX"` | Reverse lookup: which document a sentence belongs to. |
| `doc_to_para_mapping.json` | `"DOC_XXXX"` | `["PARA_XXXXXX", ...]` | All paragraph IDs belonging to a document, in original text order. |
| `para_to_doc_mapping.json` | `"PARA_XXXXXX"` | `"DOC_XXXX"` | Reverse lookup: which document a paragraph belongs to. |
| `para_to_sent_mapping.json` | `"PARA_XXXXXX"` | `["SENT_XXXXXX", ...]` | The sentence IDs (2–5) composing a paragraph, in order. |
| `sent_to_para_mapping.json` | `"SENT_XXXXXX"` | `"PARA_XXXXXX"` | Reverse lookup: which paragraph a sentence belongs to. |

---

## ID Naming Conventions

| ID Format | Example | Scope |
|-----------|---------|-------|
| `DOC_XXXX` | `DOC_0001` | Document volume (4-digit zero-padded integer, 1–90) |
| `SENT_XXXXXX` | `SENT_000001` | Individual sentence (6-digit zero-padded integer) |
| `PARA_XXXXXX` | `PARA_000001` | Paragraph of ≤5 sentences (6-digit zero-padded integer) |

---

## Cross-Format Consistency

Each sentence exists in all three formats with the same annotations:

| Format | Sentence identifier | Document identifier | Paragraph identifier | Entity/relation encoding |
|--------|--------------------|--------------------|---------------------|--------------------------|
| LLM    | `sent_id` (SENT_XXXXXX) | `doc_id` (DOC_XXXX) | — | Text strings |
| PLM json | `sent_id` (SENT_XXXXXX) | `doc_id` (DOC_XXXX) | — | Token index spans (exclusive end) |
| PLM jsonl | — | `doc_id` (DOC_XXXX) | `para_id` (PARA_XXXXXX) | Cumulative token index spans (inclusive end) |

To cross-reference: use `mappings/sent_to_para_mapping.json` to find which paragraph
(and thus which jsonl line) contains a given sentence from the LLM or json-format files.
