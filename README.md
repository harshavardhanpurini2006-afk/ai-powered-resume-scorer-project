# 🎯 AI-Powered Resume Reviewer and ATS Scorer

A modular, privacy-first Python toolkit built with **NumPy**, **pandas**, **Matplotlib**, and **Seaborn** to compare resumes against job descriptions, calculate transparent ATS compatibility scores, identify skill gaps, provide constructive feedback, and render high-resolution visual analytics charts.

---

## 📌 Features

- **Multi-Format Extraction**: Extracts raw text seamlessly from `.txt`, `.pdf`, and `.md` files using `pypdf` with `PyMuPDF` fallback.
- **NLP & Tech-Aware Preprocessing**: Preserves punctuation-heavy technical terms (e.g., `C++`, `CI/CD`, `React.js`, `.NET`, `REST APIs`, `Scikit-Learn`) while stripping formatting artifacts and stop words.
- **4-Dimensional Scoring Engine (0–100 Scale)**:
  1. **Skill Match Coverage (35%)**: Evaluates match ratio against a curated ontology of 70+ technical and soft skills across domains.
  2. **Keyword & Domain Relevance (25%)**: Computes TF-IDF vector representations and NumPy cosine similarity between candidate and job descriptions.
  3. **Experience & Impact Evidence (20%)**: Detects action verbs, quantifiable metrics (`%`, `$`, scale multipliers, latency metrics), and high-impact accomplishments.
  4. **Readability & ATS Structure (20%)**: Analyzes standard section headers (`Summary`, `Experience`, `Education`, `Skills`, etc.), word counts, sentence lengths, and Flesch Reading Ease.
- **Actionable Feedback & Gap Analysis**:
  - Highlights missing skills and target keywords found in the job description.
  - Tailored before/after bullet point rewrites following the `[Action Verb] + [Scope] + [Quantified Result]` framework.
- **Optional Friendly Roaster Mode (`--roast`)**:
  - Adds lighthearted, constructive humor to highlight common resume pitfalls without being insulting or discriminatory.
- **Matplotlib & Seaborn Visualizations**:
  - `score_breakdown.png`: Dual-panel Polar Radar chart & styled horizontal dimension score bars.
  - `skill_match_chart.png`: Stacked domain bar chart showing matched vs. missing skills by category.
  - `keyword_distribution.png`: Keyword frequency comparison between resume and job description.
- **Strict Fairness & Privacy Guarantee**:
  - **Zero demographic bias**: Protected attributes (age, gender, ethnicity, religion, marital status, nationality, disability) are never scored.
  - **100% Local Execution**: All computations occur locally on your machine. No resume text is ever sent to external cloud APIs or 3rd-party services.
  - **Optional PII Anonymizer (`--anonymize`)**: Redacts emails, phone numbers, and URLs prior to processing.

---

## 📁 Project Structure

