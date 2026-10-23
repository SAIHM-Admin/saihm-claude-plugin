# SAIHM

Long-term memory for Claude Code that belongs to you.

SAIHM lets Claude save what matters while you work, recall it in later sessions, share one
memory with another person's agent, take the share back, or erase a memory. Your memories
also come with you to other AI assistants and models that use SAIHM.

SAIHM encrypts each memory on your computer before sending it, so neither SAIHM nor the
storage behind it can read your memories. For fast recall, the server also keeps an
unencrypted copy of your memories on your computer. Only your user account can read it,
and [What this plugin runs, sends and keeps](#what-this-plugin-runs-sends-and-keeps) says
how to go without one.

This plugin adds the SAIHM MCP server,
[`@saihm/mcp-server-pro`](https://www.npmjs.com/package/@saihm/mcp-server-pro) version
0.14.2, which runs on your computer. It sets up the free tier. Paid plans use a different
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
that only one copy runs. Otherwise, or if you're not sure, keep your current setup.

1. The first start downloads the package from npm. To make it quick, run this once in a
   terminal. It downloads the package, prints its version and exits:

   ```sh
   npx -y @saihm/mcp-server-pro@0.14.2 --version
   ```

2. In Claude Code, add this repository as a plugin marketplace:

   ```
   /plugin marketplace add SAIHM-Admin/saihm-claude-plugin
   ```

3. Install the plugin:

   ```
   /plugin install saihm@saihm
   ```

4. Restart Claude Code.

You can also run steps 2 and 3 from a terminal with
`claude plugin marketplace add SAIHM-Admin/saihm-claude-plugin` and
`claude plugin install saihm@saihm`.

## Get started

In Claude Code, on a computer you keep, say **"Join SAIHM"**. The free join needs no card:

1. Claude first asks whether you already have a SAIHM identity. Answer no to start a new
   one.
2. SAIHM creates your key on this computer, and Claude shows you a link and a short code.
3. Open the link, sign in to GitHub, enter the code and approve. The join finishes by
   itself.

Then say **"Recall my SAIHM memories."**

**Already have a SAIHM identity?** Don't join. If you're on the free tier and joined on
another computer with GitHub, first check this computer: if `~/.saihm/free-identity.key`
is already there, for example from an accidental join, keep a copy of it. Then copy the
other computer's `~/.saihm/free-identity.key` to the same place on this computer yourself.
Don't paste it into a chat, where Claude would see it. Make the copied file readable only
by you (on macOS and Linux, `chmod 600 ~/.saihm/free-identity.key`). Then restart Claude
Code and say **"Recall my SAIHM memories."** Claude may suggest `export-identity` instead.
With this plugin, copying the key file is the simpler route.

**Signed up at [saihm.net/free](https://saihm.net/free)?** Set up SAIHM with the steps in
the guide's [Start free](https://github.com/SAIHM-Admin/saihm-mcp-server-pro#start-free--one-command)
section instead of this plugin.

**Keep a copy of your key file somewhere safe**, and treat it like a password. SAIHM can't
make you another one, and anyone who has the file can read, change and erase your memories.

## Things to say

You talk to Claude in plain language, and it picks the tools. Some starters:

> Recall my SAIHM memories before you start.

> Remember that I prefer short answers and no preamble.

> What do you remember about my deployment checklist?

> Show my SAIHM status.

> Erase my SAIHM memory about the Henderson account.

## Tools

| Tool | What it does |
|---|---|
| `saihm_remember` | Encrypts on your machine, then stores it |
| `saihm_recall` | Fetches and decrypts on your machine |
| `saihm_forget` | **Permanently erases.** No undo |
| `saihm_status` | Your plan, memory counts and settings |
| `saihm_share` | Grants one memory to one agent, optionally with an expiry |
| `saihm_revoke_share` | Withdraws that grant |
| `saihm_join` | Starts the free join described above |
| `saihm_governance_propose`, `saihm_governance_vote` | Present, but answer "governance unavailable" for now |

Each tool carries labels for hosts that ask before destructive actions, and
`saihm_forget` is the one marked destructive.

**What erasing reaches.** `saihm_forget` makes the memory permanently unreadable in SAIHM
and removes it from this computer's local copy. Another computer that uses your key drops
it at its next recall. Erasing can't reach a conversation that already contains the
memory, including Claude Code's saved session history, a backup of your computer made
while the local copy held it, or what someone you shared it with has already read.

## What this plugin runs, sends and keeps

- It runs `npx -y @saihm/mcp-server-pro@0.14.2`. On the first start, npm downloads that
  package and its dependencies from the npm registry.
- The server connects only to `https://saihm.net`. It sends your encrypted memories there
  with the requests that store, share and erase them, fetches them back when Claude
  recalls them, and carries the steps of a join.
- A join asks you to sign in to GitHub in your own browser.
- The server asks Claude to load your memories before other work in each session, so
  your memories become part of the conversation.
- On your computer, the server keeps your key and its own state in `~/.saihm`. It also
  keeps an unencrypted copy of your memories there, so that later recalls fetch only what
  is new. The copy holds every memory you save on this computer and, after the first
  recall, all of your memories. Only your user account can read it, and `forget` removes a
  memory from it too. This plugin keeps the copy on. If you don't want one, quit every
  Claude Code window and any other app that runs SAIHM with this key, uninstall the plugin
  and delete the `recall.` files in `~/.saihm`. Then set up SAIHM with the guide's steps
  and turn the copy off in each client that uses this key, as the guide's
  [How it works](https://github.com/SAIHM-Admin/saihm-mcp-server-pro#how-it-works) explains.
- The plugin sets `SAIHM_ENDPOINT_URL`. The server also reads other `SAIHM_` settings, and
  a few standard variables such as proxy settings, from the environment Claude Code starts
  in.
- Apart from those settings, it sees only what Claude passes to its tools, and it reads only its own files. It
  doesn't read Claude's own memory, your chat history or your other files.

## Plans

Start free. The free tier is a one-time allowance for trying SAIHM, with no card and
nothing to cancel. Paid plans are monthly, and prices are on the
[pricing page](https://saihm.net/pricing).

This plugin sets up the free tier. A paid plan needs two settings that the plugin
doesn't set, so on a paid plan, set up SAIHM with the client steps in the
[guide](https://github.com/SAIHM-Admin/saihm-mcp-server-pro#add-saihm-to-your-ai-client)
and uninstall the plugin. Upgrading by card keeps the same key and every memory.

## Remove the plugin

To delete your memories from SAIHM too, ask Claude to erase them while the plugin is still
installed. Looking memories up counts against the free allowance, so if yours has run out,
Claude may not be able to find them to erase. In that case, contact SAIHM at
[saihm.net/contact](https://saihm.net/contact).

Then quit every Claude Code window and any other app that runs SAIHM with this key. Run
`claude plugin uninstall saihm@saihm`, and `claude plugin marketplace remove saihm` if you
no longer want this marketplace. Your key and the local copy stay in `~/.saihm`. Keep the
key if you may come back, because it's the only way to read your memories. To remove the
local copy, delete the `recall.` files in that folder.

## Help

- **The tools don't appear:** run the `npx` command from step 1 of Install once. Then open
  `/mcp` in Claude Code and reconnect the `plugin:saihm:saihm` server.
- **Guide and reference:**
  [github.com/SAIHM-Admin/saihm-mcp-server-pro](https://github.com/SAIHM-Admin/saihm-mcp-server-pro)
- **Support:** [saihm.net/contact](https://saihm.net/contact)
- **Security reports:** security@saihm.net
- **Privacy:** [saihm.net/privacy](https://saihm.net/privacy)

## License

Apache-2.0 © SAIHM, Inc. See
[LICENSE](https://github.com/SAIHM-Admin/saihm-claude-plugin/blob/main/LICENSE).
