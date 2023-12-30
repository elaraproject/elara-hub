+++
title = "Elara Hub creation plan"
+++

Elara Hub is meant to be a new open-source home of project elara. It uses a git repository for collaboration, and `zola` to generate the docs. When editing it is advised to use `zola serve` to live-preview as you write. Elara hub is meant to read like obsidian, with obsidian-style internal links. In the future, it will also include a file switcher via command palette like obsidian, and maybe a graph view.

Elara Hub supports an extended markdown syntax adapted for technical writing and project management. It includes:

- Table of contents
- Basic text formatting
- Lists
- Links
- Quotes
- Code blocks & syntax highlight
- Horizontal rules
- Footnotes
- Formulas with KaTeX
- Task lists
- Internal linking
- Diagrams with mermaid.js (which includes Gantt charts)
- Plots with charter

Future additions:

- Author support (a post can have many authors) - not sure how good of an idea this is, some pages like `_index.md` might have dozens of authors
- Include the Open Sans font by default

In the future there will be a realtime collaborative editor that integrates with Elara Hub. The editor uses a CRDT that syncs everyone's edits. So on every startup, it loads the latest edit via CRDT (it doesn't store any files on the disk, only in memory, due to how the CRDT/server works). The user can then make their edits and those are automatically reflected on everyone's computer via the CRDT. At any point, users can submit a commit request, which basically freezes the state of the edits, checks out `main` in the Elara Hub repo to create a temporary `staging` branch, and commits the edits to the `staging` branch, as well as opening a pull request for that branch on github. Finally, a number of project-approved maintainers can merge that pull request or close it (depending on their judgement)
