# Source: I Built Self-Evolving Claude Code Memory w/ Karpathy's LLM Knowledge Bases
- URL: https://www.youtube.com/watch?v=7huCP6RkcY4
- Title: I Built Self-Evolving Claude Code Memory w/ Karpathy's LLM Knowledge Bases
- Channel: Cole Medin
- Key Topics: Claude Code, Memory, Self-Evolving, Karpathy, LLM Wiki

## Summary
Cole Medin demonstrates how to create a "Self-Evolving" memory system for Claude Code using the LLM Wiki structure proposed by Andrej Karpathy. The core idea is to have Claude Code automatically update its own knowledge base (the Wiki) as it learns new things or completes tasks, allowing the model to stay contextually relevant over time without manual intervention.

### Key Points:
- Uses the `/pages` and `/raw` directory structure.
- Claude Code is instructed via `CLAUDE.md` to update specific pages after every significant interaction.
- Enables "Long-term memory" across different sessions by persisting refined knowledge in Markdown files.
