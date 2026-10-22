# Open-Source Security Briefing — October 2026

**Prepared for:** HeziLiu's team
**Date:** October 2026
**Status:** Ready for maintainer review

---

## 1. Starred Repositories Review

The authenticated account (HeziLiu) currently has **6 starred repositories**:

| # | Repository | Language | Stars |
|---|------------|----------|-------|
| 1 | OpenBMB/AgentCPM | Python | 833 |
| 2 | PaddlePaddle/ERNIE | Python | 7,739 |
| 3 | FudanNLP/fastNLP | Python | 3,138 |
| 4 | maplezzz/NTU_ML2017_Hung-yi-Lee_HW | Jupyter Notebook | 874 |
| 5 | fighting41love/funNLP | Python | 84,084 |
| 6 | 996icu/996.ICU | — | 277,153 |

All starred repositories are still active or historically relevant; none are archived.

## 2. Latest Release Tag Tracked

Of the starred repositories, **PaddlePaddle/ERNIE** has release tags. Its most recent tag:

- **Tag name:** `ernie-4.5`
- **Commit SHA:** `c4d32d4d351018fb2abbf693d9467013028d39da`
- **Commit message:** "Update blog link" (authored by aigcliu, dated 2025-06-30)
- **Note:** ERNIE's default branch is `release/v1.5`.

## 3. Critical Security Advisories

### 3.1 npm — Storybook (CRITICAL)

- **GHSA ID:** GHSA-f2ph-cmg5-wxc8
- **CVE ID:** CVE-2026-114125
- **Affected package:** `storybook` (npm)
- **Vulnerable version ranges:**
  - `>= 8.0.0, < 8.6.19` → patched in **8.6.19**
  - `>= 9.0.0, < 9.1.21` → patched in **9.1.21**
  - `>= 10.0.0, < 10.6.2` → patched in **10.6.2**
  - `>= 11.0.0-alpha` → patched in **11.0.0-alpha.6**
- **Primary patched version (first range):** **8.6.19**
- **Summary:** Missing origin validation in the Storybook preview allows code injection from malicious websites. A malicious website may cause unauthorized file changes in a developer's project while an affected Storybook dev server is running, potentially leading to arbitrary code execution with developer privileges.
- **CWEs:** CWE-22, CWE-94, CWE-96, CWE-346, CWE-441, CWE-749, CWE-862, CWE-1021
- **Recommendation:** Upgrade before running `storybook dev` again. Note behavior changes: network iframe embedding is disabled by default, preview iframes now reject messages from unknown hosts, and the `highlight` event signature has changed.
- **Reference:** https://github.com/advisories/GHSA-f2ph-cmg5-wxc8

### 3.2 pip — Pillow (CRITICAL)

- **GHSA ID:** GHSA-332p-7hrc-92wf
- **Affected package:** `Pillow` (pip)
- **Summary:** Heap buffer overflow in `_tiffWriteProc` when saving TIFF to `BytesIO`. Pillow can write attacker-chosen data past the end of a heap buffer when saving a TIFF image to an in-memory destination, and also leaks stale heap memory into saved files. Reachable with default settings via `Image.open(upload); im.save(io.BytesIO(), "TIFF")`.
- **CWE:** CWE-122 (Heap-based Buffer Overflow)
- **Defect present since:** Pillow 2.0.0 (2013)

## 4. GitHub Actions Workflow Health

Reviewed all repositories under the authenticated account (HeziLiu): **Little-rookie**, **Big-Buzzard**, **CodingInterviewChinese2**, and **awesome-chatgpt-prompts-zh**.

- **Result:** No GitHub Actions workflows exist in any of these repositories — therefore **no failed workflow runs** and nothing to re-run.
- **Action:** No remediation required this month.

## 5. Action Items

1. Audit all internal projects for `storybook` (npm) usage and upgrade to at least the patched version for the relevant major line (8.6.19 / 9.1.21 / 10.6.2 / 11.0.0-alpha.6) — GHSA-f2ph-cmg5-wxc8, CVE-2026-114125.
2. Audit Python services that re-save uploaded images to TIFF via in-memory sinks (e.g., `io.BytesIO`) for the Pillow heap overflow — GHSA-332p-7hrc-92wf.
3. Review new Storybook behavior changes (iframe embedding, `postMessage` origin checks, `highlight` event API) before upgrading.
4. Confirm no CI/CD regressions — no failing GitHub Actions workflow runs were found.
