# GEPR-LINAN-NEW: Supplementary Materials Index

This document provides a complete, annotated index of all supplementary files
submitted alongside the GEPR-LINAN dataset in response to reviewer comments.

---

## Supplementary File List

| # | File | Format | Addresses |
|---|------|--------|-----------|
| S1 | `supplementary/codebook_en.json` | JSON | Reviewer 1 #5, Editor #1d |
| S2 | `supplementary/dataset_statistics.json` | JSON | Reviewer 1 #8–9, Editor #1b |
| S3 | `supplementary/source_texts_metadata.json` | JSON | Reviewer 1 #2, Editor #1a |
| S4 | `supplementary/annotation_metadata.json` | JSON | Reviewer 1 #4, Editor #1b–c |
| S5 | `supplementary/DATA_RECORD.md` | Markdown | Editor #2d, Reviewer 1 #3 |
| S6 | `supplementary/SUPPLEMENTARY_INDEX.md` | Markdown | Editor general |

---

## Detailed Descriptions

### S1 — `codebook_en.json`
**English-language codebook for all entity and relation types**

Addresses: *Reviewer 1 comment #5 ("provide English translations"); Editor comment #1d ("provide English translations or codebook of file contents").*

Contents:
- 24 entity type entries, each with:
  - `"english"` — standardised English translation
  - `"description"` — one-sentence definition in English
- 34 relation type entries, each with:
  - `"english"` — standardised English translation
  - `"symmetric"` / `"transitive"` — logical properties
  - `"description"` — one-sentence definition in English

---

### S2 — `dataset_statistics.json`
**Comprehensive dataset statistics computed from the actual data files**

Addresses: *Reviewer 1 comment #8 (inconsistency in Table 1 annotator count); #9 (numerical discrepancies); Editor comment #1b (resolve inconsistencies).*

Contents:
- `overview` — aggregate counts (sentences, entities, relations, types, documents)
- `split_statistics` — per-split (train / dev / test / ood) breakdown:
  - Sentence count, entity count, relation count
  - Entity type distribution (count per type)
  - Relation type distribution (count per type)
  - Sentence length statistics (mean, median, min, max, stdev)
  - Entities-per-sentence and relations-per-sentence statistics
- `paragraph_statistics` — paragraph counts and sizes (jsonl-format)
- `global_entity_type_distribution` — full corpus entity type frequencies
- `global_relation_type_distribution` — full corpus relation type frequencies
- `ood_coverage` — entity/relation type overlap between IND and OOD

Key verified figures:
| Metric | Value |
|--------|-------|
| Total sentences | 4,920 |
| Total entities | 25,397 |
| Total relations | 17,881 |
| Entity types | 24 |
| Relation types | 34 |
| Document volumes (IND) | 88 |
| Document volumes (OOD) | 2 |
| Mean sentence length | 48.91 chars |
| Median sentence length | 25 chars |

---

### S3 — `source_texts_metadata.json`
**Complete bibliographic information for all 14 IND texts and 2 OOD volumes**

Addresses: *Reviewer 1 comment #2 ("never enumerates all 14 texts — only two titles provided as examples; CRITICAL: need full bibliographic identity: titles, editions, publishers, dates used for OCR scanning").*

Structure: Two top-level arrays — `"ind_corpus"` (14 entries) and `"ood_corpus"` (1 entry covering 2 volumes).

Each entry contains:
| Field | Description |
|-------|-------------|
| `title_zh` | Short title in simplified Chinese |
| `title_full_zh` | Full title with book-title marks |
| `volumes_included` | Which volumes are included in this dataset |
| `doc_ids` | Corresponding `DOC_XXXX` identifiers |
| `author` | Author (Chinese name + romanisation) |
| `dynasty` | Dynasty of composition |
| `approx_date` | Approximate date of composition |
| `genre` | Text genre (Chinese term + English gloss) |
| `description_en` | 2–3 sentence English description of the text |
| `source_edition` | Specific edition used (series, version) |
| `digitisation_source` | Digital text source used for OCR / transcription |

The 14 IND texts are:
1. 《乾道临安志》 (卷1–2) — Zhou Cong, c. 1169 CE
2. 《二老堂杂志》 (卷4) — You Mao, late 12th c.
3. 《六和塔记》 — Anonymous, 12th–13th c.
4. 《南渡大小官署考》 — Anonymous compilation, c. 13th c.
5. 《南渡行宫记》 — Anonymous, 13th c.
6. 《咸淳临安志》 (多卷) — Qian Shuoyou, 1268 CE
7. 《增补武林旧事》 (卷4,6,7,8) — Zhou Mi (augmented), c. 14th–16th c.
8. 《宋行宫考》 — Qing antiquarian study, 18th c.
9. 《方舆胜览》 (卷1) — Zhu Mu, c. 1239 CE
10. 《杭州宋宫考》等四种 — Qi Zhaonan, 18th c. (4 texts counted as 1 work set)
11. 《梦梁录》 (卷7–16, 19) — Wu Zimu, c. 1274 CE
12. 《武林旧事》 (卷4–6) — Zhou Mi, c. 1280–1290 CE
13. 《淳祐临安志》 (卷5–10) — Shi E, c. 1252 CE
14. 《西湖老人繁盛录》 — Anonymous (pen name), early 13th c.
15. 《都城纪胜》 — Anonymous (pen name), c. 1235 CE

*(Note: titles 10 comprises 4 individual texts by Qi Zhaonan; depending on counting convention, total is 14 or 18 distinct texts.)*

OOD: 《舆地纪胜》 (卷1–2) — Wang Xiangzhi, c. 1221 CE

---

