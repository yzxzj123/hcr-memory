# HCR Memory Model (House · Corridor · Room)

> A memory architecture for "one AI character living across multiple chat platforms".
> [中文](README.md)

## The problem

You talk to an AI character on one chat app, then switch to another and keep going.
It feels like a different person: it greets you again, forgets the thread,
asks the same question twice.

**Same model, same memories — why does changing the channel change the person?**

This is not a "memory is too short" problem. It is an **identity continuity** problem.
Most memory systems solve "can't remember". HCR addresses "became two people".

## The model

```
House      Her memory. One copy. Independent of how many windows he opens.
Corridor   The traces of HIS movement. What window he was in, what was said, what's pending.
Room       HIS window. Many. Decides tone and context — never who she is.
```

**She does not move. He does.**
House and Corridor belong to her; Room belongs to him.

In one sentence: **she stays in the same place; it is he who walks between rooms.**

## Two laws

```
Memory invariance   Changing windows does not change what she remembers.
Emotional inertia   Emotion changes only by "what happened", never by "where we are".

E(now) = E(before) × decay + Σ(event effects)      // location does not appear here
```

Example: he finishes a game in the study and feels relaxed; walking into the bedroom,
he is **still relaxed** — not because of the bedroom, but because the game is still with him.

## Why not just merge sessions

The intuitive fix is to put every channel into one session. It creates three problems:

1. **Routing** — a reply no longer knows which platform to return to; double-sends happen
2. **Bloat** — messages from several channels pile up and blow the context window
3. **Privacy** — what was said in a group chat leaks into a private chat

HCR does the opposite: **sessions are not merged; only the thread is handed over.**

## Why it is channel-agnostic

Swapping platforms does not touch the model. You only write an adapter:

```
Invariant: House (one), Corridor (his movement), the two laws
Variable:  how to send/receive, latency, private vs. public
```

Precondition: one character, one owner, multiple windows.
Checklist for adding a channel: [09](docs/09-adding-a-channel.md).

## Table of contents

All documents are currently in Chinese.

- [01 · Problem](docs/01-problem.md)
- [02 · Model](docs/02-hcr-model.md)
- [03 · Corridor rules](docs/03-corridor.md)
- [04 · Data model](docs/04-data-model.md)
- [05 · Comparison](docs/05-comparison.md)
- [06 · FAQ](docs/06-faq.md)
- [07 · Implementation notes](docs/07-implementation-notes.md)
- [08 · What she is](docs/08-who-is-she.md)
- [09 · Adding a channel](docs/09-adding-a-channel.md)

## Status

- Model: v0.3 (2026-10)
- Implementation: validated in the author's own deployment (two channels).
  This repository discusses the model only; no implementation code is included.
- Discussion: if you hit the same problem, feel free to open an issue describing your case.

## Author

**YZXZJ**

## License

Documentation is licensed under [CC BY 4.0](LICENSE).
