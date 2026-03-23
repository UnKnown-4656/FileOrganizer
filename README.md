# 🗂️ FileOrganizer

![Python](https://img.shields.io/badge/Python-100%25-blue?logo=python&logoColor=white)
![LLM](https://img.shields.io/badge/LLM-Groq%20AI-orange)
![Status](https://img.shields.io/badge/Status-Active-brightgreen)
![License](https://img.shields.io/badge/License-MIT-green)

> A fast and efficient Python CLI tool that automatically organizes files in any folder into categorized subfolders based on file extension — powered by **Groq AI** for unknown file types.

---

## 📋 Table of Contents

- [Features](#-features)
- [How It Works](#-how-it-works)
- [Supported Categories](#-supported-categories)
- [LLM Support](#-llm-support-for-unknown-extensions)
- [Installation](#-installation)
- [Usage](#-usage)
- [Logging](#-logging)
- [Project Structure](#-project-structure)
- [Known Issues & Notes](#-known-issues--notes)
- [Author](#-author)

---

## ✨ Features

- 📁 **Auto Categorization** — Sorts files into folders by extension using a large built-in extension map
- 🤖 **Groq AI Fallback** — Unknown file extensions are sent to Groq AI which returns the best folder name
- 🔁 **Duplicate File Safety** — Files with the same name get renamed as `file(1).ext`, `file(2).ext` etc. instead of being overwritten
- 📂 **Auto Folder Creation** — Creates destination folders automatically if they don't exist
- 📝 **Logging** — Every file move, duplicate, and unknown extension is logged to `log.txt`
- ⚡ **Fast Lookup** — Uses a dictionary (`ext_map`) for O(1) extension lookup instead of slow loops
- 🖥️ **CLI Based** — Run directly from terminal with a folder path as argument

---

## ⚙️ How It Works

```
📁 Messy Folder (input via CLI)
│
├── photo.jpg       →  📁 images/
├── report.pdf      →  📁 documents/
├── song.mp3        →  📁 audio/
├── movie.mp4       →  📁 video/
├── script.py       →  📁 code/
├── data.json       →  📁 data/
├── model.gguf      →  📁 ml_models/
├── report.pdf      →  📁 documents/report(1).pdf  ← duplicate handled
└── weird.xyz       →  🤖 Groq AI decides → 📁 <ai_suggested_folder>/
```

**Internal flow:**
1. Reads folder path from CLI argument (`sys.argv[1]`)
2. Lists all files using `os.listdir()`
3. Skips subfolders — only processes files
4. Extracts extension using `os.path.splitext()`
5. Looks up extension in `ext_map` dictionary
6. If not found → calls `unknown_file(ext)` which queries Groq AI
7. Groq AI returns a JSON with `{"extension": "...", "folder_name": "..."}`
8. Adds new extension to `ext_map` for future use in same run
9. Creates destination folder if it doesn't exist
10. Checks for duplicates → renames if needed
11. Moves file using `shutil.move()`
12. Logs every action to `log.txt`

---

## 📂 Supported Categories

| Category | Example Extensions |
|---|---|
| `documents` | `.pdf` `.docx` `.txt` `.md` `.epub` `.pages` |
| `spreadsheets` | `.xlsx` `.xls` `.csv` `.ods` `.numbers` |
| `presentations` | `.pptx` `.ppt` `.odp` `.key` |
| `images` | `.jpg` `.png` `.gif` `.svg` `.psd` `.raw` `.heic` |
| `audio` | `.mp3` `.wav` `.flac` `.aac` `.ogg` `.m4a` |
| `video` | `.mp4` `.mkv` `.avi` `.mov` `.webm` `.3gp` |
| `archives` | `.zip` `.rar` `.7z` `.tar` `.gz` `.iso` `.apk` |
| `code` | `.py` `.js` `.ts` `.html` `.css` `.java` `.cpp` `.go` `.rs` |
| `scripts` | `.sh` `.bat` `.ps1` `.cmd` `.bash` |
| `data` | `.json` `.xml` `.yaml` `.toml` `.env` `.parquet` |
| `database` | `.sql` `.db` `.sqlite` `.sqlite3` |
| `fonts` | `.ttf` `.otf` `.woff` `.woff2` |
| `3d` | `.obj` `.fbx` `.stl` `.blend` `.glb` `.dwg` |
| `executables` | `.exe` `.msi` `.dll` `.bin` `.pyc` |
| `subtitles` | `.srt` `.vtt` `.ass` `.sub` |
| `gis` | `.shp` `.geojson` `.gpx` `.kml` |
| `certificates` | `.pem` `.crt` `.p12` `.pfx` `.gpg` |
| `ml_models` | `.pt` `.onnx` `.gguf` `.safetensors` `.keras` |
| `shortcuts` | `.lnk` `.url` `.webloc` |
| `logs` | `.log` `.trace` `.evtx` |
| `unknown` | anything else → **Groq AI decides** 🤖 |

---

## 🤖 LLM Support for Unknown Extensions

When a file extension is **not found** in the built-in `ext_map`, the tool calls **Groq AI**:

```python
def unknown_file(ext):
    groq = Groq(api_key=api)
    response = groq.chat.completions.create(
        model="openai/gpt-oss-20b",
        ...
        response_format={"type": "json_schema", ...}
    )
    return {result["extension"]: result["folder_name"]}
```

- Groq returns a structured JSON response: `{"extension": ".xyz", "folder_name": "misc"}`
- The new extension is **added to `ext_map`** so it's reused within the same run
- The decision is logged in `log.txt`

> ⚠️ Requires a valid `GROQ_API_KEY` in your `.env` file

---

## 🚀 Installation

### Prerequisites
- Python 3.8+
- [uv](https://github.com/astral-sh/uv) *(recommended)* or pip

### Clone the Repository

```bash
git clone https://github.com/UnKnown-4656/FileOrganizer.git
cd FileOrganizer
```

### Install Dependencies

Using `uv` *(recommended)*:
```bash
uv sync
```

Using `pip`:
```bash
pip install groq
```

### Setup API Key

Create a `.env` file in the project root:
```
GROQ_API_KEY=your_groq_api_key_here
```

> 🔑 Get your free API key at [console.groq.com](https://console.groq.com)

---

## 🖥️ Usage

```bash
python main.py "C:\Users\user\Desktop\MessyFolder"
```

**On Linux/Mac:**
```bash
python main.py /home/user/Downloads
```

**What happens:**
- All files in the given folder get sorted into subfolders
- A `log.txt` file is created/updated with every action

---

## 📝 Logging

All operations are appended to `log.txt` in your project directory:

```
photo.jpg Moved to C:\Desktop\MessyFolder\images
report.pdf Moved to C:\Desktop\MessyFolder\documents
Already Exists: report.pdf moved as report(1).pdf
unknown file extension: weird.xyz added to dict
```

---

## 📁 Project Structure

```
FileOrganizer/
├── main.py           # Core logic — scanning, categorizing, moving files
├── pyproject.toml    # Project config and dependencies
├── uv.lock           # Locked dependency versions
├── log.txt           # Auto-generated operation log (git ignored)
├── .env              # Your Groq API key (git ignored ⚠️ never commit this)
├── .gitignore        # Ignores .env and log.txt
└── README.md         # You are here
```

---

## ⚠️ Known Issues & Notes

- `log.txt` is created in the **project directory**, not the organized folder
- The `.env` file **must never be committed** to GitHub — always keep it in `.gitignore`
- Groq AI is called **once per unknown extension** per run — if the same unknown extension appears multiple times, it reuses the cached result
- Currently only supports **flat folders** — files inside subfolders are not organized recursively

---

## 👨‍💻 Author

**UnKnown** — [@UnKnown-4656](https://github.com/UnKnown-4656)

> Built from scratch with Python — learning by doing 🚀

---

## 🔍 Keywords

`file-organizer` `python` `automation` `file-management` `groq-ai` `llm` `cli-tool` `python-tool` `file-sorting` `productivity`

---

*⭐ If this tool helped you, consider giving it a star!*