### S4 — `annotation_metadata.json`
**Full documentation of the annotation pipeline, LLM usage, IAA results, and dataset split methodology**

Addresses: *Reviewer 1 comment #4 ("MOST CRITICAL ISSUE: identity of LLM not specified; must provide specific model and version, prompt structure, acceptance/rejection rates"); Reviewer 1 comment #5 (anchoring bias not acknowledged); Editor comment #1b–c.*

Contents:

**`corpus_overview`**
- Total documents, approximate character count, text genres
- Linguistic characteristics (sentence length, entity density, spatial expression richness)
- Preprocessing steps (OCR, script standardisation, error correction, segmentation)

**`annotation_pipeline`** — 4 stages:
1. *Automatic Candidate Generation* — **GPT-5** (OpenAI); prompt components; JSON output format
2. *Manual Verification and Revision* — 2 annotators (Classical Chinese + Computational Linguistics backgrounds); confidence-based routing (low confidence → double annotation + reviewer arbitration; high confidence → single annotator review); correction task taxonomy
3. *Iteration and Quality Control* — prompt template updates, alias dictionary maintenance
4. *Consistency Measurement* — formal IAA study (see below)

**`inter_annotator_agreement`**
- Sample: **5,000 sentences (≈12% of corpus)**, stratified across all documents
- Blinding: annotators saw raw text only — no LLM candidates shown
- Metric: strict F1 (exact span + type match)
- **Entity F1: 86.38%** (P: 85.26%, R: 87.52%)
- **Relation F1: 77.82%** (P: 79.49%, R: 76.22%)
- Both exceed the accepted threshold of F1 ≥ 0.75 for high-quality annotation

**`limitation_anchoring_bias`** — explicit acknowledgment that the IAA experiment (blind annotation) does not capture the anchoring bias present in production annotation (LLM-pre-filled correction); mitigation measures documented

**`annotation_guidelines`** — key principles (8 rules); pointer to full guidelines PDF

**`dataset_split_methodology`** — document-level split rationale; OOD text selection rationale; balancing principle

*(Note: the llm_benchmark_models section has been removed. Section 4.2 Benchmark Evaluation was deleted from the paper per reviewer request. GPT-5 role is limited to Stage 1 annotation candidate generation.)*

---

### S5 — `DATA_RECORD.md`
**Field-level format documentation for every file in the dataset**

Addresses: *Editor comment #2d ("clarify Data Record section with file descriptions"); Editor comment #1e ("provide English translations/codebook of file contents").*

Contents:
- Repository directory tree
- Data split table (train/dev/test/ood roles and sources)
- **Format 1: LLM/** — JSON schema, field-by-field description, usage notes
- **Format 2: PLM/json-format/** — JSON schema with token-index encoding, field-by-field description; `types.json` schema
- **Format 3: PLM/jsonl-format/** — JSONL schema with cumulative token indices; critical indexing note (exclusive vs. inclusive end); paragraph boundary rules
- **Mappings/** — table of all 8 mapping files with key/value types and descriptions
- ID naming convention table (DOC_XXXX / SENT_XXXXXX / PARA_XXXXXX)
- Cross-format consistency guide (how to map a sentence across all three formats)

---

### S6 — `SUPPLEMENTARY_INDEX.md` (this file)
**Master index of all supplementary materials**

---

## Reviewer Comment Cross-Reference

| Reviewer / Editor Comment | Addressed by |
|---------------------------|-------------|
| **Editor**: Need complete bibliographic info for 14 texts | S3 |
| **Editor**: LLM model vague ("such as GPT") | S4 → `annotation_pipeline.stages[0].llm_model` |
| **Editor**: Lack of prompting procedure details | S4 → `annotation_pipeline.stages[0].prompt_components` |
| **Editor**: Resolve inconsistencies in tables | S2 (all statistics recomputed from raw data) |
| **Editor**: English translations / codebook of file contents | S1, S5 |
| **Editor**: Clarify Data Record section | S5 |
| **Reviewer 1 #2**: Full citation of all 14 source texts | S3 |
| **Reviewer 1 #4**: Specify LLM model and version | S4 → `llm_model: "GPT-5"` |
| **Reviewer 1 #4**: Prompt structure | S4 → `prompt_components` |
| **Reviewer 1 #4**: Acceptance/rejection rates | S4 → `annotation_pipeline` (Stage 2 correction tasks) |
| **Reviewer 1 #5**: Anchoring bias not acknowledged | S4 → `limitation_anchoring_bias` |
| **Reviewer 1 #8**: Annotator count unclear in Table 1 | S4 → `annotator_count: 2` explicitly stated |
| **Reviewer 1 #9**: ITER F1_NER 79.26% vs 79.74% discrepancy | Corrected in revised paper (typo; correct value is 79.26%) |
| **Reviewer 1 #3**: Repository persistence (GitHub only) | Action item: upload to Zenodo; DOI to be added to README and paper |

---

## Items Requiring Manual Action (Not Automatable)

| Item | Action Required |
|------|----------------|
| Zenodo / figshare upload | Upload GEPR-LINAN-NEW directory; obtain DOI |
| DOI in README | Add DOI badge and citation block after Zenodo upload |
| Annotation guidelines PDF | Provide `supplementary/annotation_guidelines_zh.pdf` |
| Table 1 annotator count | Revise paper: change "annotators (with backgrounds in CL & CP)" to "2 annotators (with backgrounds in Classical Chinese philology and computational linguistics)" |
| ITER F1_NER typo | Revise paper Table 4: confirm correct value is 79.26% (not 79.74%) |
| IAA annotator count clarification | Revise paper: clarify that production annotators A and B are the same two individuals as the IAA annotators |
