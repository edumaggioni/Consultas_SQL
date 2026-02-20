# Copilot / AI agent guidance for Consultas_SQL

This repository contains QSQL/SQL queries and case-study scripts for the RM (TOTVS) system. The files are organized by domain (examples: `Financeiro`, `Imobiliario`) and are intended as runnable queries and documentation of business rules.

Key points for AI coding agents
- Purpose: preserve business logic and readability. These scripts are often executed against a production-like RM database; do not change semantics without explicit user instruction.
- Language & style: comments and descriptions use Brazilian Portuguese. Keep new comments in Portuguese to remain consistent.
- File layout: domain folders (e.g., `Financeiro/`, `Imobiliario/`) contain one-off queries and scenario files. Some files may omit a `.sql` extension (e.g., `Financeiro/Consulta FTB2`); prefer creating `.sql` files for new queries.

Repository patterns (discoverable)
- Header block: many queries should include a short header describing: objetivo (purpose), parâmetros de entrada, dependências de tabelas, e versão RM usada. If missing, add a concise header comment at the top.
- Variable usage: queries commonly declare and set variables (example: `declare @ANO int` then `set @ANO = 2025`). Preserve variable names and default values unless instructed otherwise.
- Date filtering: year/month logic uses `DATEPART`; keep the approach when rewriting temporal filters unless migrating to a safer parameterized approach.
- Aggregation pivoting: examples use `PIVOT (SUM(...))` to produce monthly columns (see `Imobiliario/FaturamantoAno.sql`). Maintain pivot logic when refactoring.
- Table names / RM model: scripts reference RM tables like `FLAN`, `XALGEVENTOFINANCEIRO`, `FCFO`, `GCCUSTO` and integration tables such as `ZT_BAIXAALG_INTEGRACHAVE`. Do not rename columns/tables.

Developer workflows
- There is no build/test system in this repo — queries are validated by running them in the RM SQL client or a compatible SQL engine. When proposing code changes, include a suggested validation step (example SQL client command or quick manual checklist).
- When adding or changing a query, update the top-of-file header with: summary, inputs, outputs, dependencies, and any performance notes.

Agent editing rules (conservative and actionable)
- Non-destructive edits: prefer adding comments, extracting repeated logic into a new query file, or adding a helper `.sql` rather than rewriting an existing script in-place.
- When refactoring, create a new file under the same domain (e.g., `Imobiliario/xxx_refactor.sql`) and keep the original file untouched until user approves replacement.
- Never remove or change `CODCOLIGADA`-related filters or integration keys unless user confirms the change — these are environment scoping controls for RM environments.
- Preserve formatting and capitalization of RM object names (they are often treated case-insensitively but consistent style helps reviewers).

Examples & references
- Monthly pivot example: see `Imobiliario/FaturamantoAno.sql` for `DATEPART/MM` + `PIVOT (SUM(...))` pattern.
- Placeholder/empty files: `Financeiro/Consulta FTB2` exists but contains only whitespace — treat such files as placeholders and ask the user before deleting or renaming.

Merge guidance
- If a `.github/copilot-instructions.md` already exists, merge preserving any author notes and add or update the concise patterns above. Keep the file short (20–50 lines) and specific to this repo.

If anything here is unclear or you want the agent to operate with different permissions (e.g., perform automated refactors and commits), tell me which level of autonomy is allowed.
