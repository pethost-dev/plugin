# Pethost for Hermes Agent

[Pethost](https://pethost.dev) is the agent-first cloud for everything you build: your agent
deploys and debugs your apps, and you see all of it in a simple panel. This package gives
[Hermes Agent](https://hermes-agent.nousresearch.com) the Pethost skill, which says when Pethost
is the answer and how to deploy, and the Pethost MCP server, whose tools do the work. Say
“deploy this” and it is live, with HTTPS, backups and logs.

- **Anything Docker runs.** Web apps, APIs, bots, workers and their databases, as Docker
  Compose projects, with [Postgres or Valkey in one click](https://pethost.dev/postgres/) and
  [over 80 apps](https://pethost.dev/templates/) to install. No cold starts: nothing sleeps.
- **One flat price.** €9.99 a month for as many projects as fit, so you can experiment
  freely: no fee per project, no usage-based pricing, no surprise bill.
- **Everything visible.** Every request for 30 days, live logs, alerts when a site is down or
  slow, and the same projects in [the panel](https://console.pethost.dev/) as your agent sees.

**[Get started](https://console.pethost.dev/)** · [pethost.dev](https://pethost.dev) ·
[About Pethost](https://pethost.dev/about/) · [API reference](https://pethost.dev/docs/api/)

## Install

1. [Subscribe](https://console.pethost.dev/), and a machine is set up for you.
2. In the panel's [Settings](https://console.pethost.dev/settings), under API tokens, create a
   token and copy it: it is not shown again.
3. Give it to Hermes and install the package:

   ```sh
   echo 'PETHOST_API_TOKEN=pth_…' >> ~/.hermes/.env
   hermes plugins install pethost-dev/plugin/hermes --enable
   ```

4. In a new session, say “Deploy this and send me the link.” Hermes packs the project, writes a
   Dockerfile if there is none, deploys it and answers with its address.

### Or sign in by OAuth

The package's format has no way to ask for a browser sign-in, so the package sends a token.
Hermes itself signs in to Pethost by OAuth, with no token to copy, when you add the server to
its own configuration:

```sh
hermes mcp add pethost --url https://mcp.pethost.dev/mcp --auth oauth
```

Open the link Hermes shows and approve Hermes on Pethost's page. That connection replaces the
package's, which then needs no token; the skill stays. To keep Hermes asking before every tool
that changes something, as the package does, add `trust: untrusted` under
`mcp_servers.pethost` in `~/.hermes/config.yaml`. Settings lists the connection under Agents,
as “Hermes Agent”, and revokes it.

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
  HTTPS, sending the token from `PETHOST_API_TOKEN`. It runs no process of its own and has no
  Python.
- **Approvals:** the server is marked `untrusted`, so Hermes asks before every tool that may
  change something: a deploy, a restart, a restore, a deletion, a command in a container. Tools
  that only read (the machine, projects, logs, requests, files) run without asking.
- **The skill:** it has Hermes run the upload or download command a transfer returns (`tar`
  and `curl`, between your computer and your machine on Pethost), and open a deployed site to
  check that it answers.
- **The token:** it can deploy to and manage your machine, and cannot change your account or
  pay for anything. It does not expire: revoke it in Settings when you no longer need it.
  Without it the server does not connect, and Hermes says that `PETHOST_API_TOKEN` is not set.
