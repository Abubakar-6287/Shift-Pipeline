# Project-HR: Visual Pipeline Workflow Builder

A modern web application for creating, managing, and executing complex data processing pipelines with a visual node-based editor interface.

## 📋 Overview

Project-HR is a full-stack application that allows users to design and execute workflows using a visual interface. Users can drag-and-drop nodes, connect them with edges, and execute the resulting pipeline to process data.

## 🛠 Technology Stack

### Frontend
- **React 18.2.0** - UI framework
- **ReactFlow 11.8.3** - Visual node-based workflow editor
- **CSS3** - Styling with dark mode support

### Backend
- **FastAPI** - Modern Python web framework
- **Python 3.x** - Backend runtime
- **CORS Middleware** - Cross-origin resource sharing support

## 📁 Project Structure

```
Project-hr/
├── frontend/              # React frontend application
│   ├── src/
│   │   ├── App.jsx       # Main application component
│   │   ├── toolbar.jsx   # Toolbar component
│   │   ├── ui.jsx        # Main UI component
│   │   ├── submit.jsx    # Submit button component
│   │   ├── savedList.jsx # Saved pipelines list
│   │   ├── store.js      # State management
│   │   ├── index.js      # Entry point
│   │   ├── index.css     # Global styles
│   │   └── nodes/        # Node components
│   │       ├── BaseNode.jsx
│   │       ├── inputNode.jsx
│   │       ├── outputNode.jsx
│   │       ├── apiNode.jsx
│   │       ├── filterNode.jsx
│   │       ├── mathNode.jsx
│   │       ├── textNode.jsx
│   │       ├── llmNode.jsx
│   │       ├── loggerNode.jsx
│   │       └── delayNode.jsx
│   └── public/           # Static assets
├── backend/              # FastAPI backend
│   └── main.py          # Backend server and endpoints
└── README.md            # Project documentation
```

## 🚀 Getting Started

### Prerequisites
- Node.js 14+ and npm
- Python 3.8+

### Frontend Setup

```bash
cd frontend
npm install
npm start
```

The frontend will start on `http://localhost:3000`

### Backend Setup

```bash
cd backend
pip install fastapi uvicorn pydantic
python main.py
```

The backend will run on `http://localhost:8000`

## 📦 Features

### Node Types
- **Input Node** - Define pipeline inputs
- **Output Node** - Display pipeline results
- **API Node** - Call external APIs
- **Filter Node** - Filter data based on conditions
- **Math Node** - Perform mathematical operations
- **Text Node** - Manipulate text data
- **LLM Node** - Integration with language models
- **Logger Node** - Log data for debugging
- **Delay Node** - Add delays in pipeline execution

### Core Features
- ✅ Visual node-based pipeline editor
- ✅ Drag-and-drop interface
- ✅ Pipeline parsing and validation
- ✅ Save/load pipelines
- ✅ Dark mode support
- ✅ Responsive design
- ✅ Real-time pipeline execution

## 🔌 API Endpoints

### GET `/`
Health check endpoint
- Returns: `{"Ping": "Pong"}`

### POST `/pipelines/parse`
Parse and validate a pipeline configuration
- Request body:
  ```json
  {
    "nodes": [{ "id": "node1", ... }],
    "edges": [{ "source": "node1", "target": "node2" }]
  }
  ```
- Returns: Pipeline validation results and execution order

## 🎨 Customization

- **Colors & Dark Mode**: Edit `src/index.css`
- **Node Styles**: Modify `nodes/` component files
- **Toolbar**: Customize `src/toolbar.jsx`
- **Backend Logic**: Extend `backend/main.py`

## 📝 Recent Updates

- Initial project setup and structure
- Frontend visual editor with ReactFlow
- Multiple node type implementations
- Backend API with CORS support
- Pipeline parsing and validation
- Saved pipelines management

## 🔧 Development

### Run frontend dev server
```bash
cd frontend
npm start
```

### Run backend dev server
```bash
cd backend
uvicorn main:app --reload
```

## 📄 License

This project is private and confidential.

## 👤 Author

Created and maintained by the development team.

---