```
project3/
├── README.md                      # Comprehensive documentation & ethical guidelines
├── requirements.txt               # Dependencies (NumPy, pandas, Matplotlib, Seaborn, pypdf)
├── setup.py                       # Package installation and entrypoints
├── main.py                        # CLI entrypoint executable
├── ats_reviewer/
│   ├── __init__.py                # Package exports
│   ├── cli.py                     # Command-line interface with argparse
│   ├── config.py                  # Default scoring weights, stop words, lexicons, disclaimers
│   ├── extractor/
│   │   ├── __init__.py
│   │   └── text_extractor.py      # TXT & PDF document text parser
│   ├── pipeline/
│   │   ├── __init__.py
│   │   ├── cleaner.py             # Normalizer, tokenizer, n-gram generator, tech term protector
│   │   ├── taxonomy.py            # Structured skill ontology & pandas DataFrame generator
│   │   └── matcher.py             # Skill extractor, TF-IDF vectorizer, NumPy cosine similarity
│   ├── scoring/
│   │   ├── __init__.py
│   │   ├── readability.py         # Section detector, syllable counter, Flesch Reading Ease
│   │   ├── metrics.py             # Action verb detector, quantifiable metric analyzer
│   │   └── scorer.py              # Multi-dimensional ATS compatibility scoring engine
│   ├── feedback/
│   │   ├── __init__.py
│   │   ├── generator.py           # Constructive recommendations & before/after bullet rewrites
│   │   └── roaster.py             # Lighthearted, friendly roaster commentary
│   ├── privacy/
│   │   ├── __init__.py
│   │   └── anonymizer.py          # PII redactor & demographic fairness validator
│   ├── visualizer/
│   │   ├── __init__.py
│   │   └── charts.py              # Matplotlib & Seaborn radar charts, bar plots, heatmaps
│   └── reports/
│       ├── __init__.py
│       └── exporter.py            # Markdown, JSON, and Text report generator
├── samples/
│   ├── resumes/
│   │   ├── data_scientist_resume.txt
│   │   ├── data_scientist_resume.pdf
│   │   ├── software_engineer_resume.txt
│   │   └── product_manager_resume.txt
│   └── jobs/
│       ├── data_scientist_jd.txt
│       ├── software_engineer_jd.txt
│       └── product_manager_jd.txt
└── tests/
    ├── __init__.py
    ├── test_extractor.py          # Tests for TXT and PDF text extraction
    ├── test_cleaner.py            # Tests for text cleaning and n-gram extraction
    ├── test_matcher.py            # Tests for skill extraction & TF-IDF similarity
    ├── test_scorer.py             # Tests for 4-dimension scoring & readability
    ├── test_feedback.py          # Tests for feedback generator and roast mode
    ├── test_fairness_privacy.py   # Tests for PII anonymizer and zero-bias validation
    └── test_cli.py                # Tests for end-to-end CLI workflows
```

---

## 🚀 Installation

### 1. Prerequisites
- Python **3.9+** (Tested on Python 3.11, 3.12, 3.13)
- `pip` package manager

### 2. Install Dependencies
```bash
pip install -r requirements.txt
```

*(Optional) Install editable package:*
```bash
pip install -e .
```

---

## 💻 Usage

### 1. Basic Evaluation
Compare a resume with a target job description and generate all reports + visual charts in `./output`:
```bash
python main.py --resume samples/resumes/data_scientist_resume.pdf --job-desc samples/jobs/data_scientist_jd.txt
```

### 2. Enable Friendly Roaster Mode
Add lighthearted, constructive humor to the feedback:
```bash
python main.py -r samples/resumes/software_engineer_resume.txt -j samples/jobs/software_engineer_jd.txt --roast
```

### 3. Redact Personally Identifiable Information (PII)
Anonymize candidate contact details (emails, phone numbers, URLs) before analysis:
```bash
python main.py -r samples/resumes/product_manager_resume.txt -j samples/jobs/product_manager_jd.txt --anonymize
```

### 4. Custom Output Directory and Format
```bash
python main.py -r my_resume.pdf -j target_job.txt -o ./my_results --format markdown
```

### 5. CLI Arguments Reference

| Flag | Description | Default |
| :--- | :--- | :--- |
| `-r, --resume` | Path to resume file (`.txt`, `.pdf`, `.md`) | **Required** |
| `-j, --job-desc` | Path to job description file (`.txt`, `.pdf`, `.md`) | **Required** |
| `-o, --output-dir` | Directory where reports and charts are saved | `./output` |
| `--format` | Output report format (`all`, `markdown`, `json`, `text`) | `all` |
| `--roast` | Enable lighthearted, friendly commentary | `False` |
| `--anonymize` | Redact emails, phone numbers, and URLs | `False` |
| `--no-charts` | Disable Matplotlib/Seaborn image generation | `False` |
| `--weight-skills` | Custom weight for Skill Match Coverage | `0.35` |
| `--weight-keywords` | Custom weight for Keyword Relevance | `0.25` |
| `--weight-evidence` | Custom weight for Experience Evidence | `0.20` |
| `--weight-readability` | Custom weight for Readability & Formatting | `0.20` |

---

## 🐍 Python API Usage

You can also use ATS Scorer programmatically in your own Python applications:

