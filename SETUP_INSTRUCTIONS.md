# Setup Instructions — LinkedIn Auto Job Applier

This guide walks a brand-new person through installing and running this bot on their
own Windows PC. It reflects how the tool *actually* behaves (including the manual
LinkedIn login step), not just the theory.

> ⚠️ **Read this first — privacy & safety**
> - This tool logs into **your** LinkedIn and submits **real** job applications on your behalf.
> - The config files contain personal data and passwords. **Never share your filled-in
>   `config/secrets.py` or `config/personals.py` with anyone.** Each person fills in their own.
>   (Both files are already listed in `.gitignore` so they won't be committed to git.)
> - Use it in accordance with LinkedIn's Terms of Service. You are responsible for what it submits.

---

## 1. Prerequisites (downloads needed on a fresh laptop)

| Requirement | How to get it | Notes |
|-------------|---------------|-------|
| **Python 3.10 or newer** | https://www.python.org/downloads/ (or [Anaconda](https://www.anaconda.com/download)) | During the python.org install, **check "Add Python to PATH"**. Includes `pip` and `tkinter` (used for the bot's popups). |
| **Google Chrome** | https://www.google.com/chrome | Install in the default location. |
| **The project files** | Download the ZIP from the repo and unzip it, **or** clone it with Git | The download already includes the `config/` files you'll edit in step 4. |
| **Git** (optional) | https://git-scm.com/download/win | Only needed if you want to `git clone` instead of downloading the ZIP. |

Verify Python and pip are installed — open **PowerShell** and run:

```powershell
python --version
pip --version
```

You should see `Python 3.10` (or higher) and a `pip` version. If `python` isn't
recognized, reinstall Python with **"Add Python to PATH"** checked.

> Note: The bot's confirmation popups (e.g. "Login Required") use **tkinter**, which
> ships with the standard python.org installer and with Anaconda. No separate install needed.

---

## 2. Install the Python packages

Open PowerShell **in the project folder** (the folder that contains `runAiBot.py`) and run:

```powershell
pip install -r requirements.txt
```

This installs everything the project imports:

| Package | Used for |
|---------|----------|
| `undetected-chromedriver`, `selenium` | Driving Chrome / automating LinkedIn |
| `pyautogui` (pulls in `pillow`) | On-screen popups and screenshots |
| `setuptools` | Build/runtime support |
| `flask`, `flask-cors` | The applied-jobs dashboard (`app.py`) |
| `openai` | AI features (also covers DeepSeek / OpenAI-compatible endpoints) |
| `google-generativeai` | AI features — **imported automatically whenever `use_AI = True`**, even if you don't use Gemini |
| `python-docx`, `fpdf2` | The (experimental) resume generator |

> Why `google-generativeai` is required: `runAiBot.py` imports the Gemini, OpenAI, and
> DeepSeek modules together when `use_AI = True` (the default). If you'd rather not install
> the AI packages at all, set `use_AI = False` in `config/secrets.py`.

---

## 3. Match the Chrome version (important!)

The bot pins which ChromeDriver to download via `chrome_version_main` in
`config/settings.py`. **This number must match your installed Chrome's major version**,
or the browser will fail to open.

1. Open Chrome and go to `chrome://settings/help` to read your version
   (e.g. `149.0.7827.197` → the major version is **149**).
2. Open `config/settings.py` and set:

   ```python
   chrome_version_main = 149   # use YOUR Chrome major version
   ```

   (Set it to `None` to auto-detect, but pinning the exact number is more reliable.)

> Tip: Chrome auto-updates. If you later see an error like
> "ChromeDriver only supports Chrome version X", just update this number to X.

---

## 4. Fill in your configuration (in the `config/` folder)

Three of the config files are **not included in the repo** (they hold personal data, so they're
git-ignored). Instead the repo ships `*.example` templates. **First, copy each template to its
real filename**, then edit it. In PowerShell, from the project folder:

```powershell
Copy-Item config/secrets.py.example   config/secrets.py
Copy-Item config/personals.py.example config/personals.py
Copy-Item config/questions.py.example config/questions.py
```

(In an editor you can just copy/rename the files manually.)

Then edit these files with your own information:

| File | What goes in it |
|------|-----------------|
| `config/secrets.py` *(from template)* | LinkedIn email & password (optional), and AI/OpenAI API key (optional). |
| `config/personals.py` *(from template)* | Name, phone, address, demographic / EEO answers. |
| `config/questions.py` *(from template)* | Years of experience, salary, visa, LinkedIn URL, cover letter, resume path, etc. |
| `config/search.py` | `search_terms`, `search_location`, filters (job type, experience level, remote/hybrid), companies/words to skip. |
| `config/settings.py` | Bot behavior (see key settings below). |

### Resume
Put your resume PDF where `default_resume_path` in `config/questions.py` points, e.g.:
```
all resumes/default/your-resume.pdf
```
If the file isn't found, the bot falls back to whatever resume you last uploaded to LinkedIn.

### Key safety settings in `config/settings.py`
For a **first run**, beginners may prefer a cautious setup so you can watch what it does:

| Setting | Cautious first run | Full automation |
|---------|--------------------|-----------------|
| `pause_before_submit` (in `config/questions.py`) | `True` (you approve each submit) | `False` (auto-submit) |
| `pause_at_failed_question` (in `config/questions.py`) | `True` (asks you) | `False` (answers randomly) |
| `use_resume_generator` | `False` (experimental, can break) | `True` |
| `safe_mode` | `True` (clean guest profile) | `False` (uses your real Chrome profile) |

---

## Recommended editor: VS Code (free) or Cursor

You don't strictly need a code editor — everything can be done in PowerShell — but an
editor makes editing the `config/` files and running the bot much easier.

**Which is cheaper?** **VS Code is completely free, forever**, and is all you need to run
this project. **Cursor** is built on VS Code and can also run it for free (its "Hobby"
tier); Cursor only costs money ($20/month "Pro") for its AI assistant features, which are
**not needed** to run this bot. So for just running the program, **use VS Code**.

> The steps below are identical in Cursor, since Cursor is a fork of VS Code.

1. **Install VS Code:** https://code.visualstudio.com/ (or Cursor: https://cursor.com/).
2. **Open the project folder:** `File > Open Folder…` and pick the folder that contains
   `runAiBot.py`.
3. **Install the Python extension:** open the Extensions panel (the squares icon on the
   left, or `Ctrl+Shift+X`), search **"Python"** (by Microsoft), and click Install.
4. **Select your Python interpreter:** press `Ctrl+Shift+P`, type
   **"Python: Select Interpreter"**, and choose your Python 3.10+ install.
5. **Open the built-in terminal:** menu `Terminal > New Terminal` (or `` Ctrl+` ``). It opens
   already inside the project folder, so you can run all the commands from this guide there:
   ```powershell
   pip install -r requirements.txt
   python runAiBot.py
   ```
   You can also just open `runAiBot.py` and click the **▶ Run** button in the top-right.

> Tip: keep the editor's terminal visible while the bot runs — the bot's progress messages
> show up there and in `logs/log.txt`.

---

## 5. Run the bot

Make sure Chrome isn't blocking things, then run it (in PowerShell **or** the editor's
built-in terminal):

```powershell
python runAiBot.py
```

### What to expect on the first run
1. The bot downloads a matching ChromeDriver (can take a minute on each run in stealth mode).
2. It opens a Chrome window and tries to log into LinkedIn.
3. **The automatic login usually fails on a fresh/guest profile** (LinkedIn blocks scripted
   logins with a captcha / "verify it's you"). This is normal.
4. A small **"Login Required"** popup appears. **Leave it open**, switch to the bot's Chrome
   window, and **log into LinkedIn manually** (enter credentials, complete any verification
   until you see your home feed).
5. Click **"Confirm Login"** on the popup. The bot detects the login and continues
   searching and applying automatically.

> Avoiding the manual login each time: set `safe_mode = False` in `config/settings.py`.
> Then the bot reuses your normal Chrome profile (where you stay logged into LinkedIn).
> If you do this, **fully close all Chrome windows before starting the bot.**

### View your applied-jobs history (optional dashboard)
In a second PowerShell window:
```powershell
python app.py
```
Then open `http://localhost:5000` in a browser.

Application history is also saved to CSV files in the `all excels/` folder.

---

## 6. Helping someone else install it (sharing checklist)

When you hand this off to another person:

1. Give them the project **without your personal files**. The safest way is to share the
   original repo/ZIP and have them fill in their own config. **Do not** include your
   `config/secrets.py` or `config/personals.py`, or your resume.
2. Have them follow steps 1–5 above with **their own** details.
3. Remind them to set `chrome_version_main` to **their** Chrome version (step 3).
4. If you ever shared your `config/secrets.py` by accident, **change your LinkedIn password
   and rotate (regenerate) any API key** that was in it.

---

## Troubleshooting

| Problem | Fix |
|---------|-----|
| Browser won't open / "ChromeDriver only supports Chrome version X" | Set `chrome_version_main = X` in `config/settings.py` (step 3). |
| "Couldn't find username field" / "not logged in" | Expected on guest profile — log in manually, then click "Confirm Login" (step 5). |
| Chrome opens then closes immediately | Close all other Chrome windows; or try `safe_mode = True`. |
| `pip` not recognized | Reinstall Python with "Add to PATH" checked, or use `python -m pip ...`. |
| AI/OpenAI errors | Set `use_AI = False` in `config/secrets.py` to run without AI features. |
| Package import errors | Re-run `pip install -r requirements.txt`. |

---

*This tool is provided for educational use. Comply with LinkedIn's terms and use at your own risk.*
