# gurditt-skills

Claude Code plugins for the 1mg Health Records (HIH) mweb team.

| Plugin | What it does |
| --- | --- |
| `hih-qa` | QA and UX audits of HIH mweb pages in a mobile browser, with severity-ranked reports, short screen recordings for issues, and publishing to Confluence. |

## Install

In Claude Code, add this repo as a marketplace, then install the plugin from it:

```
/plugin marketplace add gurditt-khurana/skills
/plugin install hih-qa@gurditt-skills
```

Restart Claude Code (or run `/reload-plugins`) if the skill doesn't show up straight away.

## Use

Run the skill and say what to test and where:

```
/hih-qa:hih-qa do a qa of the health score page on stag
```

- Name the page or flow, or give a PR or branch. Without one, it asks. A full-suite run (every page) takes over an hour, so only ask for it when you need it.
- Environment: `stag` and `earth` sign in with the built-in test account. On prod it asks for your number and the OTP.
- Output goes to `~/Desktop/hih-qa-audits/<pages>_<date>/`: `report.md`, `ux-audit.md`, and the screenshots and clips for issues only.

## What it needs

- A browser tool that can drive a mobile browser, e.g. the Playwright MCP.
- Node and npm, used once to fetch `ffmpeg-static` into `~/.cache/hih-qa-ffmpeg` for trimming recordings.
- The Atlassian MCP, only if you want the audit published to the Health Records (`HEA`) Confluence space.

## Update

```
/plugin marketplace update gurditt-skills
/plugin update hih-qa@gurditt-skills
```

Updates are picked up when `version` in `plugins/hih-qa/.claude-plugin/plugin.json` changes.

## Uninstall

```
/plugin uninstall hih-qa@gurditt-skills
/plugin marketplace remove gurditt-skills
```

## Changing the skill

The skill lives in `plugins/hih-qa/skills/hih-qa/`: `SKILL.md` is the entry point, `ux-audit.md` covers recordings, and `pages/` has one file per HIH page. When you change it, raise `version` in `plugins/hih-qa/.claude-plugin/plugin.json`, otherwise installed copies won't update.
