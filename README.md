# Step 7 Hard-Coded Logic – Integration Guide

> **Scope**
> This README describes how to integrate the hard-coded **Step 7** module with the existing
> `step1_to_6.py` Streamlit app in your Codex-linked GitHub repository.
> All identifiers, column names, and Korean UI strings **must remain exactly as written**
> to ensure a 1-to-1 mapping with `step7_data.xlsx`.
> The Excel file is used only to generate the code—Step 7 itself relies solely on
> hardcoded `if` statements and does not load the spreadsheet at runtime.

---

## 1. Repository Structure

    / (project root)
    ├─ step1_to_6.py        # completed Steps 1 – 6
    ├─ step1_to_7_final.py  # ★ add this file
    ├─ step7_data.xlsx      # worksheet containing evaluation rules & texts
    └─ README.md            # (this file)

---

## 2. Activation Sequence

1. User finishes Step 6 → `st.session_state.step = 7`
2. `step1_to_6.py` imports `step1_to_7_final.py` **after** its own logic
3. Step 7 uses hardcoded `if` statements (converted from `step7_data.xlsx`) to render pages and store results
4. Final-page button **"신청양식 확인하기"** sets `st.session_state.step = 8`, handing control to Step 8

---

## 3. Required Objects

| Name | Type | Origin | Purpose |
|------|------|--------|---------|
| `step6_targets`      | list[str] | Step 6 | ordered `title_key` list (user choices) |
| `step6_selections`   | dict      | Step 6 | key → `"변경 있음" / "충족" / "미충족"` |
| `step6_items`        | dict      | Step 6 | `title_key` → `{ "title": str }` |
| `step7_page`         | int       | Step 7 | current page index (0-based) |
| `step7_results`      | dict      | Step 7 | `{ title_key: [(output_1_tag, output_1_text, output_2_text), …] }` |

---

## 4. Per-Page UI Flow

| Element | Data / Fixed Text |
|---------|-------------------|
| Header   | `## 제조방법 변경에 따른 필요서류 및 보고유형` |
| Subtitle | `step6_items[current_key]["title"]` |
| Row output | every row where `output_condition_all_met` evaluates **True** → show `output_1_text` + `output_2_text` (HTML) |
| Warning   | shown **only** if no rows hit → `"해당 변경사항에 대한 … 확인됩니다"` |
| Buttons   | `"이전단계로"` (−1) · `"다음단계로"` (+1) · `"신청양식 확인하기"` (+1, final page only) |

---

## 5. Data-Integrity Rules

* **Do NOT** invent new column names, session keys, or button labels  
* Keep every Korean string verbatim – no translation or trimming  
* Preserve line-breaks, bullets, and HTML tags inside Excel cells

---

## 6. Step 8 Dependency

`step7_results` is the **sole** data source consumed by Step 8 to build the final
summary and pre-fill the submission form.  
Do not alter its structure.

---

## 7. Quick Setup Checklist

- [ ] Copy `step1_to_7_final.py` to the repo root (same level as `step1_to_6.py`)
- [ ] In `step1_to_6.py` add the line
      `import step1_to_7_final` **after** the Step 6 logic
- [ ] Commit & push → Codex auto-runs Step 7 whenever `st.session_state.step == 7`

---
