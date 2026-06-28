# Applier_bot — LinkedIn Auto Job Applier

A bot that automates applying to jobs on LinkedIn. It searches for roles you choose,
fills in application forms with your saved answers, and submits Easy Apply applications
for you. Optional AI features can help answer questions and tailor resumes.

> **New here? Read [SETUP_INSTRUCTIONS.md](SETUP_INSTRUCTIONS.md).** It walks you through
> every download and step needed to run this on a fresh Windows laptop.

---

## Quick start

```bash
git clone https://github.com/stger040/Applier_bot.git
cd Applier_bot
pip install -r requirements.txt
```

Then:

1. **Copy the config templates** (they hold personal data, so the real ones are not in the repo):
   ```powershell
   Copy-Item config/secrets.py.example   config/secrets.py
   Copy-Item config/personals.py.example config/personals.py
   Copy-Item config/questions.py.example config/questions.py
   ```
2. **Fill in** `config/secrets.py`, `config/personals.py`, `config/questions.py`,
   `config/search.py`, and `config/settings.py` with your own info.
3. **Set your Chrome version** in `config/settings.py` (`chrome_version_main`) to match the
   version shown at `chrome://settings/help`.
4. **Run it:**
   ```powershell
   python runAiBot.py
   ```
   On the first run you may need to log into LinkedIn manually in the window it opens,
   then click **"Confirm Login"**.

Optional dashboard of applied jobs:
```powershell
python app.py    # then open http://localhost:5000
```

Full details, package list, editor (VS Code / Cursor) instructions, and troubleshooting
are in **[SETUP_INSTRUCTIONS.md](SETUP_INSTRUCTIONS.md)**.

---

## What's in here

| Path | Purpose |
|------|---------|
| `runAiBot.py` | The main bot. |
| `app.py` | Web dashboard for applied-jobs history. |
| `config/` | Your settings. `*.example` files are templates to copy and edit. |
| `modules/` | Core logic (browser control, AI, helpers, resume tools). |
| `requirements.txt` | Python dependencies. |
| `SETUP_INSTRUCTIONS.md` | Step-by-step install + run guide. |

## A note on safety & privacy

- Your `config/secrets.py`, `config/personals.py`, and `config/questions.py` are
  **git-ignored** so your credentials and personal info are never committed. Keep it that way.
- This tool submits **real** applications on your LinkedIn account. For your first runs,
  consider setting `pause_before_submit = True` (in `config/questions.py`) so you can review
  each one.

## Credits & License

Originally created by **Sai Vignesh Golla** —
[Auto_job_applier_linkedIn](https://github.com/GodsScion/Auto_job_applier_linkedIn).

Licensed under the **GNU Affero General Public License v3.0** — see [LICENSE](LICENSE).

> For educational use. Comply with LinkedIn's Terms of Service. Use at your own risk.
