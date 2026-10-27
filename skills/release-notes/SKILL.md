---
name: release-notes
description: The shape of a release-notes file in this open-source Kortix project — sections, entry format, and the rule for what gets dropped.
---

# Release notes

This skill belongs to an open-source Kortix project: Kortix is open source, and the notes it produces land through a change request.

Write to `notes/release-<to-tag>.md`. Move the previous file to `notes/archive/` if it exists; do not edit a released file.

## Shape

```markdown
# Release <to-tag>

<One sentence: what this release changes for a user of the project.>

## New

- <Behaviour, past tense, one sentence.> (`<hash>`)

## Changed

- <What worked one way and now works another.> (`<hash>`)

## Fixed

- <The symptom the reader saw, then the cause.> (`<hash>`)

## Dropped

<How many commits were left out and why, in one line. No hashes here.>
```

## Rules

- One entry per user-visible behaviour, not per commit. Two commits describing the same change become one entry that lists both hashes.
- The first line of each entry is the behaviour; the parenthetical hash is the evidence.
- Never invent a section that has no entries. An empty `## New` section is a claim that nothing is new, and only the log can make that claim.
- If a commit's effect is unclear after reading it, put it under `## Dropped` and name it. A reader can reopen it; they cannot unread a wrong entry.
