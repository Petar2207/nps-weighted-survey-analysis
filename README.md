# 📊 Survey Data Analysis — NPS Pivot and Weighted Summary

This repository contains a Jupyter Notebook that processes customer survey responses to generate **Net Promoter Score (NPS)**-based analysis across multiple questions. It creates pivot tables, calculates group-specific metrics, and applies weighting logic to produce normalized summary outputs.

---

## 📁 Input Data Structure

The input is a CSV file with one row per question response. Example structure:

| Column Name         | Description |
|---------------------|-------------|
| `result_id`         | Unique identifier for each respondent |
| `result_code`       | Additional respondent code |
| `combination_type`  | Response classification group (e.g. Promoter/Passive/Detractor) |
| `customer_name`     | Name of the company being rated |
| `question_id`       | Unique ID for each question |
| `question_text`     | Full text of the question |
| `end_date`          | When the survey was completed |
| `survey_id`         | ID of the survey campaign |
| `survey_year`       | Year of the survey |
| `answer_value`      | Numeric answer given (e.g. 1–5, 6, 12) |
| `combination_value` | Label like `Promoter`, `Detractor`, or `Top2`, `Middle` |
| `usergroup`         | Subgroup of the customer (e.g. `Privatkunde / B2C`) |

---

## 🛠️ What This Notebook Does

### 1. **Data Upload and Cleaning**
- Uploads a CSV file using Google Colab.
- Filters responses to keep only entries with **question 883**.
- Excludes:
  - Responses where `question_id == 883` and `answer_value == 12`
  - Responses where `answer_value == 6` for other questions

### 2. **Pivot Table Creation**
For each of the following question pairs:
- **Main question:** `883` — Net Promoter question
- **Compared to:** `668`, `682`, `827`, `647`

It creates a merged pivot table on:
- `result_id`
- `customer_name`
- `usergroup`

Columns include:
- `answer_value` for each question
- `combination_value` (e.g. Promoter, Top2, etc.)

### 3. **Summary Calculation**
For each combination and customer (and by usergroup if customer is `"xy"`), the following metrics are calculated:

| Metric                | Description |
|-----------------------|-------------|
| `Total Responses`     | Number of responses in this group |
| `Promoters`           | Count where `combination_q883 == 'Promoter'` |
| `Detractors`          | Count where `combination_q883 == 'Detractor'` |
| `NPS Score`           | `(Promoters - Detractors) / Total * 100` |
| `Weight_Base`         | Average total responses across all customers |
| `Weight_Company`      | Normalization factor: `Weight_Base / Total Responses for company` |
| `Weighted_Promoters`  | `Promoters * Weight_Company` |
| `Weighted_Detractors` | `Detractors * Weight_Company` |

This is done for each of:
- Each answer value (1–5)
- An `"Insgesamt"` (overall) row per customer or user group

---

## 📤 Output

The notebook exports **four Excel files** — one per question pair — with fully processed summaries:

- `summary_df_668.xlsx`
- `summary_df_682.xlsx`
- `summary_df_827.xlsx`
- `summary_df_647.xlsx`

Each file includes:
- Customer-level breakdowns
- Optional usergroup-level analysis
- NPS metrics (raw and weighted)

---

## ▶️ How to Use

1. Open the notebook in [Google Colab](https://colab.research.google.com/).
2. Upload your `sva.csv` file when prompted.
3. Run the entire notebook.
4. Download the final summary files from the provided links.

---

## 🔧 Requirements

- Python 3.x
- `pandas`
- `scikit-learn`
- Google Colab (recommended for file upload/download)

---

## 📜 License

This project is licensed under the **MIT License** — free to use and modify.

---

## 👤 Author

[Your Name Here]
