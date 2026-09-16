# Connecting Your AI Assistant to Aragon Control MCP

This guide walks you through connecting an AI assistant — Claude, ChatGPT,
or another compatible app — to your Aragon Control MCP server, so you can
control your home by typing or speaking to your assistant ("Turn on the
living room lights," "Close the office shutters").

## What you need before you start

- **Aragon Control MCP installed and running** on your Aragon Maestro
  device. If you haven't set that up yet, see the installer instructions
  that came with your software.
- **Your server's address (URL) and your owner password.** You'll find
  both of these in the Aragon Control MCP section of your Aragon Maestro
  app — it shows your server's address and lets you set the password
  you'll use to sign in when an assistant asks to connect. This is not
  your Claude/ChatGPT/etc. account password.

## The one rule that matters everywhere

Wherever a client asks for a **Server URL**, always add `/mcp` to the end
of your address:

```
https://your-device-name.your-tailnet.ts.net/aragon-control/mcp
```

Leaving off `/mcp` is the single most common setup mistake — the
assistant will report a connection or "not found" error that looks like
something is broken, when really the address is just missing its last
piece. Double-check this first if a connection ever fails.

---

## Claude (claude.ai and Claude Desktop)

1. Open **Settings → Connectors → Add custom connector.**
2. **Server URL:** enter your address with `/mcp` on the end, as above.
3. **Authentication:** leave it as detected ("Always required" / "Sign in
   now") — no need to change this.
4. **OAuth client:** choose **"No client ID — register one
   automatically"** (this may also be labeled "Register automatically").
   Do **not** choose "Use Anthropic's hosted client metadata," even
   though it may be marked "Recommended" — that option doesn't work with
   this server.
5. Leave **Additional request headers** empty.
6. Submit. You'll be redirected to an **Aragon Maestro** sign-in page —
   enter your **owner password** there (this is not a Claude account
   login). After signing in, you'll be sent back to Claude and connected.

Once connected, you can talk to your assistant in plain language — in
English or, if your assistant supports it, in another language you
speak, since the assistant translates your request before it reaches
your Maestro system. Ask it to list your rooms, check a light's status,
or turn things on and off.

**If a previously-working connection starts showing "Connection issue" or
"Couldn't connect":** don't just click Reconnect — try removing the
connector and adding it again from scratch if a couple of reconnect
attempts don't fix it.

**Note:** the sign-in page you're redirected to is branded "Aragon
Maestro," while the connector itself is named "Aragon Control MCP" in
your connectors list. That's expected — the sign-in page is about the
home system you're granting access to, not the connector's name.

**If you previously set this connector up in Claude Desktop using a
manually-edited configuration file** (an older, unofficial method): remove
that old entry and fully restart Claude Desktop before adding the
official connector as described above. Running both at once can cause
problems.

## ChatGPT

Custom connectors are available under ChatGPT's **Developer Mode**
connector settings. Use the same server URL (with `/mcp`) and choose
automatic/dynamic OAuth registration, same as above.

**Plan requirement:** on a personal ChatGPT Plus or Pro plan, custom MCP
connectors may be limited to reading status (for example, listing
devices) without being able to change anything — turning a light on or
off may silently not work. Full control (read and write) requires a
ChatGPT Business, Enterprise, or Edu plan. If commands seem to connect
and read fine but don't actually control anything, check your plan
before assuming something is wrong with the setup.

**If creating the connector fails on the first attempt** with an error
like "doesn't support Dynamic Client Registration": delete the connector
and add it again. This has been observed to fail once and then succeed
immediately on a second attempt with no other change — it appears to be
ChatGPT itself not completing its discovery process the first time,
rather than anything wrong with your server or address.

**If a command in your own language doesn't work at first** ("Jag kunde
inte tända lamporna just nu — anslutningen till hemstyrningen
misslyckades" / "couldn't turn on the lights — the connection to the home
system failed"), try the same request once in English, then try your own
language again. This has been observed to fail on the first non-English
command, then work immediately afterward — including for further commands
in your own language — once one English command has gone through. If a
command keeps failing after that, it's worth treating as a real issue
rather than this warm-up effect.

## Perplexity

Perplexity supports custom remote connectors using the same kind of
connection as above (OAuth with automatic client registration).

**Plan requirement:** setting up a *custom* MCP connector in Perplexity
requires a **paid Perplexity account** — it isn't available on the free
tier.

## Mistral Le Chat

At this time, connecting to Mistral Le Chat is **not supported** — adding
the connector fails during setup with a "could not connect to server"
error. This has been confirmed to be an issue on Mistral's side, not with
your Aragon Control MCP server (the server works correctly with other
assistants at the same time). If you use Mistral Le Chat, please check
back after Mistral resolves this, or use one of the supported assistants
above in the meantime.

## Grok

According to xAI's own documentation, Grok's **Free** plan includes
Connectors, and custom MCP connectors can be added by entering a server
URL — so this may not require a paid SuperGrok plan, despite earlier
assumptions. **This has not yet been tested end-to-end against Aragon
Control MCP**, so treat it as unconfirmed rather than a guarantee. If you
try it, the same steps should apply: enter your server URL with `/mcp` on
the end and choose automatic/dynamic OAuth client registration. Please
let us know what happens if you test this, so this section can be
updated with a confirmed result.

## Other assistants

If your assistant supports connecting to custom remote MCP servers with
OAuth 2.0/2.1 and automatic (dynamic) client registration, the same
steps should apply: enter your server URL with `/mcp` on the end, choose
automatic OAuth client registration, and sign in with your Aragon Maestro
owner password when prompted. If you run into trouble with an assistant
not listed here, please get in touch and let us know what happened.
