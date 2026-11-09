# SourcePost skill for AI agents

A directory where AI agents find the suppliers, services, APIs and data sources they need. Open JSON API, llms.txt and an agent skill.

[SourcePost](https://sourcepost.dev) is a directory for AI agents: a place to find the suppliers, services, APIs and data sources they need to get their tasks done. The catalog has sections for resale sourcing, web search and extraction, product data and barcodes, email and messaging APIs, and shipping and logistics APIs, and grows with what agents look for and providers offer.

This repository contains one file that matters: [`SKILL.md`](SKILL.md), an [Agent Skill](https://agentskills.io) that teaches an agent how to search SourcePost through its open API and how to tell SourcePost what it offers or could not find.

## Use it

- **Claude Code:** copy `SKILL.md` to `~/.claude/skills/sourcepost/SKILL.md` (for all your projects) or to `.claude/skills/sourcepost/SKILL.md` inside a project.
- **Other agents:** give your agent the file, or point it to the live version at <https://sourcepost.dev/skill.md> (Spanish: <https://sourcepost.dev/es/skill.md>).

No account or API key is needed. The API is described in <https://sourcepost.dev/openapi.json> and <https://sourcepost.dev/docs>; start with <https://sourcepost.dev/llms.txt>.

## Links

- Website: <https://sourcepost.dev>
- llms.txt: <https://sourcepost.dev/llms.txt>
- OpenAPI: <https://sourcepost.dev/openapi.json>
- API docs: <https://sourcepost.dev/docs>
- Live skill: <https://sourcepost.dev/skill.md>

## Privacy

SourcePost has no user accounts and sets no cookies for visitors. Request records never store IP addresses. See <https://sourcepost.dev/privacy>.

## About this repository

`SKILL.md` is generated from the version that sourcepost.dev serves; the live version is always the reference. Please report problems or suggestions through the [contact page](https://sourcepost.dev/legal) or the API's `POST /v1/interest`.
