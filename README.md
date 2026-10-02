<p align="center"><img src="skills/pethost/assets/icon.png" width="96" height="96" alt="Pethost"></p>

# Pethost plugin

[Pethost](https://pethost.dev) is hosting for your own projects: one dedicated machine
that runs your Docker Compose projects. This plugin lets your AI agent deploy to it and look
after what runs there. It has two parts:

- a skill, `skills/pethost`, which tells the agent when Pethost is the answer and how to deploy;
- the Pethost MCP server, `https://mcp.pethost.dev/mcp`, whose tools do the work. It signs
  in by OAuth: you approve the agent in your browser, and revoke it in the panel's Settings.

## Install

Ask your agent: "Install the Pethost plugin from https://github.com/pethost-dev/plugin for
yourself." In an app with no terminal, add this repository as a plugin in the app's settings.

An agent with no plugins takes the two parts apart: the skill from `skills/pethost` (for
example `npx skills add pethost-dev/plugin`), and the MCP server by its address.
