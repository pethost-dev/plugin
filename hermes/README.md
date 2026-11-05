# Pethost for Hermes Agent

[Pethost](https://pethost.dev) is the agent-first cloud for everything you build: your agent
deploys and debugs your apps, and you see all of it in a simple panel. This package gives
[Hermes Agent](https://hermes-agent.nousresearch.com) the Pethost skill, which says when Pethost
is the answer and how to deploy, and the Pethost MCP server, whose tools do the work. Say
“deploy this” and it is live, with HTTPS, backups and logs.

- **Anything Docker runs.** Web apps, APIs, bots, workers and their databases, as Docker
  Compose projects, with [Postgres](https://pethost.dev/postgres/), MariaDB or Valkey in one
  click and [over 80 apps](https://pethost.dev/templates/) to install. No cold starts: nothing
  sleeps.
- **One flat price.** €9.99 a month for as many projects as fit, so you can experiment
  freely: no fee per project, no usage-based pricing, no surprise bill.
- **Everything visible.** Every request for 30 days, live logs, alerts when a site is down or
  slow, and the same projects in [the panel](https://console.pethost.dev/) as your agent sees.

**[Get started](https://console.pethost.dev/)** · [pethost.dev](https://pethost.dev) ·
[About Pethost](https://pethost.dev/about/) · [API reference](https://pethost.dev/docs/api/)

## Install

1. [Subscribe](https://console.pethost.dev/), and a machine is set up for you.
2. Install the package, and connect Hermes to Pethost by OAuth:

   ```sh
   hermes plugins install pethost-dev/plugin/hermes --enable
   hermes mcp add pethost --url https://mcp.pethost.dev/mcp --auth oauth
   hermes config set mcp_servers.pethost.trust untrusted
   ```

   Open the link Hermes shows and approve Hermes on Pethost's page: there is no token to copy.
   The panel's Settings lists the connection under Agents, as “Hermes Agent”, and revokes it.
3. In a new session, say “Deploy this and send me the link.” Hermes packs the project, writes a
   Dockerfile if there is none, deploys it and answers with its address.

The package's format has no way to ask for a browser sign-in, so its own connection names only
Pethost's address, and Pethost answers it that it is not signed in: Hermes has no Pethost tools
until `hermes mcp add` writes the server into Hermes's configuration, where it signs in by OAuth
and takes the package's connection's place. The skill stays the package's. The last line keeps
Hermes asking before every tool that changes something, as the package marks its own connection.

## What to ask

| You say | Hermes |
|---|---|
| “Deploy this and send me the link.” | Uploads the project, builds and deploys it, and answers with its HTTPS address. |
| “Why does the contact form fail?” | Finds the path that fails in the requests, the error in the logs and the line at fault in the code. |
| “Roll back to yesterday's version.” | Puts an earlier deploy or commit back in one step. |
| “Back it up before I change the schema.” | Makes a backup it can restore. |
| “Put it on example.com.” | Adds the address and tells you the one DNS record to make. |

The tools and what each does are in [the plugin's README](https://github.com/pethost-dev/plugin#the-mcp-servers-tools).

## What the package does

- **Network:** it connects Hermes to Pethost's MCP server, `https://mcp.pethost.dev/mcp`, over
  HTTPS, signed in by OAuth from the server you added: the package holds no key or token and
  reads none. It runs no process of its own and has no Python.
- **Approvals:** the package marks its server `untrusted`, and the install's last line marks the
  one you added so, so Hermes asks before every tool that may change something: a deploy, a
  restart, a restore, a deletion, a command in a container. Tools that only read (the machine,
  projects, logs, requests, files) run without asking.
- **The skill:** to move files it has Hermes run `tar` and `curl` in commands the skill itself
  writes, only to `https://` at your machine's own name on Pethost (`….pethost.network`), and
  before an upload it tells you that the folder goes as it is, its `.env` included. It also
  opens a deployed site to check that it answers.
- **The sign-in:** it can deploy to and manage your machine, and cannot change your account or
  pay for anything. The panel's Settings revokes it, under Agents.