```python
from ats_reviewer import extract_text, ATSScorer, FeedbackGenerator, Visualizer
from pathlib import Path

# 1. Extract documents
resume_text = extract_text("samples/resumes/data_scientist_resume.pdf")
jd_text = extract_text("samples/jobs/data_scientist_jd.txt")

# 2. Score compatibility
scorer = ATSScorer()
score = scorer.score(resume_text, jd_text)

print(f"Overall Score: {score.overall_score}/100 [{score.tier}]")
print(f"Matched Skills: {score.match_result.matched_skills}")
print(f"Missing Skills: {score.match_result.missing_skills}")

# 3. Generate feedback
feedback_gen = FeedbackGenerator()
feedback = feedback_gen.generate(score)
for advice in feedback.skill_gap_advice:
    print(f"- {advice}")

# 4. Generate visualizations
visualizer = Visualizer()
visualizer.generate_all_charts(score, Path("./output"))
```

---

## 🔬 Scoring Methodology & Math

The total ATS compatibility score is a calibrated weighted sum:

$$\text{Overall Score} = w_{\text{skill}} S_{\text{skill}} + w_{\text{kw}} S_{\text{kw}} + w_{\text{exp}} S_{\text{exp}} + w_{\text{read}} S_{\text{read}}$$

| Dimension | Default Weight | Mathematical / Heuristic Method |
| :--- | :---: | :--- |
| **Skill Match Coverage** ($S_{\text{skill}}$) | **35%** | $\frac{\sum_{s \in \text{Matched}} W_s}{\sum_{s \in \text{JD Skills}} W_s} \times 100$ where $W_s$ is domain importance weight. |
| **Keyword Relevance** ($S_{\text{kw}}$) | **25%** | $0.60 \times (\text{TF-IDF Cosine Similarity} \times 125) + 0.40 \times (\text{Vocabulary Overlap} \times 130)$, clipped to $[0, 100]$. |
| **Experience Evidence** ($S_{\text{exp}}$) | **20%** | $0.40 \times S_{\text{verbs}} + 0.40 \times S_{\text{metrics}} + 0.20 \times S_{\text{impact\_density}}$. |
| **Readability & ATS Structure** ($S_{\text{read}}$) | **20%** | $0.40 \times S_{\text{sections}} + 0.25 \times \text{FRE} + 0.20 \times S_{\text{length}} + 0.15 \times S_{\text{bullets}}$. |

### Compatibility Tiers

- **85.0 – 100.0**: `Excellent Match` (High alignment with primary qualifications, strong metrics, clean structure).
- **70.0 – 84.9**: `Strong Contender` (Solid alignment; minor gaps in specialized keywords or quantified results).
- **50.0 – 69.9**: `Moderate Alignment` (Foundational qualifications met; missing several role-specific competencies).
- **0.0 – 49.9**: `Low Alignment` (Significant skill or domain mismatch; substantial resume customization recommended).

---

## ⚠️ Limitations & Ethical Guidance

> [!IMPORTANT]
> **Scoring Disclaimer**: This score is a heuristic estimate designed to guide resume clarity and structural optimization. It is **NOT** an official applicant tracking system (ATS) decision or hiring guarantee. Human hiring managers evaluate real-world problem-solving abilities, communication, portfolio depth, and cultural alignment far beyond keyword counts.

> [!TIP]
> **Anti-Keyword-Stuffing Rule**: **Never fabricate skills or work history.** ATS matching is designed to help you surface genuine relevant accomplishments. Adding buzzwords for tools you haven't used leads to negative interview outcomes and damages professional credibility.

---

## 🔒 Privacy & Fairness Guarantee

1. **Zero Protected Characteristic Bias**: The analyzer actively strips demographic attributes (age, gender, ethnicity, marital status, religion, nationality, disability) to prevent discriminatory scoring.
2. **100% Offline & Local**: No resumes or job descriptions are transmitted across external networks or third-party cloud APIs.
3. **Built-in Anonymization**: The `--anonymize` flag ensures that personal contact data can be redacted on the fly.

---

## 🧪 Running Automated Tests

Run the complete test suite with `pytest`:
```bash
pytest -v
```

All 29 tests cover text extraction (TXT, PDF), regex tokenization, taxonomy mapping, TF-IDF cosine similarity, readability formulas, metric detection, feedback generation, fairness safeguards, and end-to-end CLI execution.
