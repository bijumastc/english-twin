# English Comprehension Twin

Adaptive reading-comprehension practice for B.Com first-year students in Kerala.

## How it works for students

Students practise with **Twin** 🦊, a friendly language twin that welcomes them back, tracks their strengths and cheers them on.

1. **Reading check (6 passages):** 60 → 80 → 100 → 120 → 150 → 200 words.
   - Passages 1–2: general, commerce, business, management situations.
   - Passages 3–6: alternate general and business-analytics themes.
2. **Unlimited practice afterwards:** passages keep coming at the student's level, built around their weakest skill. 80%+ moves up a level; below 50% moves down one.
3. **Higher-order thinking questions only** — nothing can be answered by copying one sentence:
   - Q1 Analysis & inference (MCQ): why, what follows, what is likely next.
   - Q2 Vocabulary in context (MCQ): work out meaning from clues.
   - Q3 Critical thinking (written): judge, justify, suggest or predict.
   Each has an optional 💡 hint, and instant feedback with smileys.
4. **Scaffolds:** an English glossary (meaning + example sentence), a language note based on the student's own recent mistakes, a grammar table, vocabulary tips and a polished version of their answer.
5. **Full review after every passage:** a personal message from Twin, what went well, what to work on, a think-deeper tip and a goal for next time.
6. **Quit any time:** "Save & quit" keeps everything. On return, Twin offers to continue an unfinished passage. "Try this passage again" lets them redo one.

## Teacher dashboard and Sheet

The Google Sheet gets these tabs automatically:

| Tab | What it holds |
|---|---|
| **Summary** | One row per student: times entered, passages completed, questions answered, average and last score, practice minutes, last visit. Updates live by formulas. |
| Logins | Every login and quit, with passages completed in that visit. |
| Passages | One row per completed passage with all skill scores. |
| Attempts | Every answer with its score and feedback. |
| Drafts | Unfinished passages, used for resume. |

The in-app dashboard shows the same, plus charts, each student's answers, and CSV downloads.

## Setup

### 1. Gemini API key
Create a key at https://aistudio.google.com/apikey. The default model is `gemini-3.5-flash-lite`; change `GEMINI_MODEL` in secrets if needed. The app falls back to other models automatically if one is unavailable.

### 2. Google Sheet (permanent storage)
1. In Google Cloud Console, create a project, enable the **Google Sheets API** and **Google Drive API**.
2. Create a **service account** → Keys → Add key → JSON. Download it.
3. Create an empty Google Sheet. Share it with the service account's `client_email` as **Editor**.
4. Copy the Sheet ID from its URL: `docs.google.com/spreadsheets/d/<THIS_PART>/edit`.

The app creates two tabs itself: **Attempts** (one row per question) and **Passages** (one row per completed passage).

Without these secrets the app saves to local CSV files in `data/`, which is fine for testing but is wiped whenever Streamlit Cloud restarts.

### 3. Run locally
```bash
pip install -r requirements.txt
cp .streamlit/secrets.toml.example .streamlit/secrets.toml   # then fill it in
streamlit run app.py
```

### 4. Deploy (private GitHub repo + Streamlit Community Cloud)
1. Push this folder to a **private** GitHub repo. `.gitignore` already keeps `secrets.toml` and JSON keys out.
2. At https://share.streamlit.io, create an app from the repo with main file `app.py`.
3. In **App settings → Secrets**, paste the full contents of your `secrets.toml`.

### Adding students
Edit the `[roster]` section in secrets: `"roll number" = "Full Name"`. Login is case- and spacing-insensitive on the name. The Summary tab is rebuilt with the new roster the next time the app restarts (Manage app → Reboot).
