# Aragon System MCP: Letting an AI Assistant Check Your Maestro

The Aragon System MCP lets an AI assistant such as Claude look inside your
Aragon Maestro and find out why something does not work: a satellite that
does not react, voice commands that do nothing, a slow system after an
update. You describe the problem in your own words; the assistant checks the
Maestro itself and explains what it finds.

It only reads. It never changes a setting, restarts anything or switches a
device. It is off until you switch it on, and it switches itself off after
48 hours.

## What it can check

| Area | What the assistant can see |
| ----------- | ------------------------------------------------------------ |
| Health | The Maestro's full health check: packages, services, ports, voice models, the AI assistant, Cloud AI, disk and memory |
| Errors | Warnings and errors in the logs of each Aragon service, with each time they happened |
| Changes | Language switches, voice training, Cloud AI settings saved, reboots (and what started them), sign-ins, the last update and what it changed |
| Performance | CPU, memory, swap and temperature over the last hours, and which program caused a peak |
| Speech | How many voice commands went to Cloud AI, how fast the answers came, retries (counts only) |
| KNX gateway | Gateway type and address, whether the connection works, the devices and rooms Aragon loaded from it |
| Satellites | Each satellite's room, wake word, firmware and network connection; satellites the Maestro cannot find |
| Guides | Aragon's own guides and videos, so the answer can point you to the right page |

## What is shared, and what never is

The assistant sees system information: states, versions, settings without
their secret values, and log lines. Through Claude this information goes to
Anthropic; through another assistant, to that assistant's company; in the
Aragon chat, to the cloud AI service.

Never shared:

- **Passwords and keys.** They are removed from everything the connector
  returns, including the user name and password of your KNX gateway.
- **What was said at home.** Speech is reported as counts and times only,
  never as text.
- **Recordings.** The connector has no access to audio.

## What you need before you start

- **Remote Connect set up** on your Maestro, with an owner password. This is
  the same password you use for Aragon Control MCP. If you have not set it
  up yet, see [Connecting Your AI Assistant to Aragon Control
  MCP](mcp-client-setup-guide.md).
- **An AI assistant that supports custom connectors**, for example Claude
  (claude.ai or Claude Desktop) or ChatGPT with Developer mode on.

## Step 1: Switch it on

1. Open the Maestro page and choose **System → Support access** in the menu
   (the section sits under Remote Connection).
2. Click **Turn on for 48 hours** and enter your owner password.
3. The card shows until when it is on, and your connector address with a
   copy button.

You can click **Turn off now** at any time. The assistant then loses access
at once. Turning it on again starts a new 48-hour window and signs out any
assistant that was connected before: it has to sign in again. Changing the
owner password in the Aragon Control MCP section does the same.

## Step 2: Connect your assistant

Your connector address ends in `/aragon-system/mcp`:

```
https://your-device-name.your-tailnet.ts.net/aragon-system/mcp
```

Copy it from the Support access card. Leaving off `/mcp` is the most common
setup mistake.

**Claude (claude.ai and Claude Desktop)**

1. Open **Settings → Connectors → Add custom connector** and give it a name,
   for example "Aragon System".
2. **Server URL:** your address, ending in `/mcp`.
3. **OAuth client:** choose **"No client ID — register one automatically"**.
   Do not choose "Use Anthropic's hosted client metadata".
4. Leave **Additional request headers** empty and submit.
5. On the **Aragon Maestro** sign-in page, enter your **owner password**
   (not your Claude password). You are sent back to Claude, connected.

**ChatGPT:** turn on **Developer mode** in **Settings → Security and login**,
then add a connector with the same address and automatic OAuth registration.

Aragon Control MCP and the System MCP are two separate connectors with two
addresses. You can have both: one controls your home, the other checks it.

## Step 3: Ask your question

Start a new chat, make sure the connector is turned on in that chat, and
describe the problem as you would to a person. For example:

- "My Maestro no longer reacts to voice commands. Find out why and tell me
  what you checked."
- "The satellite in the living room doesn't react. What is wrong?"
- "Voice commands don't switch any lights. Is my KNX gateway connected?"
- "Since the last update something is broken. What changed?"
- "Was anything unusual on my Maestro yesterday between 18:00 and 22:00?"
- "How do I change the wake word of a satellite?"

The assistant chooses which checks to run. Good answers say what was
checked, what is normal and what is not, and link to the right Aragon guide.

**Seeing each check before it runs:** because the checks only read, Claude
may run them without asking. To approve each one, open the connector's
settings in Claude and set the tools you want to see, for example
**Recent errors** and **Support report**, to **Needs approval**.

## In the Aragon chat (with Cloud AI)

The same checks also work in the Maestro's own chat page, without Claude,
when Cloud AI is on. This does not depend on the Support access switch and
does not need Remote Connect.

1. Open the chat from the Maestro menu, then open **MCP Servers**.
2. Turn on **Aragon System** and start a **New chat**.
3. Ask your question as in Step 3.

While it is on, each message to the cloud AI costs a little more, because the
descriptions of the checks go with every message. To stop, turn it off and
start a new chat; opening the chat from the Maestro menu turns it off again.
While the checks are offered, the chat cannot open web pages.

## Sending a report to Aragon support

Ask the assistant: *"Make a support report and give it to me as text to send
to Aragon support."* The report holds the same facts the assistant saw, with
the date and time, so support sees exactly what was checked. Make a second
report after a fix to show what changed.

## If it does not connect

- **"Couldn't connect" or "service unavailable":** Support access is off, or
  its 48 hours have ended. Turn it on again on the Maestro page and
  reconnect.
- **The connection worked before and now asks you to sign in again:** Support
  access was turned on again, which signs out earlier connections. Sign in
  with your owner password.
- **Reconnect does not help:** remove the connector in Claude and add it
  again.
- **The card says "Remote Connect is not connected":** the Maestro is not
  connected to Tailscale right now. Support access works again by itself as
  soon as Remote Connect is connected; there is no need to turn it off and on.
- **The address does not answer at all:** Remote Connect must be set up and
  reachable. Check the Remote Connection status on the Maestro page.

## Limits

- The assistant explains; it does not repair. You or Aragon support make the
  change.
- It cannot check a Maestro that is switched off or has no internet
  connection.
- It sees the KNX gateway, the satellites and the Node-RED flow from the
  Maestro's side: whether they answer and what they report, not their own
  internal settings.
