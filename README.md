# CoverCraft: Cover Letter Generator using a Local LLM

**LLM Mini Project — Problem Statement No. 42**[cite: 2]  
**Author:** Keyur Patle (Roll No. 42, B.Tech CSE)[cite: 2]  
**Repository:** [https://github.com/keyurrrrr/llm-ps42-roll42](https://github.com/keyurrrrr/llm-ps42-roll42)[cite: 2]

**CoverCraft** is a locally hosted web application that generates tailored, ~250-word cover letters from a candidate's resume (PDF or plain text), a target job description, and the company name[cite: 2]. Powered by **Ollama (`qwen2.5:3b`)**[cite: 2], it runs 100% locally with zero paid APIs or external data sharing, ensuring candidate privacy while preventing LLM hallucinations through structured XML-style prompt tags and a post-generation rule-based accuracy checker[cite: 2, 3].

---

## Key Features

* **100% Local & Private Inference:** Communicates with Ollama's local REST API (`/api/tags`, `/api/chat`) using `qwen2.5:3b`[cite: 2]. No cloud APIs, paid subscriptions, or API keys are required[cite: 2].
* **Robust PDF Resume Processing:** Extracts and cleans text (normalizing ligatures and spacing) via **PyMuPDF**, automatically rejecting empty, corrupted, password-protected, or scanned (image-only) PDFs with clear error messages[cite: 2]. Extracted text is displayed in an editable box for review before generation[cite: 2].
* **Anti-Hallucination Prompt Architecture:** Separates candidate facts (`<RESUME>`) from employer requirements (`<JOB_DESCRIPTION>`) with explicit system rules forbidding fabricated skills, numbers, dates, or achievements[cite: 2].
* **Rule-Based Accuracy Checker:** Scans the generated letter against the source resume and flags any skill or number not found in the resume so the user can review and edit before exporting[cite: 2].
* **Automatic Length Calibration:** Cleans raw model outputs (removing preambles, Markdown formatting, placeholders, and post-signature notes) and performs a one-time automated rewrite if the draft deviates by more than 25% from the 250-word target[cite: 2].
* **Interactive Single-Page Studio & A4 DOCX Export:** Built with Streamlit (*Profile*, *Opportunity*, and *Letter* sections) featuring a live editable preview, word count tracker, copy-to-clipboard, regenerate, undo, and formatted A4 `.docx` export via `python-docx` that preserves the user's latest edits[cite: 2, 3].

---

## Tech Stack & Environment

* **Language:** Python 3.13[cite: 2]
* **Frontend:** Streamlit (with custom CSS styling targeting Streamlit containers)[cite: 2, 3]
* **Local LLM Engine:** Ollama (`qwen2.5:3b`, temperature: `0.4`, context window: `8192`)[cite: 2]
* **Libraries:** `requests`, `PyMuPDF` (`fitz`), `python-docx`, `python-dotenv`[cite: 2]
* **Testing:** `pytest` (45 automated unit and mocked API tests)[cite: 2]
* **Development Environment:** VS Code on Windows[cite: 2]

---

## Application Screenshots

| Home Page & Local Model Status | Resume Extraction & Job Input |
| :---: | :---: |
| ![Home Page](Screenshots_CoverCraft/screenshot_1.png) | ![Resume and Job Input](Screenshots_CoverCraft/screenshot_2.png) |

| Generated Letter & Accuracy Check | Exported A4 Word Document (`.docx`) |
| :---: | :---: |
| ![Generated Letter](Screenshots_CoverCraft/screenshot_3.png) | ![DOCX Output](Screenshots_CoverCraft/screenshot_4.png) |

---

## Project Structure

```text
llm-ps42-roll42/
├── .env.example                  # Default Ollama & generation configuration
├── .gitignore                    # Excludes venv, .env, cache, and generated files
├── README.md                     # Project documentation
├── requirements.txt              # Python dependencies
├── app.py                        # Main Streamlit application
├── prompts/
│   └── cover_letter_prompt.txt   # System prompt and tagged user prompt template
├── scripts/
│   └── run_scenarios.py          # Script to run 5 sample evaluation scenarios
├── tests/                        # 45 automated pytest unit & error-handling tests
├── docs/
│   └── Keyur_Patle_42_Report.pdf # Procedure report for Problem Statement No. 42
└── Screenshots_CoverCraft/         # UI and output verification screenshots
    ├── screenshot_1.png
    ├── screenshot_2.png
    ├── screenshot_3.png
    ├── screenshot_4.png
    └── screenshot_5.png
```

---

## Setup & Installation

### 1. Prerequisites
* **Python 3.13** installed on your system[cite: 2].
* **[Ollama](https://ollama.com/)** installed locally[cite: 2]. Pull and verify the chat model (`qwen2.5:3b`):
  ```bash
  ollama pull qwen2.5:3b
  ollama list
  ```

### 2. Clone the Repository
```bash
git clone [https://github.com/keyurrrrr/llm-ps42-roll42.git](https://github.com/keyurrrrr/llm-ps42-roll42.git)
cd llm-ps42-roll42
```

### 3. Create a Virtual Environment & Install Dependencies
```bash
python -m venv venv

# On Windows:
venv\Scripts\activate

# On macOS/Linux:
source venv/bin/activate

pip install -r requirements.txt
```

### 4. Configure Environment Variables
Copy `.env.example` to `.env` (validated automatically at startup)[cite: 2]:
```bash
cp .env.example .env
```
Default configuration values[cite: 2]:
```env
OLLAMA_HOST=http://localhost:11434
OLLAMA_MODEL=qwen2.5:3b
OLLAMA_TIMEOUT=120
OLLAMA_TEMPERATURE=0.4
OLLAMA_NUM_CTX=8192
TARGET_WORD_COUNT=250
```

### 5. Run the Application
Ensure the Ollama service is running in the background, then launch the Streamlit app:
```bash
streamlit run app.py
```

---

## Testing & Verification

* **Run the Automated Test Suite (45 Tests):**  
  Tests cover PDF parsing, prompt construction, environment validation, rule-based accuracy checking, `.docx` export, and mocked Ollama network/error responses[cite: 2]:
  ```bash
  pytest
  ```
* **Run Sample Scenarios:**  
  Executes five end-to-end sample scenarios against the local model[cite: 2]:
  ```bash
  python scripts/run_scenarios.py
  ```

---

## Sample Evaluation Result

* **Input:** 634-word resume PDF, target company **Voltas**, role **BI Intern**, and a 191-word job description (Power BI/Excel dashboards, data cleaning, Python a plus)[cite: 3].
* **Output:** A 213-word cover letter (*On target*) linking resume experience (migrating 650+ legacy Excel records to PostgreSQL and building a platform for 250+ stakeholders) directly to the role's dashboard and data-cleaning requirements[cite: 3].
* **Verification:** Passed the post-generation accuracy check with zero hallucinated skills or metrics and exported cleanly to an A4 `.docx` file[cite: 3].

---

## Limitations & Future Improvements

* **Model Size Trade-offs:** As a lightweight 3B parameter model, `qwen2.5:3b` writes plainer prose than larger cloud models and occasionally uses generic phrasing—mitigated by the built-in interactive editor and **Regenerate** option[cite: 3].
* **Scanned Resumes:** Image-only PDFs are currently rejected rather than parsed[cite: 2, 3].
* **Planned Enhancements:** Adding real-time token streaming during generation and integrating OCR support for scanned resume PDFs[cite: 3].
