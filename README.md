<h1 align="center">🤖 ATLAS — AI Task & Automation System</h1>

<p align="center">
  <img src="https://img.shields.io/badge/Python-3.10+-3776AB?style=for-the-badge&logo=python&logoColor=white" />
  <img src="https://img.shields.io/badge/Platform-Windows%2010%2F11-0078D6?style=for-the-badge&logo=windows&logoColor=white" />
  <img src="https://img.shields.io/badge/LLM-Ollama%20Local-FF6F00?style=for-the-badge" />
  <img src="https://img.shields.io/badge/License-MIT-green?style=for-the-badge" />
</p>

<p align="center">
  A fully autonomous AI agent powered by a <strong>local Ollama LLM</strong> that can
  plan, execute, and verify desktop automation tasks on Windows.
</p>

---

## ✨ Features

| Category | Capabilities |
|----------|-------------|
| 📄 **Files** | Create, read, edit, delete, copy, move, search, append text files |
| 📊 **Excel** | Create workbooks with formatted tables, auto-sum, charts (bar/line/pie) |
| 📸 **Screenshots** | Full-screen or region capture, saved automatically to `screenshots/` folder |
| 🌐 **Web** | Open URLs, fill forms, click buttons, scrape page content |
| 💻 **Shell** | Execute CMD & PowerShell commands, get system info |
| 🤖 **GUI Automation** | Mouse clicks, keyboard typing, hotkeys |
| 📈 **Charts** | Generate bar, line, pie, scatter charts via matplotlib |
| 📝 **Word Documents** | Create `.docx` with headings, paragraphs, bullet lists |

### 🧠 Agent Intelligence

- **Plan → Execute → Verify** — every task goes through a full lifecycle
- **Self-healing** — automatic retries with LLM-corrected parameters on failure
- **Safety** — dangerous commands are blocked, plans require user confirmation
- **Path intelligence** — understands "Desktop", "Documents", "Downloads" as real paths

---

## 🚀 Quick Start

### 1. Prerequisites

