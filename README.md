# Knowledge Grapher

From zero to knowledge graph. A small, single-file web app for building a knowledge graph about anything (yourself, your work, a project, a tool, a topic or an idea), keeping it current, storing it as JSON in GitHub, and handing it to any AI as context.

- **One file.** `index.html` with no build step or account. It works offline from `file://`; the only external request is the Schibsted Grotesk web font, which falls back to the system font when offline.
- **Private by default.** The graph lives in your browser's local storage until you export it.

## Use it

Open `index.html` in a browser and pick one of three starts:

- **Start blank.** Name what the graph is about and pick a kind (person, role, project, topic, tool or idea). That becomes the center. Then answer four quick questions (people, parts, tools, related topics). Leave the name empty to start with an empty canvas.
- **Start from example.** Personal, Work, Project, Topic, Tool or Idea, each with eight entities to explore and edit.
- **Open JSON.** Load a `knowledge-graph.json` from your repo or a download.

Click the logo at any time to return to the start page. "Continue" there takes you back to your graph.

In the workspace:

| Action | How |
| --- | --- |
| Add an entity | **+ Add** or `A`; optionally connect it to something in the same step |
| Edit, connect, delete | Click a node to open its panel |
| Arrange | Drag nodes (they stay where you put them), drag the background to pan, scroll or pinch to zoom, `F` to fit |
| Keep it current | **Review** (`R`) walks through entities not confirmed in 60 days: *Still true*, *Edit* or *Remove* |
| Save to GitHub & use with AI | **Export** (`E`) |

## Export: GitHub + AI

Export produces four files for a private GitHub repo:

| File | Purpose |
| --- | --- |
| `knowledge-graph.json` | The graph and the source of truth. Open it here again to keep editing. |
| `knowledge-graph.md` | Readable digest for any LLM: entities by type with notes, plus `A → relation → B` lines. "Copy AI context" puts it on your clipboard. |
| `AGENTS.md` | Instructions for coding agents (Claude Code, Codex, Cursor, Copilot): how to read the graph and propose safe edits. For Claude Code, add a `CLAUDE.md` containing `@AGENTS.md`. |
| `README.md` | Overview of the repo. |

Commit them by hand in GitHub's web UI (Add file → Upload files). After each review, export again and replace the files; git history becomes the graph's history.

## JSON format (`knowledge-grapher/v1`)

```json
{
  "schema": "knowledge-grapher/v1",
  "meta": { "title": "Portfolio website", "center": "portfolio-website", "updated": "2026-10-17" },
  "entities": [
    { "id": "portfolio-website", "name": "Portfolio website", "type": "project", "notes": "Launch by the end of November", "reviewed": "2026-10-17" },
    { "id": "jordan", "name": "Jordan", "type": "person", "reviewed": "2026-10-17" }
  ],
  "relations": [
    { "from": "portfolio-website", "type": "involves", "to": "jordan" }
  ]
}
```

- **`meta.center`** (optional) is the id of the entity the graph is about. Older graphs with an entity `me` use it as the center.
- **IDs** are slugs that never change, even when you rename an entity.
- **Types and relation types** are lowercase free text.
- **`reviewed`** is the date an entity was last confirmed or edited; Review uses it.
- **Node positions** are kept in the browser only, so dragging doesn't create noisy diffs.
- **Opening a file validates it** and lists readable errors, such as duplicate ids or relations pointing at missing entities. The older `{ nodes, relationships }` format is also accepted.

## Privacy

Nothing leaves the browser until you export. Keep secrets out of the graph and keep the repo private. If you host `index.html` on GitHub Pages, note that Pages sites are public on most plans, even for private repos. The app itself holds no data, but treat the URL as public.