**Last Updated**: February 17, 2026

---

## 📝 Changelog Summary

This section tracks how the codebase and features evolved from the very first edits to the current state.

1. **Initial commit**
   - Simple React/ReactFlow frontend with drag‑and‑drop canvas.
   - Only a few node types and basic layout, no persistence or undo.
   - Backend: `main.py` with FastAPI, a `/` ping endpoint and a `/pipelines/parse` route for basic pipeline data validation.

2. **State management and auto‑save**
   - Introduced `store.js` using Zustand for nodes/edges, autoSaveStatus, and draft persistence in localStorage.
   - Frontend could now add and move nodes, with unsaved status indicator.

3. **Undo/Redo history**
   - `saveToHistory`, `undo`, `redo`, `historyIndex` added to the store.
   - Every node/edge change automatically pushed a history entry, allowing step‑by‑step undo (Ctrl+Z) and redo.

4. **Custom controls and UI polish**
   - Created `customControls.jsx` with zoom buttons, fit view, opacity slider, and undo/redo buttons.
   - Added dark mode toggle, zoom tracking, and improved toolbar layout.

5. **Backend enhancements**
   - CORS middleware configured for development.
   - Pipeline parsing logic updated to compute number of nodes/edges and DAG validation.

6. **Optimized history behavior**
   - History entries were noisy (one per pixel during drag). Changed to manual saving:
     - disabled automatic saves in `onNodesChange` and `onEdgesChange`.
     - added `saveToHistory()` call on `onNodeDragStop` in `ui.jsx`.
   - This allowed a 200px move to be undone in a single click instead of many steps.

7. **Additional features**
   - `undoAll` (reset to first snapshot) proposed/added for quick full rollback.
   - Edge operations and connect events could also trigger history when needed.
   - Added auto‑save drafts and load/clear draft functions in store.

8. **Usability updates**
   - Added drag‑over/drop handlers for bringing new nodes into the canvas.
   - Saved pipelines list and submit button components were built out.
   - Zoom/fit and panning adjustments improved for smoother interaction.

9. **Current state (Feb 2026)**
   - Full undo/redo with efficient history control, one‑click back movement, and undoAll support.
   - Persistent dark mode, auto‑save status indicators, and custom UI controls.
   - Backend remains lightweight but supports DAG validation and can be extended for execution logic.

The project has moved from a basic visual editor to a robust workflow builder with history, persistence, and user-friendly controls—all while keeping the backend simple and extensible.

> **Note:** all of the features listed above (history/undo, custom controls, auto‑save drafts, undoAll, drag‑stop history optimization, dark mode, zoom/pan controls, etc.) were not present in the initial commit; they were progressively added over time as the codebase evolved.

## ⚙️ Architectural & Node Enhancements

A few deeper technical improvements came from detailed refactoring notes:

- **BaseNode abstraction** introduced as a reusable wrapper for common layout, styling, and handle logic. This eliminated duplication across nodes and made the component system scalable and DRY.
- **Node refactors**
  - *OutputNode* and *LLMNode* were cleaned up to only provide behavior/configuration; shared structure moved into `BaseNode`. Styling centralized and handles became configuration-driven rather than hardcoded positions.
  - *TextNode* gained auto‑resizing textarea and regex-based variable detection. Input handles are generated dynamically for variables like `{{name}}`, duplicates removed automatically.
- **Styling upgrade**: BaseNode was later replaced with a new styled version featuring dark UI, consistent padding, rounded corners, and colored handles. This allows future design changes from a single file.
- **Submit button integration**: Earlier a dummy button, it now extracts nodes/edges via `useReactFlow`, sends them to the backend POST `/pipelines/parse`, and displays node/edge count plus DAG status in an alert.
- **Backend DAG logic**: Implementation uses Kahn’s algorithm for cycle detection (compare visited node count to total nodes). The `/pipelines/parse` endpoint switched from GET to POST and now parses JSON payloads, returning `num_nodes`, `num_edges`, and `is_dag`.

These architectural changes transformed the node system into a flexible, configuration-driven designer where adding new node types takes minutes, and the backend supports real validation logic.
