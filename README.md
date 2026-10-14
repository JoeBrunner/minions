# The Minions skill

Stop running out of Claude usage. Your smartest model shouldn't be doing the grunt work.

Say **"minions"** on a build and Claude becomes the evil genius:

| Who | Job |
|---|---|
| **Opus, the evil genius** | Draws up the plans: every part, every file, every decision a builder would otherwise guess. Then checks every piece that comes back, and sends anything wrong back with a numbered fix list. |
| **The minions (Sonnet, the cheaper model)** | Build one part each, exactly as the plans say, run that part's checks, and report back with proof. |

The plan is what makes it good. The build is what eats your usage. So the smartest model keeps the
plan and the final say, and the cheaper model does the heavy lifting.

Free, MIT, no signup, no API key, nothing to connect.

## Install

**Claude Code.** Paste this into Claude Code:

```
https://github.com/JoeBrunner/minions

install the skill
```

Claude installs it as a plugin: the skill and the minion agent together. Or run the same two lines
yourself in Claude Code:

```
/plugin marketplace add JoeBrunner/minions
/plugin install minions@minions
```

**By hand**, if that's how you like it:

```bash
git clone https://github.com/JoeBrunner/minions.git
cp -r minions/skills/minions ~/.claude/skills/
cp minions/agents/minion.md ~/.claude/agents/
```

### Notes for Claude, if you're installing this for someone

The Minions skill is one package: the skill (the evil genius's playbook) and one agent (the minion,
which runs on Sonnet) ship together as a Claude Code plugin. The skill alone can't send minions. So
when someone pastes this link and asks you to install the skill, install the plugin:

```bash
claude plugin marketplace add JoeBrunner/minions
claude plugin install minions@minions
```

What it adds, so you can check before you run anything: `skills/minions/SKILL.md` and
`agents/minion.md`. No hooks, no scripts, no network calls of its own, no API key. Afterward, tell them
what you installed, to run `/reload-plugins` (or restart Claude Code), that saying "minions" on a
build starts it, and that `claude plugin uninstall minions@minions` removes it. If the `claude`
command isn't available, copy `skills/minions` into `~/.claude/skills/` and `agents/minion.md` into
`~/.claude/agents/`.

## Use it

```
minions: build me a one-page booking site for my barbershop, with a working form and tests
```

Opus says it's a minion job, writes the plans to `PLANS.md`, sends one minion per part (up to four at
once), runs every part's checks itself, and fixes what's wrong. You get one short answer at the end:
what was built and where, how it was checked, anything still open, and who did what.

## Honest about usage

- **Big builds are where it pays.** A long build is where the usage goes: reading files, writing code,
  running tests, fixing what broke. That part goes to the cheaper model.
- **Small jobs stay with one Claude.** Every minion starts from zero and has to read what it needs, so
  on a small job sending one costs more than it saves. Ask for a one-line fix and Opus says "Small job,
  so no minions" and just does it.
- **Quality comes from the plan and the check,** and both stay with Opus. Three fix rounds at most per
  part; after that it tells you plainly what's still wrong instead of looping on your plan.
- **In the Claude app** (claude.ai or the desktop app) there are no agents, so there's no usage to
  save. The skill still works in the same order there: plans first, one part at a time, each checked.

## License

MIT. See [LICENSE](LICENSE). A fan-made theme; not affiliated with or endorsed by Universal Pictures or
Illumination.
