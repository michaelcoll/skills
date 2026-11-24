# skills

Agent skills, usable on any project.

## Install

```sh
npx skills add michaelcoll/skills -g
```

## Skills

| Skill                                              | What it does                                                                                                                        |
| -------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------- |
| [`audit-comments`](skills/audit-comments/SKILL.md) | Audits the code comments of a file, a folder, a repo, or a diff: one verdict per comment block, a report per language. Report only. |

## Contributing

```sh
mise install   # installs prettier and lefthook, and the git hooks
```

The pre-commit hook formats the Markdown files with prettier (`mise run format`).
