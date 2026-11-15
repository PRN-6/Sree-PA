# Contributing to Project SREE 🚀

Thank you for your interest in contributing to **SREE – Personal AI Assistant**! SREE is an open-source, local-first, zero-operating-cost personal digital assistant inspired by JARVIS, designed for privacy, speed, and deep Windows OS integration.

We welcome contributions of all kinds: bug fixes, new features, UI enhancements, voice skills, test coverage, and documentation improvements.

---

## 📑 Table of Contents
1. [Architecture Overview](#architecture-overview)
2. [Development Environment Setup](#development-environment-setup)
3. [Git Workflow & Branching Conventions](#git-workflow--branching-conventions)
4. [Coding Standards](#coding-standards)
5. [Running Tests & Quality Assurance](#running-tests--quality-assurance)
6. [Submitting a Pull Request](#submitting-a-pull-request)
7. [Reporting Bugs & Requesting Features](#reporting-bugs--requesting-features)

---

## 🏛️ Architecture Overview

SREE is organized into modular layers:

- **Backend**: FastAPI (Python 3.10+) serving REST endpoints, WebSockets, and static frontend assets.
  - `backend/app/api/`: REST route handlers (Chat, Memory, Notes, Reminders, System Telemetry, Desktop Control, Voice).
  - `backend/app/ai/`: Universal multi-provider router with local Ollama CUDA acceleration, cascading fallbacks, and cloud endpoints (Groq, Gemini, OpenRouter, OpenAI).
  - `backend/app/database/`: SQLite database layer managed via SQLAlchemy with automated schema bootstrapping and WAL mode.
  - `backend/app/automation/`: Allowlist enforcement, 3-tier Security Guard, dynamic Windows path resolver, Skill Crystallizer (0ms fast paths), and PyAutoGUI desktop control.
  - `backend/app/audio/`: Voice biometrics and acoustic voiceprint verification ($F_0$ pitch and spectral resonance).
- **Frontend**: Vanilla HTML5, CSS3, and JavaScript PWA (Progressive Web App) with reactive holographic HUD, audio visualizer, interactive command input, and camera pairing.
- **Desktop Runtime**: `run_sree_app.py` standalone Windows software controller with native system tray icon (`pystray`), auto-startup, and 24/7 background listener.

---

## 🛠️ Development Environment Setup

### Prerequisites
- **Python**: 3.10, 3.11, 3.12, or 3.13
- **Git**: Installed and configured on your system
- **Ollama**: (Optional for local offline inference) Download from [ollama.com](https://ollama.com) and pull `llama3.2:3b`

### 1. Clone the Repository
```bash
git clone https://github.com/adarsh-custom/Sree-PA.git
cd Sree-PA
```

### 2. Set Up Virtual Environment (Recommended)
```bash
python -m venv .venv
# On Windows PowerShell:
.venv\Scripts\Activate.ps1
# On Command Prompt:
.venv\Scripts\activate.bat
```

### 3. Install Dependencies
```bash
pip install -r requirements.txt
```

### 4. Configure Environment
Copy `.env.example` to `.env`:
```bash
cp .env.example .env
```
Optionally configure your desired model or cloud API keys in `.env`.

### 5. Start Backend Server
```bash
python scripts/start.py
# or run directly:
python -m uvicorn backend.app.main:app --host 0.0.0.0 --port 8005 --reload
```
Open your browser at `http://localhost:8005`.

---

## 🌿 Git Workflow & Branching Conventions

Always create a dedicated feature or bugfix branch off `main`:

### Branch Naming Conventions
- `feature/<short-description>`: New features or capabilities (e.g. `feature/spotify-control`)
- `fix/<short-description>`: Bug fixes (e.g. `fix/db-bootstrap-error`)
- `refactor/<short-description>`: Code cleanup or refactoring
- `docs/<short-description>`: Documentation additions or fixes
- `test/<short-description>`: Test suite enhancements

### Creating a Branch
```bash
git checkout -b feature/my-cool-feature
```

### Commit Message Guidelines (Conventional Commits)
Write concise, descriptive commit messages:
- `feat: add support for media player controls in skill crystallizer`
- `fix: resolve sqlite table auto-bootstrap on fresh installations`
- `docs: update quickstart guide and API endpoints reference`
- `test: add unit test coverage for security guard wipe data action`

---

## 📏 Coding Standards

To ensure clean, maintainable, and secure code, follow these principles:

1. **Logging**:
   - Use `from backend.app.core.logger import logger` instead of bare `print()` statements in backend code.
2. **Database Operations**:
   - Always access SQLite via `db_manager` from `backend/app/database/db.py` using SQLAlchemy session context managers (`with self.get_session() as session:`).
3. **Security Confirmation Guard**:
   - Any sensitive action (modifying files, wiping databases, terminating processes) must pass through `SecurityGuard` in `backend/app/automation/security_guard.py`.
   - Never allow unvetted shell execution of Level 3 critical commands (`rmdir /s`, `format`, `reg delete`, `system32`).
4. **Graceful Fallbacks**:
   - Operating system-specific or GUI dependencies (such as `pyautogui` or `pystray`) must be wrapped with try/except fallbacks so the backend can function in headless or non-display environments.
5. **Type Annotations**:
   - Use Python type hints (`from typing import Optional, List, Dict, Any, Tuple`) on all function signatures.

---

## 🧪 Running Tests & Quality Assurance

Before submitting a pull request, ensure all test suites pass with 100% success rate:

### 1. Run Master Unit Test Suite (90 Unit Tests)
```bash
python tests/test_unit_suite.py
```
Validates Dynamic Path Resolution, 3-Tier Security Confirmation Guard, Skill Crystallizer fast-paths, Multi-Provider AI Routing, Database Auto-Bootstrap, SQLite Long-term Memory, Notes, Reminders, and Telemetry.

### 2. Run Desktop Automation Suite (19 Tests)
```bash
python tests/test_desktop_screen_suite.py
```
Validates screen capture, cursor positions, mouse/keyboard automation, OS shortcuts, and voice macros.

### 3. Run Live Master System Integration Test (Optional with Ollama)
```bash
python tests/test_master.py
```

---

## 🚀 Submitting a Pull Request

1. **Push your branch to your fork**:
   ```bash
   git push origin feature/my-cool-feature
   ```
2. **Open a Pull Request**:
   - Target the `main` branch.
   - Provide a clear summary of what changes were made and why.
   - Reference any relevant Issue ID (e.g. `Closes #ISSUE-01`).
3. **PR Checklist**:
   - [ ] All unit tests pass (`python tests/test_unit_suite.py`).
   - [ ] No extraneous debug prints or files checked in.
   - [ ] Documentation updated if user-facing features or settings were added.
   - [ ] Commit history is clean and follows conventional commit style.

---

## 💡 Reporting Bugs & Requesting Features

- For bug reports, use the [Bug Report template](.github/ISSUE_TEMPLATE/bug_report.yml).
- For feature suggestions, use the [Feature Request template](.github/ISSUE_TEMPLATE/feature_request.yml).
- Consult [ISSUES.md](ISSUES.md) for the active backlog of tracked bugs, architecture tasks, and future feature proposals.
