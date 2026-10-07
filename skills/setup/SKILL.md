---
name: setup
description: Get started with Pethost right after its plugin is installed. Use when the user chooses Set up for Pethost, or asks to set up, try or get started with Pethost. Checks the sign-in and the machine, says what runs there and offers the first deploy. Everything after that is the pethost skill's.
license: MIT
compatibility: Needs the Pethost MCP server (streamable HTTP, sign-in by OAuth) and a Pethost account (https://pethost.dev).
---

# Set up Pethost

The person has just installed Pethost. In a few lines, show them that it works and what to ask
for. This skill only starts: how to deploy and how to look after a project is in the `pethost`
skill, which you follow from there on.

1. **Call `GetMachine` with `{}`.**
   - No Pethost tools in this session, or `UNAUTHENTICATED`: the MCP server is not signed in.
     Start its sign-in, which the person approves in their browser, and call again. Never ask
     for a password or a token.
   - `FAILED_PRECONDITION` with a `NoMachine` detail: they have no machine yet. Give them the
     sentence and the link the error has, and stop until they say it is done: only they can get
     one.
2. **Say what is there, in two or three lines:** the machine in a few words (its plan, how much
   of its memory and disk is used), then each project with its address, and what is wrong with
   it if its `problems` are not empty. A machine with no projects: say that it is ready for the
   first one.
3. **If you have a `Show` tool, call it once with `{}`:** where the person's app shows views,
   they then see their machine in the conversation; where it does not, the answer has the
   panel's link, which you give them. Only if you run in the ChatGPT desktop app (Codex there
   included), add in a line that Pethost is also an app in its sidebar, that a conversation has
   a Project tab, and that `@` in a message names one of their projects. In any other app say
   nothing of these: it has none of them.
4. **Offer the next step, in one line.**
   - The conversation's directory holds a project that is not on the machine (a
     `compose.yaml`, a `Dockerfile`, or an app you could write one for): offer to deploy it, and
     on a yes follow the `pethost` skill.
   - Otherwise give three things they can say: "Deploy this and send me the link", "Why does
     my site fail?", "Show my machine".

If the plugin was installed in the middle of a task, do the first two steps briefly and go back
to the task.