Make sure [Ollama](https://ollama.ai) is installed and running:

```bash
ollama serve
ollama pull jobautomation/OpenEuroLLM-Polish:latest
```

> 💡 You can use **any Ollama model** instead of the default one. See [Changing the Model](#changing-the-model) in the Configuration section.

### 2. Clone & Install

```bash
git clone https://github.com/yourusername/atlas-agent.git
cd atlas-agent
pip install -r requirements.txt
```

### 3. Run

```bash
# Interactive mode
python atlas.py

# Single task mode
python atlas.py --task "Create a file hello.txt on Desktop with text Hello World"

# Custom model
python atlas.py --model "your-model:latest"
```

---

## 💬 Usage Examples

```
🤖 ATLAS> Create a budget Excel on Desktop with food, transport, bills and add a pie chart

🤖 ATLAS> Take a screenshot of my screen

🤖 ATLAS> Open wikipedia.org and scrape the main page title

🤖 ATLAS> List all .txt files on my Desktop

🤖 ATLAS> Create a Word document with a quarterly report

🤖 ATLAS> Run systeminfo in PowerShell

🤖 ATLAS> Create a bar chart of monthly sales: Jan 100, Feb 150, Mar 200
```

### Interactive Commands

| Command | Description |
|---------|-------------|
| `/help` | Show help and examples |
| `/tools` | List all 29 available tools |
| `/status` | System & dependency status |
| `/history` | View past tasks |
| `/clear` | Clear conversation memory |
| `/exit` | Quit ATLAS |

---

## 🏗️ Architecture

```
                        User Request
                             │
                             ▼
                      ┌─────────────┐
                      │  Ollama LLM │
                      │  (Planning) │
                      └──────┬──────┘
                             │ JSON Plan
                             ▼
                      ┌─────────────┐
                      │  Response   │
                      │  Parser     │
                      └──────┬──────┘
                             │ TaskPlan
                             ▼
                      ┌─────────────┐
                      │  Execution  │◄──── Retry + Self-Heal
                      │  Engine     │
                      └──────┬──────┘
                             │
          ┌──────┬───────┬───┴────┬────────┬─────────┐
          ▼      ▼       ▼       ▼        ▼         ▼
       ┌──────┬───────┬───────┬───────┬────────┬─────────┐
       │ File │ Excel │  Web  │ Shell │ Screen │  Chart  │
       │ Tools│ Tools │ Tools │ Tools │ Tools  │  Tools  │
       └──────┴───────┴───────┴───────┴────────┴─────────┘
                             │
                             ▼
                      ┌─────────────┐
                      │  Ollama LLM │
                      │(Verification)│
                      └─────────────┘
```

---

## 📁 Project Structure

```
atlas/
├── atlas.py               # Main agent script (single file)
├── requirements.txt       # Python dependencies
├── README.md              # This file
├── LICENSE                # MIT License
├── screenshots/           # Auto-created: saved screenshots
└── logs/                  # Auto-created: execution logs
    └── atlas_YYYYMMDD_HHMMSS.log
```

---

## 🧰 All 29 Tools

<details>
<summary>Click to expand full tool list</summary>

### File Operations

| Tool | Parameters | Description |
|------|-----------|-------------|
| `create_text_file` | path, content | Create or overwrite a text file |
| `read_file` | path | Read file contents |
| `edit_file` | path, old_text, new_text | Find & replace text in file |
| `delete_file` | path | Delete a file |
| `list_files` | directory | List directory contents |
| `create_directory` | path | Create directory tree |
| `copy_file` | source, destination | Copy a file |
| `move_file` | source, destination | Move or rename a file |
| `search_files` | directory, pattern | Recursive glob search |
| `append_to_file` | path, content | Append text to file |

### Excel

| Tool | Parameters | Description |
|------|-----------|-------------|
| `create_excel` | path, data, sheet_name | Create formatted workbook |
| `edit_excel` | path, sheet_name, cell, value | Set a cell value |
| `add_excel_chart` | path, chart_type, title | Add bar/line/pie chart |
| `read_excel` | path | Read all rows from workbook |

### Screenshots

| Tool | Parameters | Description |
|------|-----------|-------------|
| `take_screenshot` | filename | Full-screen capture |
| `screenshot_region` | x, y, width, height, filename | Region capture |

### Web / Browser

| Tool | Parameters | Description |
|------|-----------|-------------|
| `open_url` | url | Open URL in browser |
| `web_fill_form` | url, fields | Fill form fields |
| `web_click` | selector | Click a web element |
| `web_scrape` | url, selector | Extract page data |

### Shell

| Tool | Parameters | Description |
|------|-----------|-------------|
| `run_shell` | command | Execute CMD command |
| `run_powershell` | command | Execute PowerShell command |
| `get_system_info` | — | Get system information |

### GUI Automation

| Tool | Parameters | Description |
|------|-----------|-------------|
| `mouse_click` | x, y | Click at screen coordinates |
| `type_text` | text | Type text via keyboard |
| `hotkey` | keys | Send keyboard shortcut |
| `wait_seconds` | seconds | Wait / sleep |

### Charts & Documents

| Tool | Parameters | Description |
|------|-----------|-------------|
| `create_chart` | data, chart_type, title, filename | Matplotlib chart |
| `create_word_document` | path, content | Word .docx document |

</details>

---

## ⚙️ Configuration

### Changing the Model

By default ATLAS uses `jobautomation/OpenEuroLLM-Polish:latest`, but you can use **any model available in Ollama**. To switch:

**1. See what models you have installed:**
```bash
ollama list
```

**2. Pull a different model (if you don't have it yet):**
```bash
ollama pull qwen2.5:1.5b-instruct
```

**3. Run ATLAS with your chosen model:**
```bash
python atlas.py --model "qwen2.5:1.5b-instruct"
```

> 💡 Models with good instruction-following work best (e.g. `qwen2.5`, `llama3`, `mistral`, `gemma2`). Larger models produce more reliable JSON plans but require more RAM.

**To change the default model permanently** (without using `--model` every time), edit `atlas.py` in two places:

```python
# 1. In OllamaEngine.__init__:
model_name: str = "your-model:latest"

# 2. In main() argparse:
default="your-model:latest"
```

---

### Command-line Arguments

| Flag | Description | Default |
|------|-------------|---------|
| `--model` | Ollama model name | `jobautomation/OpenEuroLLM-Polish:latest` |
| `--task` | Run single task then exit | *(interactive mode)* |

### LLM Parameters (tuned for speed)

| Parameter | JSON mode | Chat mode | Purpose |
|-----------|-----------|-----------|---------|
| temperature | 0.1 | 0.5 | Lower = more deterministic plans |
| num_predict | 768 | 1024 | Max tokens generated |
| num_ctx | 2048 | 2048 | Context window (RAM-friendly) |
| repeat_penalty | 1.2 | 1.2 | Prevents repetitive output |

---

## 🛡️ Safety Features

- ⛔ Blocked commands: `format`, `del /s /q c:`, `shutdown`, `rm -rf /`
- ✅ User confirmation required before executing any plan
- 🔒 Path validation via `PathResolver` — no accidental system file access
- 🔍 Post-execution verification by the LLM
- 📝 Full logging of every action to `logs/` directory

---

## 🖥️ System Requirements

- **OS:** Windows 10 / 11
- **RAM:** 8 GB minimum, 16 GB recommended
- **Python:** 3.10 or newer
- **Ollama:** Running locally with a pulled model
- **Chrome:** Required only for web automation (Selenium)

Key Python dependencies: `ollama`, `openpyxl`, `matplotlib`, `selenium`, `webdriver-manager`, `pillow`, `rich`, `python-docx`, `beautifulsoup4`, `requests`

---

## 🔧 Troubleshooting

**LLM doesn't generate a valid plan / JSON errors**
This usually means the model is too small or not instruction-tuned well enough. Try a larger or better model. Models with at least 7B parameters work most reliably. Smaller models (1.5B–3B) may struggle with complex multi-step tasks.

**"Model not found" error**
Make sure the model name in the command matches exactly what `ollama list` shows — including capitalisation and tag (e.g. `:latest`).

**Selenium / Chrome not working**
Make sure Google Chrome is installed. The `webdriver-manager` package downloads the correct ChromeDriver automatically, but it requires an internet connection on first run.

**Screenshots not working**
Requires either `Pillow` or `pyautogui` to be installed. Run `pip install Pillow pyautogui` and try again.

**Tasks work but results are wrong**
Try increasing `num_ctx` in the code (default: 2048). For complex tasks, the LLM may lose context. Setting it to 4096 helps but requires more RAM.

**PyAutoGUI FailSafeException**
Move your mouse to the top-left corner of the screen triggers a safety stop. This is intentional — just rerun the task.

---

## 🤝 Contributing

1. Fork the repository
2. Create a feature branch (`git checkout -b feature/new-tool`)
3. Commit your changes (`git commit -m 'Add new tool'`)
4. Push to the branch (`git push origin feature/new-tool`)
5. Open a Pull Request

---

## 📄 License

This project is licensed under the MIT License — see the [LICENSE](LICENSE) file for details.

<p align="center">Built with ❤️ and local AI</p>
