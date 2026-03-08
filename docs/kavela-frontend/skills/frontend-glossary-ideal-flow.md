# Frontend Glossary & Ideal Flow

> Defines key frontend terminology (spec = skill, group hierarchy) and documents the ideal user flow through the Kavela dashboard.

**Domain:** engineering · **Version:** 1.0.0 · **By:** Yong (Kavela)

**Keywords:** `spec` `skill` `glossary` `definitions` `flow` `dashboard` `sidebar` `editor` `group` `global`

[View on Kavela Marketplace](https://kavela.ai/marketplace/kavela-frontend)

---
## Terminology

- **Skill** = **Spec**: These are the same thing. A skill is the atomic unit of knowledge in Kavela — it has a name, description, content, domain, and keywords. In the dashboard UI, skills are sometimes referred to as "specs" (short for specifications). They are interchangeable.
- **Group**: A logical container for organizing skills. Groups map to teams, projects, or repos (e.g. `kavela-frontend`, `kavela-mcp`). A skill can belong to zero or more groups.
- **Global Skills**: Skills that are not assigned to any group. They appear under the "Global" section in the sidebar, outside of any group folder.
- **Domain**: A category tag on each skill — one of: `engineering`, `legal`, `finance`, `marketing`, `operations`, `general`. Used for filtering and color-coding.

## Ideal User Flow

### 1. Sidebar Navigation
- User sees **groups** listed in the sidebar, each as a collapsible folder.
- Clicking a **group** expands it to reveal the skills inside that group.
- **Global skills** (not in any group) appear in a separate "Global" section below groups.
- Clicking a **skill** (anywhere — inside a group or global) navigates to the editor view.

### 2. Editor View
- When a skill is selected, the editor loads the skill data from the API (`GET /api/skills/:id`).
- The editor displays three editable fields:
  - **Title** (skill name) — large heading
  - **Description** — muted subtitle
  - **Content** — main body with `white-space: pre-wrap` for multi-line text
- Meta information is shown above the title: domain, groups, keywords, last edited timestamp.
- The mock collaborative cursor (e.g. "Sam") remains visible to demonstrate real-time presence.

### 3. Auto-Save
- All edits (title, description, content) are **debounce-saved** to the database after 1.5 seconds of inactivity.
- A "Saving..." indicator appears in the meta bar during save, changing to "Saved" on success.
- When the user switches to a different skill, any pending save for the previous skill is flushed immediately before loading the new one.
- On unmount (leaving the editor), pending saves are also flushed.

### 4. Dashboard Home
- The default view (no skill selected) shows the dashboard home with:
  - Recent skills (sorted by `updated_at`)
  - Domain coverage stats
  - Groups overview
  - Quick action buttons (New Spec, View All)

## API Mapping

| UI Action | API Call |
|-----------|----------|
| Load sidebar groups | `GET /api/groups` |
| Load skills in group | `GET /api/groups/:id/skills` |
| Load all skills | `GET /api/skills` |
| Open skill in editor | `GET /api/skills/:id` (returns skill + groups) |
| Save skill edits | `PUT /api/skills/:id` (debounced) |

## ContentEditable Pattern

The editor uses a **ref-based contentEditable** pattern to avoid the classic React cursor-reset bug:
- Text is set via `ref.current.innerText` (not as React children)
- User input is read via `onInput` → `ref.current.innerText`
- Content changes are stored in `useRef` (not `useState`) to avoid re-renders
- React never touches the DOM text inside contentEditable elements
