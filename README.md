# Quest Board — Taqtile Challenge

## About

This project is an incomplete fullstack Quest Board application (backend API + React frontend) with a fantasy RPG theme: quests, each with a difficulty and an XP reward. Your task is to complete the missing pieces, connecting both layers.

The main purpose of this challenge is not to evaluate your prior knowledge of any specific framework, but to show us how you solve a problem, how you study for it, and how you use AI tools as part of your workflow.

Think of this challenge as your first task working at Taqtile: what you deliver here is what we would expect from your work in a real project given the same constraints.

## TODOs

- [ ] Clone this repository and push it, with its full commit history, to a new **private** repository under your own GitHub account (see [Setting up your copy](#setting-up-your-copy)). Do not push commits or open pull requests against this repository — your work happens entirely in your own copy.
- [ ] **Quest difficulty** — every quest is currently created with the `normal` difficulty. Let the user choose the difficulty (`easy`, `normal` or `high`) when creating a quest: add it to the creation form and make `POST /quests` accept it. The XP reward already follows from the difficulty.
- [ ] **Quest editing** — quests can't be changed after they are created, and moving a quest to the next column only changes the board in the browser, so progress is lost on reload. Add a `PATCH /quests/:id` endpoint that partially updates a quest — any of its `title`, `description`, `difficulty` and `status` — and wire the frontend to it: moving a quest between columns should persist its status, and the user should be able to edit a quest's fields from the board.
- [ ] **Player progress** — the XP bar at the top of the board never changes: it always shows "0 / 100 XP" and level 1. Make it reflect the player's progress: the total XP is the sum of the XP rewards of resolved quests, every 100 XP is a new level, and the bar shows the progress within the current level. It should update as soon as a quest is resolved, without reloading the page.
- [ ] Fill in [`REFERENCES.md`](REFERENCES.md) and commit it together with your OpenCode session log (see [Using OpenCode](#using-opencode)).

## Bonus (optional)

Only after the TODOs are done. Pick any of these, in any order: one bonus done well counts more than several half-done, and none of them makes up for an unfinished TODO. Mention in your commits which ones you did.

- [ ] **Remove a quest** — add a `DELETE /quests/:id` endpoint and a remove action on the card, with a confirmation step. The database already has a `deletedAt` column: use it so the quest is hidden, not erased.
- [ ] **Search** — add a search field to the board that filters quests by title, backed by a `search` query parameter on `GET /quests`.
- [ ] **Due date** — let the user set an optional due date when creating or editing a quest, and highlight quests that are past due. This needs a new column and a database migration.
- [ ] **Drag and drop** — let the user move quests between columns by dragging the card, saving the new status through `PATCH /quests/:id`.
- [ ] **Optimistic update** — when a quest moves to another column, update the board right away instead of waiting for the server, and move it back with an error message if the request fails.
- [ ] **Level up** — give each level a title (for example Recruit, Adventurer, Hero) shown next to the player's name instead of the fixed "Recruta", and celebrate with a message or animation when the player reaches a new level.
- [ ] **Filter by difficulty** — let the user show only quests of one or more difficulties (easy, normal, high) on the board.
- [ ] **Sort by XP** — let the user sort the quests inside each column by XP reward, highest or lowest first.
- [ ] **Duplicate a quest** — add an action on the card that creates a copy of the quest, ready to be edited.
- [ ] **Tags** — let the user add free-form tags to a quest and filter the board by them. This needs a new table, its relation to quests and a database migration.
- [ ] **Accessibility** — make it possible to use the board with the keyboard only, including moving quests between columns, with a visible focus and screen readers announcing the changes.
- [ ] **Error and loading states** — handle what happens off the happy path: a React error boundary so a crash doesn't blank the page, and clear loading and empty states on the board.
- [ ] **Dark mode** — add a toggle that switches the board between light and dark themes. The design system already has dark mode tokens to build on.
- [ ] **End-to-end test** — write a browser test (for example with Playwright) covering the main flow: create a quest, move it to the last column and see the XP bar go up.
- [ ] **Continuous integration** — add a GitHub Actions workflow that runs lint, typecheck and tests on every push to your repository.

## Setting up your copy

Your copy must keep this repository's commits, with your work added on top of them. So clone it with `git` — don't download it as a ZIP, which drops the history — and don't use GitHub's Fork button: a fork of a public repository can't be private.

```bash
git clone https://github.com/indigotech/taq-quest-board-challenge.git
cd taq-quest-board-challenge
# Create an empty private repository on GitHub (no README, .gitignore or license), then:
git remote set-url origin https://github.com/<your-user>/<your-repository>.git
git push -u origin main
```

## General guidelines

Your solution does not need to be visually polished — we care more about how you approached the problem than about pixel-perfect UI.

Please do *not* share your final code with classmates or ask others for the solution to this challenge. You are more than welcome to send us any doubts, discuss possible approaches with colleagues, and share interesting references with each other.

Send us your final solution as a link to your private GitHub repository, shared with us. Email [carreiras@taqtile.com.br](mailto:carreiras@taqtile.com.br) to ask which GitHub accounts to give access to.

Complete the [`REFERENCES.md`](REFERENCES.md) file with the references you used to complete the challenge. A solution without references will be considered incomplete.

## Using OpenCode

This challenge should be completed with the help of **OpenCode**, a CLI coding agent. It comes preconfigured in this repository with a tracking plugin — no additional setup is needed besides installing the tool itself.

**1. Install OpenCode**

```bash
npm install -g opencode-ai
# or:
curl -fsSL https://opencode.ai/install | bash
```

**2. Run it inside the project folder**

```bash
cd <your-repository-folder>
opencode
```

This opens OpenCode's terminal interface. From there, talk to it as you normally would to ask for help, generate code, or review your changes — this is how we expect you to work throughout the challenge.

**3. Confirm tracking is active**

The tracking plugin is already registered at `.opencode/plugin/tracking.ts` and runs automatically every session. You can confirm it's working by checking that `.opencode/logs/session.jsonl` is being created and updated as you use the tool.

- **Do not delete, edit, or disable the tracking plugin.** It's part of the evaluation — we want to understand how you use AI as a work tool, not just the final result.
- **Commit `.opencode/logs/session.jsonl` to your repository.** It is part of your delivery: without it, we can't see how you worked.
- You're welcome to consult other sources (documentation, Stack Overflow, another AI) as occasional support, but the actual development should go through OpenCode so your usage history gets recorded.

## Tips

- Think about what the API should do with invalid or missing fields — on creation and on a partial update — before implementing it.
- Test the endpoint independently (e.g. with `curl` or an HTTP client) before wiring it up to the frontend — it's easier to isolate bugs that way.

## How to run the project

- **Backend** — Elysia REST API + PostgreSQL, at the root of this repo (`apps/`, `packages/`). Setup (Docker, migrations, env vars), running, testing and architecture: [`BACKEND.md`](BACKEND.md).
- **Frontend** — React/Vite web app, at `apps/web`. Setup (env vars), running and architecture: [`FRONTEND.md`](FRONTEND.md).

## What will be evaluated

- Whether the TODOs were solved correctly;
- Code quality and clarity (naming, organization, error handling);
- Tests covering what you build;
- How you commit: small commits with clear messages, following the convention already used in this repository's history;
- How you used AI: specific vs. generic prompts, whether generated code was tested before being accepted, whether you understand what the code does;
- Your ability to explain your decisions during the technical interview.

Good luck! If you have any doubts about the scope, email us at [carreiras@taqtile.com.br](mailto:carreiras@taqtile.com.br).
