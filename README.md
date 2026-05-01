# CleanSlate AI

CleanSlate AI is a full-stack disk cleanup assistant that turns natural language requests into safe, structured cleanup workflows.

It combines:
- a `FastAPI` backend for planning and execution
- a `React + Vite` frontend for chat-style interaction
- the `OpenAI API` for workflow planning

The app can:
- analyze disk usage
- estimate reclaimable temp/cache space
- clean temp files with explicit confirmation
- find large files without deleting them

## Features

- Natural language chat interface
- Planner-executor architecture
- JSON-based workflow planning
- Confidence-aware execution
- Safety validation for allowed actions only
- Confirmation required before destructive cleanup
- React frontend with fixed-height scrollable chat UI

## How It Works

The app follows a planner + executor flow:

1. You type a request in plain English.
2. The backend sends that request to the OpenAI model.
3. The model returns a structured JSON plan.
4. The backend validates the plan against safety rules.
5. Safe steps run automatically.
6. Destructive cleanup steps require explicit approval.
7. The UI shows only the user-facing final responses instead of all internal planning details.

## Supported Actions

- `CHECK_DISK` → Analyze disk usage
- `GET_TEMP_FILES` → Estimate reclaimable temp/cache storage
- `CLEAN_TEMP_FILES` → Delete temp/cache files, only after confirmation
- `FIND_LARGE_FILES` → Identify large files, read-only

## Tech Stack

- Backend: `Python`, `FastAPI`, `Uvicorn`
- Frontend: `React`, `Vite`
- AI: `OpenAI API`
- Config: `python-dotenv`

## Project Structure

```text
diskcleanup/
├── confidence.py
├── executor.py
├── frontend/
│   ├── package.json
│   ├── vite.config.js
│   └── src/
│       ├── App.jsx
│       ├── main.jsx
│       └── styles.css
├── planner.py
├── requirements.txt
├── schemas.py
├── tools/
│   ├── disk.py
│   ├── files.py
│   └── temp.py
├── validator.py
└── webapp.py
```

## Setup

### 1. Clone the repository

```powershell
git clone <your-repo-url>
cd diskcleanup
```

### 2. Create and activate a Python virtual environment

```powershell
py -m venv venv
.\venv\Scripts\Activate.ps1
```

### 3. Install backend dependencies

```powershell
pip install -r requirements.txt
```

### 4. Add your OpenAI API key

Create a `.env` file in the project root:

```env
OPENAI_API_KEY=your_openai_api_key_here
```

You can also copy from `.env_example` if you keep one in the repo.

### 5. Install frontend dependencies

```powershell
cd frontend
npm install
cd ..
```

## Running the App

You need two terminals.

### Terminal 1: Start the FastAPI backend

From the project root:

```powershell
uvicorn webapp:app --reload
```

Backend default URL:

```text
http://127.0.0.1:8000
```

### Terminal 2: Start the React frontend

From the `frontend` folder:

```powershell
cd frontend
npm run dev
```

Frontend default URL:

```text
http://127.0.0.1:5173
```

Open the frontend URL in your browser.

## How to Use

Type requests such as:

- `hi`
- `check my disk usage`
- `free up disk space`
- `clean temp files`
- `find large files`

### What to expect

- Greeting messages like `hi` return a normal assistant greeting.
- Cleanup-related requests generate a safe plan internally.
- Safe read-only steps run automatically.
- If temp cleanup is needed, the app pauses and asks for approval.
- The chat UI shows only the user prompt and the final assistant-facing result.

### Example requests

#### Example 1

Input:

```text
check my disk usage
```

Expected behavior:

- The app analyzes available disks.
- It returns a disk usage summary.
- No confirmation is needed.

#### Example 2

Input:

```text
free up disk space
```

Expected behavior:

- The app checks disk usage.
- It estimates reclaimable temp space.
- It may ask for confirmation before cleaning temp files.
- It may also report large files.

#### Example 3

Input:

```text
find large files
```

Expected behavior:

- The app scans common user folders.
- It returns a list of large files.
- It does not delete anything.

## Safety Rules

- Only approved actions are allowed.
- Cleanup steps are validated before execution.
- `CLEAN_TEMP_FILES` always requires confirmation.
- Large files are never deleted automatically.
- Low-confidence plans are not executed automatically.

## Notes

- The backend performs disk operations on the machine where it is running.
- This project is intended for local use.
- If you host the backend remotely, cleanup actions will affect the server machine, not the client machine.

## Troubleshooting

### The frontend cannot talk to the backend

Make sure:

- the backend is running on `http://127.0.0.1:8000`
- the frontend is running on `http://127.0.0.1:5173`
- both terminals are active

The Vite dev server is configured to proxy `/api` requests to FastAPI during development.

### OpenAI key not working

Check that:

- `.env` exists in the project root
- `OPENAI_API_KEY` is set correctly
- you restarted the backend after changing `.env`

### `npm` or `py` is not found

Install:

- Python 3.11+
- Node.js 20+

Then reopen your terminal and try again.

## Future Improvements

- Hosted planner API with local executor agent
- Better large-file filtering and ignore rules
- Streaming assistant responses
- Authentication and per-user chat state
- Packaging as a desktop app

## Resume Summary

Built a full-stack AI disk cleanup assistant using `FastAPI`, `React`, and the `OpenAI API` that converts natural language requests into safe, validated cleanup workflows with confirmation-gated execution.
