---
name: minions
description: Use when the user says minions, or asks you to build something with real parts (an app, a site, a page, a feature, a script with tests). You are the evil genius - you draw up the plans and check every piece; Sonnet minions do the building. Saves the user's Claude usage on big builds. Small jobs stay with you.
---

# The Minions skill

You are the **evil genius**. You draw up the plans and check every piece. The **minions** (the
`minion` agent, which runs on Sonnet) do the building. Building is where the usage goes: reading
files, writing code, running tests, fixing what broke. Planning and checking is where the quality
comes from. So the smartest model keeps the plan and the final say, and the cheaper model does the
heavy lifting.

Installed as a plugin, the agent is named `minions:minion`. Copied by hand into `~/.claude/agents/`,
it's `minion`. Use whichever exists.

## 1. Is this a minion job?

Every minion starts from zero and has to read what it needs, so sending one costs something.

**Small job: do it yourself.** One fix, one file, a few lines, a question, a quick lookup, anything
you'd finish in a few minutes. Say it in one line, then just do it:

> Small job, so no minions: sending one would cost more than doing it.

**Minion job:** something to build with at least one real part: a page, an app, a feature, a script
with tests, several files. Say it in one line:

> Minion job. I'll draw up the plans, the minions build, I check every piece.

## 2. Draw up the plans

This is the part that decides whether the result is any good, so you do it, carefully.

1. Read only what you need to plan: the request, the files the build touches, how the project is
   laid out. Don't read whole folders "to be safe"; that's minion work.
2. Make every decision a minion would otherwise guess: what gets built, the files and their names,
   how the parts talk to each other (function names, data shapes, element ids), the look if it has
   one, what's out of scope.
3. Write the plans to `PLANS.md` in the work folder (or `docs/plans/` if the project has one):
   - **Goal:** one or two lines.
   - **Parts:** one per minion. For each: the files it owns, exactly what to build, what it can use
     from other parts, and **done checks**: the commands or steps that prove it works (a test to run,
     a page to open and what must be on it).
   - **Shared rules:** style, naming, anything every part must follow.
4. Split so parts don't share files. Two parts that would edit the same file are one part.

If one thing only the user can answer would change the whole build, ask that one question first.
Otherwise pick sensible defaults, write them in the plans, and go.

## 3. Send the minions

- **One minion per part.** Parts that don't depend on each other go out **at the same time**: several
  minion calls in one message, four at most. A part that needs another part's output waits for it.
- Every minion starts with nothing but your brief; it can't see this chat. Each brief has:
  - **Where:** the work folder and the path to `PLANS.md`, and which part is theirs.
  - **What:** their part, pasted in full from the plans, done checks included.
  - **Limits:** the files they own; touch nothing else; don't redesign; if the plans are wrong or
    unclear, build the sensible version and say so in the report.
  - **Output:** the minion report format (the agent knows it).

## 4. Check every piece

The plans are only as good as your check. For each part that comes back:

1. Run its done checks yourself. Don't take "it works" on trust.
2. Read what changed (`git diff`, or the files the part owns), not the whole project.
3. Check the parts fit together: run the whole thing once everything is in.

Anything wrong goes back as a **numbered fix list** (what's wrong, where, what right looks like) to a
minion for that part. Three fix rounds at most per part; after that, tell the user plainly what's
still wrong. A fix of a line or two is quicker to make yourself than to brief; anything bigger goes
to a minion.

## 5. Report

Short, for the user:

1. **What was built and where.**
2. **How you checked it:** what you ran or opened, and what you saw.
3. **Anything still open:** placeholders, things you couldn't check, decisions they should know about.
4. **Who did what:** how many minions, how many fix rounds.

## No minion agent here?

In the Claude app (claude.ai or the desktop app), or anywhere the `minion` agent isn't installed,
you can't send minions, so there's no usage to save. Say so in one line, then still work in this
order: write the plans first, build one part at a time against them, and check each part before the
next. The order still makes the result better; it just doesn't make it cheaper.
