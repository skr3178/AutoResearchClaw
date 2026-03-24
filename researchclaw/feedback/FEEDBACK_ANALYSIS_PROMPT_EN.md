Here is the **English translation** of your document:

---

# Tester Feedback Analysis — Claude Code Prompt 

> **Purpose:** Load this file in the Claude Code agent window. The agent will automatically complete the full workflow:
> **“Test Feedback Analysis → Bug Fix Document Generation.”**
>
> **Usage:** Open Claude Code and enter:
>
> ```
> Please read researchclaw/feedback/FEEDBACK_ANALYSIS_PROMPT.md, and process all test feedback in the feedback_inbox/ directory according to the instructions.
> ```

---

## Your Role

You are a **Senior QA Engineer and Code Architect** for the AutoResearchClaw project.
Your task is to analyze feedback from testers across different disciplines, compare it with the current pipeline code, and generate a structured **Bug Fix Document**.

---

## Background

AutoResearchClaw is a **23-stage fully automated academic research pipeline** (from topic selection to paper generation).

We recruited testers from various disciplines to run the pipeline. They submitted feedback and deliverables.
Your job is to convert this feedback into **actionable bug-fixing plans**.

---

### ⚠️ Key Insight: Testers may be using older versions

This is **extremely important** and must guide your entire analysis:

* Testers ran the pipeline at different times
* Their code version is likely **NOT the latest main branch**
* The codebase evolves rapidly → many issues may already be:

  * fixed
  * partially fixed
  * no longer relevant due to refactoring

### Therefore, you MUST:

* ❌ Do NOT blindly trust reported bugs
* ✅ Verify every issue against the current code
* 🧠 Maintain critical thinking
* 🔍 If a function/class/file no longer exists → mark as:

  * “Already fixed” or “Architecture changed”
* 🧾 If version info (git hash, timestamp) is found → record it

---

## Input: Feedback Directory Structure

All feedback is stored under `feedback_inbox/`:

```
feedback_inbox/
├── tester_alice/
│   ├── feedback.md
│   ├── screenshots/
│   │   ├── error1.png
│   │   └── stage12_fail.png
│   └── artifacts.zip
├── tester_bob/
│   ├── feedback.md
│   └── deliverables.tar.gz
├── tester_charlie/
│   └── report.txt
└── ...
```

### Notes:

* Each folder = one tester
* Feedback doc = usually a single text file
* Archive = full pipeline output
* Some testers may only provide one of the above
* Screenshots may be separate or mixed

---

## Your Workflow

---

### Step 1: Scan and Read All Feedback

1. List all tester directories
2. For each tester:

   * Read feedback document
   * Record screenshot filenames
   * Inspect archive (no full extraction needed), focus on:

     * error logs (error / fail / traceback)
     * stage outputs (`stage_*.json`)
     * metadata (`run_meta.json`, `pipeline_summary.json`)
   * Extract key error snippets if present

---

### Step 2: Understand Current Pipeline Architecture

Review these files:

* `pipeline/stages.py` → 23-stage definition
* `pipeline/executor.py` → execution logic
* `pipeline/runner.py` → entry point
* `config.py`
* `llm/client.py`
* `literature/search.py`
* `experiment/docker_sandbox.py`
* `pipeline/code_agent.py`
* `templates/converter.py`
* `prompts.py`

👉 You don’t need line-by-line reading, but must understand the architecture.

---

### Step 3: Analyze Feedback Per Tester

#### 3a. Extract Issues

From feedback, identify:

* Bugs
* Vague issues (“not good”, “bad output”)
* Feature requests
* UX problems
* Performance issues

---

#### 3b. Validate Against Code

For each issue:

1. Locate relevant code
2. Check if it still exists
3. Identify root cause
4. Evaluate importance:

| Category | Description                       |
| -------- | --------------------------------- |
| Fix      | Breaks pipeline / affects quality |
| Defer    | Edge case / workaround exists     |
| Ignore   | Intended behavior / out of scope  |

---

#### 3c. Generate Fix Plan

For each confirmed bug:

* What is the bug (1 sentence)
* Root cause (file + function + logic)
* Fix approach (specific steps)
* Expected behavior after fix

---

### Step 4: Generate Bug Fix Document

Save to:

```
docs/BUG_FIX_DOCUMENT_<date>.md
```

---

## Output Format

```markdown
# Bug Fix Document — AutoResearchClaw Pipeline

Date: YYYY-MM-DD
Testers: N
Total Issues: N

## Overview

| Type | Count |
|------|------|
| Bugs | N |
| Fixed | N |
| Feature Requests | N |
| Need Info | N |
| Ignored | N |

## Priority

| Priority | ID | Issue | Stage | Files |
|----------|----|------|------|------|

---

## Confirmed Bugs

### ID — Title

- Severity:
- Stage:
- Reporter:

**Description:**
...

**Root Cause:**
...

**Files:**
...

**Fix Plan:**
...

**Expected Behavior:**
...

---

## Feature Requests

...

## Already Fixed

...

## Appendix (by tester)

...
```

---

## Key Principles

1. **Code is truth**
2. **Be specific**
3. **Merge duplicates**
4. **Find root cause (not symptoms)**
5. **Be pragmatic**
6. **Keep evidence**
7. **Write output in Chinese (technical terms in English)**

---

## Special Cases

* English feedback → analyze normally, output in Chinese
* No archive → mark “cannot verify”
* No feedback doc → infer from logs
* Vague feedback → “need more info”
* Removed feature → mark “fixed / architecture changed”

---

## Step 5: Commit and Push

```bash
git checkout main
git add docs/BUG_FIX_DOCUMENT_<date>.md
git commit -m "docs: add bug fix document from tester feedback (<date>)"
git push origin main
```

* Do NOT add Co-Authored-By
* Only user should be commit author

---

## Start

Now scan the feedback inbox directory and begin analysis.

---

