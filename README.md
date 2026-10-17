# Knowledge Grapher

From zero to knowledge graph. A small, single-file web app for building a personal knowledge graph, keeping it current, storing it as JSON in GitHub, and handing it to any AI as context.

- **One file.** `index.html` with no dependencies, build step or account. It works offline from `file://`.
- **Private by default.** The graph lives in your browser's local storage until you export it.

## Use it

Open `index.html` in a browser and pick one of three starts:

- **Start with me.** Answer five quick questions (you, people, projects, tools, topics) and get a graph of 10–20 entities centered on you.
- **Try an example.** Explore Work life, Learning a field or Life admin, then edit it or start your own.
- **Open a JSON file.** Load a `knowledge-graph.json` from your repo or a download.

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
  "meta": { "title": "My knowledge graph", "updated": "2026-10-17" },
  "entities": [
    { "id": "me", "name": "Me", "type": "person", "notes": "Product manager", "reviewed": "2026-10-17" },
    { "id": "maya-chen", "name": "Maya Chen", "type": "person", "reviewed": "2026-10-17" }
  ],
  "relations": [
    { "from": "me", "type": "works with", "to": "maya-chen" }
  ]
}
```

- **IDs** are slugs that never change, even when you rename an entity.
- **Types and relation types** are lowercase free text.
- **`reviewed`** is the date an entity was last confirmed or edited; Review uses it.
- **Node positions** are kept in the browser only, so dragging doesn't create noisy diffs.
- **Opening a file validates it** and lists readable errors, such as duplicate ids or relations pointing at missing entities. The older `{ nodes, relationships }` format is also accepted.

## Privacy

Nothing leaves the browser until you export. Keep secrets out of the graph and keep the repo private. If you host `index.html` on GitHub Pages, note that Pages sites are public on most plans, even for private repos. The app itself holds no data, but treat the URL as public.
