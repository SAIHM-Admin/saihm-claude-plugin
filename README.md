# SAIHM

Long-term memory for Claude Code that belongs to you.

SAIHM lets Claude save what matters while you work, recall it in later sessions, share one
memory with another person's agent, take the share back, or erase a memory. Your memories
also come with you to other AI assistants and models that use SAIHM.

SAIHM encrypts each memory on your computer before sending it, so neither SAIHM nor the
storage behind it can read your memories. For fast recall, SAIHM also keeps an
unencrypted copy of your memories on your computer. Only your user account can read it,
and you can go without one (see
[What this plugin runs, sends and keeps](#what-this-plugin-runs-sends-and-keeps)).

This plugin adds the SAIHM MCP server,
[`@saihm/mcp-server-pro`](https://www.npmjs.com/package/@saihm/mcp-server-pro) version
0.14.3, which runs on your computer. It sets up the free tier. Paid plans use a different
setup, described under Plans below.

## Where it works

- **Claude Code**, in the terminal, the IDE extensions and the desktop app's Code tab. The
  plugin is tested in the terminal.
- **Cowork** loads a plugin's local server for tasks that run on your computer, according
  to Anthropic's documentation. SAIHM hasn't been tested there, so join in Claude Code
  first.
- **claude.ai chat** doesn't start a plugin's local servers, so this plugin doesn't run in
  chat. The Claude desktop app's chat can run SAIHM through its own settings, as the
  [guide](https://github.com/SAIHM-Admin/saihm-mcp-server-pro#claude-desktop-anthropic)
  explains.

You need [Node.js](https://nodejs.org) 20.19 or later.

## Install

**Already using SAIHM in Claude Code?** Switch to the plugin only if all three hold:

- you're on the free tier;
- your key is in `~/.saihm/free-identity.key`, which is where a GitHub join puts it;
- your current entry sets no `SAIHM_` setting except `SAIHM_ENDPOINT_URL`.

Then remove the old entry with `claude mcp remove saihm` before you install the plugin, so
that only one copy runs. Otherwise, or if you're not sure, keep your current setup instead
of the plugin, and if you already installed the plugin, uninstall it
(`claude plugin uninstall saihm`) and restart Claude Code.

1. The first start downloads the package from npm. To make it quick, run this once in a
   terminal. It downloads the package, prints its version and exits:

   ```sh
   npx -y @saihm/mcp-server-pro@0.14.3 --version
   ```

2. In Claude Code, add this repository as a plugin marketplace:

   ```
   /plugin marketplace add SAIHM-Admin/saihm-claude-plugin
   ```

3. Install the plugin:

   ```
   /plugin install saihm@saihm
   ```

   If Claude Code asks where to install it, choose user scope. The plugin is then yours
   alone and stays out of any project's shared settings.

4. Restart Claude Code.

You can also run steps 2 and 3 from a terminal with
`claude plugin marketplace add SAIHM-Admin/saihm-claude-plugin` and
`claude plugin install saihm@saihm`.

## Get started

To start a new SAIHM identity, say **"Join SAIHM"** in Claude Code, on a computer you keep.
If you already have one, tell Claude so instead, and see below. No GitHub account? Skip the
join and start at saihm.net/free, as described below. The free join needs no card:

1. If Claude asks whether you already have a SAIHM identity, answer no. If it asks
   whether this computer keeps its home folder between sessions, answer yes, as your own
   computer does.
2. SAIHM creates your key on this computer, and Claude shows you a link and a short code.
3. Open the link, sign in to GitHub, enter the code and approve. The join finishes by
   itself.

Then say **"Recall my SAIHM memories."**

**Already have a SAIHM identity?** Don't join. If you're on the free tier and your key is in
`~/.saihm/free-identity.key` on another computer (from a GitHub join or from saihm.net/free),
first check this computer: if `~/.saihm/free-identity.key` is already there, for example
from an accidental join, keep a copy of it. Then copy the other computer's
`~/.saihm/free-identity.key` to the same place on this computer yourself.
Don't paste it into a chat, where Claude would see it. Make the copied file readable only
by you (on macOS and Linux, `chmod 600 ~/.saihm/free-identity.key`). Then restart Claude
Code and say **"Recall my SAIHM memories."** Claude may suggest `export-identity` instead.
With this plugin, copying the key file is the simpler route.

**No GitHub account, or signed up at [saihm.net/free](https://saihm.net/free)?** Sign up
there if you haven't yet. The page gives you your key file
and the steps to put it in `~/.saihm`, where this plugin reads it. Follow them, then add
SAIHM to Claude Code once, with this plugin or with the page's configuration, since both
add the same server. If `~/.saihm` already holds a key and the page has you keep the new
one in another folder, use the page's configuration instead of this plugin, because this
plugin uses the key in `~/.saihm`. If you use the page's configuration and already
installed the plugin, uninstall the plugin first (`claude plugin uninstall saihm`), so that
only one copy runs. To add the page's configuration, pass only the object under `"saihm"`
to `claude mcp add-json --scope user saihm`. Then restart Claude Code and say
**"Recall my SAIHM memories."** Your key is already active, so skip *"Join SAIHM"*.

**Keep a copy of your key file somewhere safe**, and treat it like a password. SAIHM can't
make you another one, and anyone who has the file can read, change and erase your memories.

## Things to say

You talk to Claude in plain language, and it picks the tools. Claude Code also keeps notes
of its own, so name SAIHM when you want your SAIHM memory. Some starters for a team:

> Liberally use SAIHM to maximize token economy. Recall by topic instead of re-reading files, and save each decision as we make it.

> Recall only my SAIHM memories about the billing service, then give me its open decisions and next steps in five bullets before we write any code.

> Remember in SAIHM as a decision record: webhook retries use idempotency keys, decided 2026-10-20 by Dana; revisit if duplicate charges exceed 0.1%.

> Set a SAIHM invariant: ask me twice before any SAIHM erase, and never store passwords, keys or customer personal data in SAIHM.

> Before we finish, save a SAIHM memory of what we changed, why, and what is still open, so any agent with my key can pick it up tomorrow.

> Share my SAIHM memory about the release runbook with the agent in ./identities/qa-agent.json, read-only, until Friday 17:00 UTC.

> A customer asked us to delete their data. Find every SAIHM memory that mentions Acme Corp, show me each one, and erase them one at a time after I confirm.

> In this CI job, recall the team's SAIHM memory "release rules" and fail the job if the changelog breaks any of them.

Sharing with a colleague needs a little setup, shown in the guide under
[Share with a colleague](https://github.com/SAIHM-Admin/saihm-mcp-server-pro#share-with-a-colleague).
A CI job needs an identity in its settings, set the way the guide's
[Hosted agent environments](https://github.com/SAIHM-Admin/saihm-mcp-server-pro#hosted-agent-environments)
section shows.

More prompts are in the [SAIHM manual](https://saihm.net/manual).

## Tools

| Tool | What it does |
|---|---|
| `saihm_remember` | Encrypts on your machine, then stores it |
| `saihm_recall` | Fetches and decrypts on your machine |
| `saihm_forget` | **Permanently erases.** No undo |
| `saihm_status` | Your identity, plan and memory counts |
| `saihm_share` | Grants one memory to one agent, optionally with an expiry |
| `saihm_revoke_share` | Withdraws that grant |
| `saihm_join` | Starts the free join described above |
| `saihm_governance_propose`, `saihm_governance_vote` | Present, but answer "governance unavailable" for now |

Each tool carries labels for hosts that ask before destructive actions, and
`saihm_forget` is the one marked destructive.

**What erasing reaches.** `saihm_forget` makes the memory permanently unreadable in SAIHM
and removes it from this computer's local copy. Another computer that uses your key drops
it at its next successful recall. Erasing can't reach a conversation that already contains
the memory, including Claude Code's saved session history, and Claude in such a
conversation can save it again, though recall on the computer that erased it may not show
it. Nor can erasing reach what someone you shared it with has already read, or anything
Claude Code wrote about it in its own notes. A backup of your computer keeps the local copy
as it was when the backup was made.

## What this plugin runs, sends and keeps

- It runs `npx -y @saihm/mcp-server-pro@0.14.3`. On the first start, npm downloads that
  package and its dependencies from the npm registry.
- The server connects only to `https://saihm.net`. It sends each memory there encrypted
  when Claude saves it, and fetches memories from there when Claude recalls them. It also
  carries the requests that share and erase memories, and the steps of a join.
- A join asks you to sign in to GitHub in your own browser.
- The server asks Claude to load your memories before other work in each session, so
  your memories become part of the conversation.
- On your computer, the server keeps your key and its own state in `~/.saihm`. It also
  keeps an unencrypted copy of your memories there, so that later recalls fetch only what
  is new or has changed, on this computer or another. The copy holds every memory you save
  or read on this computer and, after the first recall, all of your memories. Only your
  user account can read it, and `forget` removes a memory from it too. This plugin keeps
  the copy on your own computer.
- Without the copy, each recall fetches all of your memories again. That is slower and, on
  a plan that limits recalls, such as the free tier, uses up its allowance faster. If you'd
  rather not keep one, quit every Claude Code window and any other app that runs SAIHM
  with this key, uninstall the plugin (`claude plugin uninstall saihm`, or from the
  Installed tab in `/plugin`, then quit that window too) and delete the `recall.` files in
  `~/.saihm`. Then set up SAIHM with the guide's
  [client steps](https://github.com/SAIHM-Admin/saihm-mcp-server-pro#add-saihm-to-your-ai-client),
  and turn the copy off in each client that uses this key, following
  [How it works](https://github.com/SAIHM-Admin/saihm-mcp-server-pro#how-it-works) in the
  same guide.
- The plugin sets `SAIHM_ENDPOINT_URL`. The server also reads other `SAIHM_` settings, and
  standard variables such as proxy settings and the ones that mark a CI job, from the
  environment Claude Code starts in.
- Apart from those settings, it sees only what Claude passes to its tools, and it reads
  only its own files and the identity record file you name when you share. It doesn't
  read Claude's own memory, your chat history or your other files.

## Plans

Start free. The free tier is a one-time allowance, with no card and nothing to cancel.
Paid plans are monthly, and prices are on the
[pricing page](https://saihm.net/pricing).

This plugin sets up the free tier. A paid plan needs two settings that the plugin
doesn't set, so on a paid plan, uninstall the plugin (`claude plugin uninstall saihm`) and
set up SAIHM with the client steps in the
[guide](https://github.com/SAIHM-Admin/saihm-mcp-server-pro#add-saihm-to-your-ai-client).
[Upgrading by card](https://github.com/SAIHM-Admin/saihm-mcp-server-pro#what-it-costs)
keeps the same key and every memory.

## Remove the plugin

To delete your memories from SAIHM too, ask Claude to erase your SAIHM memories while the
plugin is still installed. Listing your memories is free on every plan, so Claude can find
them to erase.

Then quit every Claude Code window and any other app that runs SAIHM with this key. Run
`claude plugin uninstall saihm`, or uninstall it from the Installed tab in `/plugin` and
then quit that window too.
Also run `claude plugin marketplace remove saihm` if you added this marketplace and no
longer want it. Your key and the local copy stay in `~/.saihm`. Keep the key if you may
come back, because it's the only way to read your memories. To remove the local copy,
delete the `recall.` files in that folder.

## Help

- **The tools don't appear:** first check that you have Node.js 20.19 or later
  (`node --version`). Then run the `npx` command from step 1 of Install once, open `/mcp`
  in Claude Code and reconnect the `plugin:saihm:saihm` server.
- **Guide and reference:**
  [github.com/SAIHM-Admin/saihm-mcp-server-pro](https://github.com/SAIHM-Admin/saihm-mcp-server-pro)
- **Support:** [saihm.net/contact](https://saihm.net/contact)
- **WhatsApp:** message `@saihm.inc` with quick questions, or join the
  [SAIHM community group](https://chat.whatsapp.com/LYYL1MgKe3IE2hcR8qfErf). SAIHM
  personnel never ask for your key file; never send it to anyone.
- **Security reports:** security@saihm.net
- **Privacy:** [saihm.net/privacy](https://saihm.net/privacy)

## License

Apache-2.0 © SAIHM, Inc. See
[LICENSE](https://github.com/SAIHM-Admin/saihm-claude-plugin/blob/main/LICENSE).
