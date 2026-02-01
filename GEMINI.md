# Thoughtist - Gemini Context

## 1. Project Overview
**Thoughtist** is a premium, single-file spatial productivity tool. It functions as a local-first web application where "thoughts" are treated as physical objects in an infinite workspace.

### Core Philosophy
-   **Kinetic Information Architecture:** Uses a custom physics engine where thoughts repel each other but are attracted by shared tags.
-   **Triple-View Morphing:** Seamlessly switches between **Spatial Mode** (Mind Map), **Kanban Mode** (Process), and **Calendar Mode** (Scheduling).
-   **Local-First:** All data is stored in the browser's `localStorage`. No server, no database.

## 2. Technology Stack
The project is architected as a **Single File Application (SFA)**.

-   **Core:** HTML5, Vanilla JavaScript (ES6+).
-   **Styling:** Tailwind CSS (via CDN) + Custom CSS for glassmorphism and animations.
-   **Libraries (CDN):**
    -   `tailwindcss`: Utility-first CSS framework.
    -   `lucide`: Iconography.
    -   `marked`: Markdown parsing.
-   **Storage:** `localStorage` (Key: `thoughtist_MASTER_ULTIMATE_STABLE_V900`). Includes safety checks for storage quota limits.

## 3. Architecture & Logic

### 3.1. File Structure
-   `index.html`: Contains Structure, Style, and Logic.

### 3.2. Data Model
-   **Thoughts (`Thought` Class):**
    -   **Properties:** `x`, `y`, `vx`, `vy`, `text`, `description`, `content`, `tags`, `status`, `type` ('text', 'tasks', 'paint', 'table', 'image'), `date`, `image` (Base64), `drawing` (Base64).
    -   **Dynamic Scaling:** Thoughts scale dynamically based on view mode (Spatial: 1.0, Kanban: 1.0, Calendar: 0.45-0.75).

### 3.3. Positioning Systems
-   **Spatial Mode:** Physics-driven (Repulsion + Tag Attraction).
-   **Kanban Mode:** Columnar stacking based on `status`.
-   **Calendar Mode:** Coordinate-based hit testing. Thoughts "fly" to their assigned date cell using a "Deck of Cards" stacking algorithm (20px vertical offset).

### 3.4. Interaction Engine
-   **Global Event Listeners:** Panning, zooming, and pasting are handled at the `window` level to ensure responsiveness across the entire viewport.
-   **Drag & Drop:** Implements a distance threshold (5px) to distinguish between intentional clicks and drag-release actions.
-   **Clipboard:** Prioritizes `image/` files over `text/plain` to prevent duplicate spawns on paste.

## 4. Visual Layers (Z-Index)
-   **0:** Body Background (Radial Gradient).
-   **5:** UI Overlays (Calendar Grid, Empty State Guide).
-   **10:** Viewport Container.
-   **20:** Thoughts (The Interactive Layer).
-   **9999+:** UI Controls (Tabs, Toolbar, Inspector, Lightbox).

## 5. Development Workflow
1.  **Edit & Refresh:** Direct modification of `index.html`.
2.  **State Sync:** Ensure `save()` is called after any data change.
3.  **UI Sync:** Call `checkEmptyState()` after thought count or mode changes.