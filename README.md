# GUI_Lab
# 📄 HR Candidate Profile Parser

Turn unstructured CVs into clean, structured JSON using **LangChain**, **Groq-hosted LLMs**, and a **Streamlit** web interface.

Upload a resume (PDF, DOCX, or TXT) or paste its text, and get back a structured candidate profile with name, email, education, skills, and work experience, ready to download or plug into an HR pipeline.

---

## ✨ Features

- **Structured extraction**: LangChain `StructuredOutputParser` enforces a fixed schema for every resume.
- **Self-healing output**: `OutputFixingParser` automatically asks the LLM to repair malformed JSON.
- **Multi-format input**: PDF (`pypdf`), DOCX (`python-docx`, including tables), and plain text.
- **Fast inference**: runs on Groq (`openai/gpt-oss-20b` by default, configurable in the UI).
- **Interactive GUI**: Streamlit app with tabs for upload/paste, skill chips, data tables, raw JSON view, and JSON download.
- **One-click sharing**: the notebook exposes the app through an ngrok public URL.

---

## 🧱 Output Schema

| Field | Type | Description |
|---|---|---|
| `full_name` | string | Candidate's full name |
| `email` | string | Candidate's email address |
| `education` | list of objects | `degree` (string), `institution` (string), `year` (integer) |
| `skills` | list of strings | Technical and soft skills |
| `experience` | list of objects | `role` (string), `company` (string), `years` (string, e.g. `2023-Present`) |

### Example output

```json
{
  "full_name": "BASMALA ABOELHAMED",
  "email": "basmalaaboelhamed@gmail.com",
  "education": [
    {
      "degree": "Bachelor of Computing and Information System",
      "institution": "Arab Academy for Science, Technology and Maritime Transport (AASTMT)",
      "year": 2027
    }
  ],
  "skills": ["Python", "SQL", "TensorFlow", "LangChain", "RAG", "Docker"],
  "experience": [
    {
      "role": "Intern",
      "company": "CIB (Commercial International Bank)",
      "years": "2026-Present"
    }
  ]
}
```

---

## 🏗️ How It Works

```
CV file / pasted text
        │
        ▼
 Text extraction (pypdf / python-docx / txt)
        │
        ▼
 PromptTemplate + format instructions
        │
        ▼
 ChatGroq LLM  ──►  StructuredOutputParser
                          │ (on failure)
                          ▼
                    OutputFixingParser
        │
        ▼
 Structured JSON  ──►  Streamlit UI / JSON download
```

---

## 🧰 Tech Stack

- **Python 3**
- **LangChain** (`langchain`, `langchain-core`, `langchain-classic`, `langchain-community`)
- **Groq** via `langchain-groq`
- **Streamlit** for the web UI
- **pyngrok** for public tunneling
- **pypdf**, **python-docx**, **pandas**

---

## 🚀 Getting Started

The project is a single notebook, `langchain_lab3+GUI.ipynb`, designed for **Google Colab** or **Kaggle**.

### 1. Get the required keys

- **Groq API key**: free at [console.groq.com](https://console.groq.com)
- **ngrok authtoken**: free at [ngrok.com](https://ngrok.com) (only needed to share the GUI publicly)

### 2. Open the notebook

Upload `langchain_lab3+GUI.ipynb` to Colab or Kaggle. On Kaggle, store the keys as secrets named `GROQ_API_KEY` and `NGROK_AUTHTOKEN`. Otherwise the notebook prompts you for them.

### 3. Run the cells in order

| Section | What it does |
|---|---|
| Install | Installs LangChain, Groq, and parsing dependencies |
| 1. Output Schema | Defines the `ResponseSchema` fields and the parser |
| 2. Prompt Template | Builds the extraction prompt |
| 3. Input | Reads a CV file and extracts its text |
| 4. Run LLM | Calls the model and parses the response into JSON |
| 5. Streamlit GUI + ngrok | Writes `app.py`, launches Streamlit, and prints a public URL |

Keep the last cell running while you use the app. Interrupt it to shut everything down.

---

## 🖥️ Using the Web App

1. Open the ngrok URL printed by the last cell.
2. In the sidebar, enter your Groq API key (skipped if already set on the server), choose a model, and adjust the temperature.
3. Upload a CV in the **Upload CV** tab, or paste text in the **Paste text** tab.
4. Click **Parse profile**.
5. Review the results, then download them as `candidate_profile.json`.

---

## ⚠️ Limitations

- Scanned or image-only PDFs are not supported, because text extraction requires a text layer (no OCR).
- Very long documents are truncated to the first 24,000 characters.
- Extraction quality depends on the chosen LLM. Always review results before using them in hiring decisions.

---

## 🔮 Future Improvements

- OCR support for scanned CVs
- Batch processing of multiple resumes with CSV export
- Additional fields (certifications, projects, languages)
- Candidate-to-job-description matching and scoring

---

## 👩‍💻 Author

**Basmala AboElhamed**, Computer Science student specializing in Machine Learning and Data Analytics.
- LinkedIn: [linkedin.com/in/basmala-aboelhamed-7338b42a7](https://linkedin.com/in/basmala-aboelhamed-7338b42a7)
