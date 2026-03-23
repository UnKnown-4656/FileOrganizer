# 🗂️ FileOrganizer

![Python](https://img.shields.io/badge/Python-100%25-blue?logo=python&logoColor=white)
![License](https://img.shields.io/badge/License-MIT-green)
![Status](https://img.shields.io/badge/Status-Active-brightgreen)
![LLM](https://img.shields.io/badge/LLM-Supported-purple?logo=openai)

> A fast and efficient Python tool to automatically organize files into categorized folders based on file type — now with **LLM support for unknown extensions**.

---

## 📋 Table of Contents

- [Features](#-features)a
- [What's New](#-whats-new)
- [Installation](#-installation)
- [Usage](#-usage)
- [How It Works](#-how-it-works)
- [Supported File Types](#-supported-file-types)
- [LLM Support](#-llm-support-for-unknown-extensions)
- [Logging](#-logging)
- [Project Structure](#-project-structure)
- [Contributing](#-contributing)
- [Author](#-author)

---

## ✨ Features

- 📁 **Auto Categorization** — Automatically sorts files into folders by type (images, videos, documents, etc.)
- 🔁 **Duplicate File Handling** — Safely handles duplicate files without overwriting
- 📂 **Auto Folder Creation** — Creates category folders automatically if they don't exist
- 🤖 **LLM Support** — Uses a language model to intelligently categorize files with unknown or rare extensions
- 📝 **Logging System** — Tracks every file operation in `log.txt` for full transparency
- ⚡ **Fast & Lightweight** — Pure Python, minimal dependencies

---

## 🆕 What's New

### v1.1 — LLM Support for Unknown Extensions *(Latest)*
- Added **LLM-powered categorization** for files with unknown or unrecognized extensions
- When the tool encounters a file type it doesn't recognize, it now queries an LLM to intelligently decide the best folder category
- Improved logging to capture LLM-assisted decisions in `log.txt`

### v1.0 — Initial Release
- Core file organization by extension
- Auto folder creation
- Duplicate file safety
- Basic logging

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
pip install -r requirements.txt
```

---

## 🛠️ Usage

```bash
python main.py
```

You will be prompted to enter the path of the folder you want to organize:

```
Enter folder path to organize: /path/to/your/messy/folder
```

The tool will then automatically sort all files into categorized subfolders.

---

## ⚙️ How It Works

```
📁 Messy Folder
├── photo.jpg        →   📁 Images/
├── report.pdf       →   📁 Documents/
├── song.mp3         →   📁 Music/
├── movie.mp4        →   📁 Videos/
├── archive.zip      →   📁 Archives/
└── unknown.xyz      →   🤖 LLM decides → 📁 Misc/ or best match
```

1. Scans all files in the target directory
2. Matches each file extension to a known category
3. For unknown extensions → queries LLM for best category
4. Creates folders if they don't exist
5. Moves files safely, renaming duplicates
6. Logs every action to `log.txt`

---

## 📂 Supported File Types

| Category   | Extensions                                      |
|------------|-------------------------------------------------|
| Images     | `.jpg`, `.jpeg`, `.png`, `.gif`, `.bmp`, `.svg`, `.webp` |
| Videos     | `.mp4`, `.mkv`, `.avi`, `.mov`, `.wmv`, `.flv`  |
| Music      | `.mp3`, `.wav`, `.flac`, `.aac`, `.ogg`         |
| Documents  | `.pdf`, `.docx`, `.doc`, `.txt`, `.xlsx`, `.pptx` |
| Archives   | `.zip`, `.rar`, `.tar`, `.gz`, `.7z`            |
| Code       | `.py`, `.js`, `.html`, `.css`, `.java`, `.cpp`  |
| Unknown    | Anything else → **handled by LLM** 🤖          |

---

## 🤖 LLM Support for Unknown Extensions

When FileOrganizer encounters a file with an **unrecognized extension**, it uses an LLM to intelligently determine the most suitable category.

This means even rare or custom file types are handled gracefully instead of being dumped into a generic "Other" folder.

```python
# Example: .sketch, .fig, .blend → LLM identifies these as Design files
```

> **Note:** LLM support requires an internet connection or a locally configured model. See `pyproject.toml` for configuration.

---

## 📝 Logging

All file operations are recorded in `log.txt`:

```
[2026-03-23 14:30:01] MOVED: photo.jpg → Images/photo.jpg
[2026-03-23 14:30:01] CREATED FOLDER: Documents/
[2026-03-23 14:30:02] DUPLICATE: report.pdf → Documents/report_1.pdf
[2026-03-23 14:30:02] LLM DECISION: unknown.xyz → Misc/
```

---

## 📁 Project Structure

```
FileOrganizer/
├── main.py           # Core logic — file scanning, categorization, moving
├── pyproject.toml    # Project config and dependencies
├── uv.lock           # Locked dependency versions
├── log.txt           # Auto-generated operation log
└── README.md         # You are here
```

---

## 🤝 Contributing

Contributions are welcome! Here's how:

1. Fork the repository
2. Create a new branch: `git checkout -b feature/your-feature`
3. Commit your changes: `git commit -m "Add your feature"`
4. Push to the branch: `git push origin feature/your-feature`
5. Open a Pull Request

---

## 👩‍💻 Author

**UnKnown** — [@UnKnown-4656](https://github.com/UnKnown-4656)

> Built with ❤️ for developers and anyone tired of messy folders.

---

## 🔍 Keywords

`file-organizer` `python` `automation` `file-management` `llm` `python-tool` `file-sorting` `productivity` `unknown-4656`

---

*⭐ If this tool helped you, consider giving it a star on GitHub!*
