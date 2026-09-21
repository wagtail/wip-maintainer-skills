# wip-maintainer-skills

A collection of skills for Wagtail maintainers. Skills follow the [Agent Skills standard](https://agentskills.io) and are loaded on-demand by the agent.

## Available skills

| Skill | Description |
|---|---|
| [run-tests](run-tests/) | Run the Wagtail test suite correctly in a Wagtail checkout using `./runtests.py`, including recommended flags for fast parallel runs. |
| [write-changelog](write-changelog/) | Write a `CHANGELOG.txt` entry and matching release notes for a merged change, following Wagtail's maintainer-at-merge-time conventions. |

## Installation

Clone the repo and follow the installation instructions for your agent harness to load the skills from the cloned directory. For example, with [pi](https://github.com/badlogic/pi-mono), add the directory to the `skills` array in `~/.pi/agent/settings.json` (global) or `.pi/settings.json` (project-level):

```bash
git clone https://github.com/wagtail/wip-maintainer-skills ~/.wagtail-maintainer-skills
```

```json
{
  "skills": ["~/.wagtail-maintainer-skills"]
}
```
