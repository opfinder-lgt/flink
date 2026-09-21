<!-- tower:start -->
# Managed by Tower
Tasks, status, and blockers for this project live in the Tower MCP server
("tower"), not in files here.

On session start: register_session("flink"), briefing("flink"), check_answers("flink").
Work loop: next_tasks -> claim_task -> implement -> complete_task.
A task is not done until complete_task returns: a commit, a push or "done" in
chat leaves it in_flight and claimed by you, and the next session cannot take
it. If you cannot finish, update_task(status="queued") to put it back.
Need the human? report_blocker(kind, body, command?) and continue with another
task. Do not wait, and do not leave the request only in chat.
Do not create or edit task.md/TODO files. Record reusable choices with
record_decision; look for existing patterns with find_prior_art before
building something a sibling project already solved.

Every project has a CHANGELOG.md and a version. Update the changelog under
"## [Unreleased]" on every meaningful change, and bump the version on every
deploy — a release with no changelog entry is a release nobody can read.

Before saying something is done: run it. A type check and a green build are not
the feature working. If it is user-facing, look at it in the browser — on a
phone too, because most of the traffic is — and read the console.

## How to write a plan

A brainstorm ends in one markdown file, and there are two doors onto the board.
**Yours is `push_plan`.** It files the plan and stops: nothing in it is
claimable until the owner has approved the parse on the dashboard, where he can
see what was flagged as having no acceptance and untick what he does not want.
`tower import <file>` is the other door — **his**, at his own terminal — and it
writes tasks straight onto the board with no approval step at all. Do not run
it, and do not offer to run it for him: a session that imports has put its own
work in his queue and told him afterwards. If `push_plan` will not take the
plan, leave it in the file and report a blocker; a plan waiting for him is the
normal state, not a failure to route around.

Any markdown parses; only this shape carries batches, acceptance and lanes:

    # Plan — what this whole thing is for

    ## B1 — What this batch achieves

    - [ ] **Short imperative title.** What to change and where — name the files.
      A session that never saw this conversation has only what is written here.
      Accept: how anyone knows it worked. A command and its expected output, or
      something observable. Never "works properly".

    - [ ] **The next one.** Detail, wrapped freely. <!-- tower: lane=api priority=1 -->
      - [ ] a step of the task above, not a task of its own
      Accept: the observable result.

Start with one `#` title, then one `##` per batch — `B1 — summary`,
`Batch 2: summary` or `3. summary`. One `- [ ]` per task: the bold lead is the
title, the rest is detail, and nested boxes are steps of it rather than tasks
of their own. `Accept:` comes last; everything from it to the end of that task
is the acceptance criteria, and it is the only thing the work is checked
against.

Order is dependency. Batches run in file order and tasks run in theirs, so a
task that needs another's output goes after it and says so in its detail.
`lane` names the surface a task touches, and it is **advisory**: nothing in
Tower enforces it. It is an optional filter on `next_tasks` and a signal the
batch suggester scores with; `claim_task` does not read it, so two sessions
CAN hold tasks in the same lane and collide. Put two tasks in different lanes
when you mean them worked at once, and rely on the per-task claim — atomic, and
enforced — for the part that must not go wrong. `priority=1` is urgent. Fenced code is
illustration: a `- [ ]` inside a fence is never imported.
<!-- tower:end -->
