# Source: The Cursor 102: Pro Prompting Guide
- URL: https://www.youtube.com/watch?v=zGFroBGud7w
- Title: The Cursor 102: Pro Prompting Guide
- Channel: Indie Dev (Theo Browne)
- Key Topics: Cursor AI, Tooling, Prompt Engineering, Efficiency, Context Management

## Summary
"Cursor 102" is a guide for advanced users to move beyond basic completions and use Cursor AI as a powerful development partner. It focuses on reducing iterations, managing context costs, and getting reliable code outputs.

### High-Level Strategies (Minimize Turns & Costs):
1. **Q&A Strategy**: Don't let AI guess. Ask it to "Ask me 2-3 clarifying questions" before writing code for complex tasks (e.g., Stripe integration, DB design).
2. **Pros and Cons Strategy**: Use AI to analyze trade-offs (e.g., Resend vs. Mailgun) before choosing a tool to implement.
3. **Role Prompting**: Assign specific roles like "Senior Security Engineer" for code reviews or "React Specialist" for refactoring.

### Context & Efficiency Tips:
- **Specific References**: Use `@file` instead of `@codebase` whenever possible to save tokens and reduce noise.
- **Rules for AI (.cursorrules)**: Move recurring style/architecture rules into a persistent file to save 300-800 tokens per prompt and increase consistency.
- **Plan Mode**: Demand a numbered plan and approval before the AI starts writing code (Shift+Tab in some versions or via explicit prompt).
- **Concise Outputs**: Explicitly tell AI to be brief and avoid "chit-chat" to reduce token burn (e.g., "max 4 sentences explanation").
- **Incremental Workflow**: Break large features into smaller, testable steps (Skeleton -> Auth -> Logic) for easier debugging and cheaper model calls.

