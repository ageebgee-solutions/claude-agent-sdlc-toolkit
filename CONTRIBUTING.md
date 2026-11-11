# Contributing

Thanks for looking at this. Issues and pull requests are welcome, especially concrete reports of what went wrong or right when you used a role or playbook.

## Before you start

- For a small fix (a typo, a broken link, an unclear sentence), open a pull request directly.
- For a new role or a larger change to an existing one, open an issue first so we can agree on the scope. Use the **Role suggestion** template.
- For a bug in a role or playbook (a rule that contradicts another, an instruction that can't be followed), use the **Bug report** template.

## How changes land

`main` is protected, so every change goes through a pull request. Keep each PR small and focused on one thing: one role, one playbook, or one fix. A PR that rewrites several roles at once is hard to review and hard to revert.

Branch from `main`, make your change, and open a PR against `main`. Describe what you changed and why, and say how you checked it (see below).

## What goes in the toolkit

The toolkit is generic on purpose. Keep it that way:

- **No company-specific content.** No employer or client names, product names, internal URLs, account IDs, hostnames, or people's names outside the existing author credits.
- **No secrets.** No API keys, tokens, connection strings, or example credentials that look real. Use obvious placeholders such as `<your-key>`.
- **Rules must be checkable.** A rule like "write good tests" is hard to follow or verify. Prefer "every bug fix includes a test that fails before the fix and passes after it."
- **Keep roles short.** A role file is a prompt. Long files dilute the rules that matter.
- **Mark outside-world claims.** If a role states something about an external platform, vendor limit, or policy, give a source link and the date you checked it, in the form `Source: <url> | Checked: <date>`. A claim without both is treated as unverified. Don't state external facts from memory.

Role files are plain Markdown with YAML frontmatter, in `agents/`:

```markdown
---
name: your-role-name
description: One sentence saying what the role does and when to use it.
---

# Role Name

## Role
...
```

Playbooks are plain Markdown in `playbooks/`. The existing ones (`bug-fix.md`, `decision-log.md`, `release-validation.md`) are the reference for tone and structure.

## Proposing a new role

Not every role belongs in a small toolkit. Before writing one, check whether an existing role or playbook already covers the job, and say so in the issue.

1. Open a **Role suggestion** issue. Describe the job, the decisions the role makes or the gate it holds, and why an existing role doesn't cover it.
2. If the suggestion fits, write the role file in a PR. Include:
   - A clear boundary: what the role does and what it must not do.
   - Its "always read first" inputs, written generically (for example, "your project's context file").
   - The rules it enforces, each one checkable.
3. Say how you ran it. We'd like to know what you tried, with which model and tool setup, and what the output looked like. A role that has only been read, not run, should say so.

## Changing an existing role

Describe the failure or gap that motivates the change. A real example (what the agent did, what it should have done) is the most useful thing you can include.

## Checking your change

There is no automated test suite for the Markdown. Before you open a PR:

- Read the changed file end to end, and confirm the frontmatter parses (`name` and `description` present, `---` delimiters on their own lines).
- Check that every relative link resolves to a file in the repo.
- If you changed a role's behavior, run it in a scratch project against a realistic request and paste the relevant output into the PR description.

## Reviews and merging

Maintainers review PRs and may ask for changes. Please keep the discussion on the change itself. A PR that doesn't meet this file's rules will be asked to revise rather than merged as-is.

## Conduct and security

Participation is governed by the [Code of Conduct](CODE_OF_CONDUCT.md). To report a security problem, see [SECURITY.md](SECURITY.md). Don't open a public issue for a security problem.

## License

By contributing, you agree that your contribution is licensed under the [MIT License](LICENSE) that covers this project.
