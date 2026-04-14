# Source: Karpathy's LLM Wiki - Full Beginner Setup Guide
- URL: https://www.youtube.com/watch?v=iXd0t60YmMw
- Title: Karpathy's LLM Wiki - Full Beginner Setup Guide
- Channel: Teachers Tech
- Key Topics: Obsidian, Claude Code, AI Wiki Setup, Prompt Engineering for Knowledge Management

## Summary
The video explains how to implement Andrej Karpathy's "LLM Wiki" concept using Obsidian as the UI/Knowledge Base and Claude Code (via terminal) as the AI engine to manage it.

### Workflow:
1.  **Obsidian Setup**: Create a vault with a specific structure (e.g., `wiki/raw`, `wiki/pages`).
2.  **Claude Code Integration**: Run Claude Code in the vault directory. Use a `CLAUDE.md` file to define the agent's behavior (system prompt).
3.  **Ingestion**: Use browser extensions like Obsidian Web Clipper to save online articles as Markdown in `wiki/raw`.
4.  **Processing**: Ask Claude Code to "ingest" the new raw file. The AI reads the raw file and updates or creates multiple entity/concept pages in `wiki/pages`.
5.  **Graph View**: Visualize connections between different nodes of information.
6.  **Quality Control (Linting)**: Ask the AI to check for broken links or conflicting facts across the wiki.

