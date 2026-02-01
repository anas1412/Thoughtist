# Thoughtist - Gemini Context

## 1. Project Overview
**Thoughtist** is a premium, single-file spatial productivity tool designed to bridge the gap between chaotic brainstorming and structured execution. It functions as a local-first web application where "thoughts" are treated as physical objects in an infinite workspace.

### Core Philosophy
-   **Kinetic Information Architecture:** Uses a custom physics engine where thoughts repel each other but are attracted by shared tags.
-   **Dual-View Morphing:** Seamlessly switches between an infinite **Spatial Mode** (Mind Map) and a structured **Kanban Mode** (Todo/Doing/Done).
-   **Local-First:** All data is stored in the browser's `localStorage`. No server, no database.

## 2. Technology Stack
The project is architected as a **Single File Application (SFA)**.

-   **Core:** HTML5, Vanilla JavaScript (ES6+).
-   **Styling:** Tailwind CSS (via CDN) + Custom CSS for glassmorphism and animations.
-   **Libraries (CDN):**
    -   `tailwindcss`: Utility-first CSS framework.
    -   `lucide`: Iconography.
    -   `marked`: Markdown parsing for thought content.
-   **Storage:** `localStorage` (Key: `thoughtist_MASTER_ULTIMATE_STABLE_V900`).
-   **Runtime:** Browser (Client-side only).

## 3. Architecture & Logic

### 3.1. File Structure
The entire application resides in a single file:
-   `index.html`: Contains Structure (HTML), Style (CSS), and Logic (JS).

### 3.2. Data Model
-   **Projects:** The top-level data structure. Users can have multiple isolated workspaces.
-   **Thoughts (`Thought` Class):** The core unit of information.
    -   **Properties:** `x`, `y` (coordinates), `vx`, `vy` (velocity), `text`, `description`, `content` (Markdown), `tags`, `status` ('todo', 'doing', 'done'), `type` ('text', 'tasks', 'paint', 'table').
    -   **Methods:** `createDOM()`, `updateDOM()`, `onStart()` (Drag logic), `renderTypeView()`.

### 3.3. The Physics Engine
A custom physics simulation runs in a `requestAnimationFrame` loop (`loop()` function).
-   **Repulsion:** All nodes repel each other to prevent overlapping.
-   **Attraction:** Nodes with shared `tags` attract each other ("spring force"), creating organic clusters.
-   **Gravity:** A weak central gravity keeps the graph centered.
-   **Damping:** Applied to velocity to stabilize movement.

### 3.4. Rendering System
-   **DOM:** Nodes are `div.thought-bulb` elements absolutely positioned based on `x`/`y` coordinates.
-   **Canvas:** A background `<canvas>` (`#connection-canvas`) draws lines between related nodes (sharing tags) in real-time.
-   **Views:**
    -   **Spatial:** Physics-driven, free-form, zoomable/pannable infinite canvas.
    -   **Kanban:** Rigid columns. Physics is disabled (mostly), and nodes are programmatically positioned into "Todo", "Doing", and "Done" columns.

## 4. Development Workflow

### 4.1. Setup
Since this is a zero-dependency file:
1.  Open `index.html` in any modern web browser.
2.  Edit `index.html` in any text editor/IDE.
3.  Refresh the browser to see changes.

### 4.2. Key Global Variables
-   `thoughts`: Array of `Thought` instances currently active.
-   `projects`: Array of all saved project states.
-   `activeProjectId`: ID of the currently loaded project.
-   `transform`: Global viewport state `{x, y, scale}` for panning/zooming.

## 5. Conventions
-   **Style:** Glassmorphism (`backdrop-filter: blur()`), dark mode default (`--bg: #020408`), and vibrant accent colors.
-   **Code Style:**
    -   Classes for complex entities (`Thought`).
    -   Functional helpers for UI logic (`toggleView`, `save`, `loadProject`).
    -   Direct DOM manipulation (`document.getElementById`).
    -   Tailwind classes for layout and typography.
-   **Icons:** Use `lucide` names (e.g., `<i data-lucide="zap"></i>`) and call `refreshIcons()` after DOM updates.
