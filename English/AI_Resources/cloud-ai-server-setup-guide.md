# Setting Up the Cloud AI Server

The Cloud AI Server lets your Aragon Maestro understand voice commands it
does not understand by itself. You can then speak freely, also in another
language than the Maestro's, and get a short spoken answer. The commands
are understood by an AI service on the internet, Scaleway Generative APIs,
which runs in Paris, France. Scaleway bills it to your own account.

Without internet, or while the service does not answer, the Maestro
understands commands by itself as before.

## What you need before you start

- **A Scaleway account.** Private persons can open one; you pay by card
  or SEPA direct debit. Sign up at
  [console.scaleway.com](https://console.scaleway.com).
- **Aragon Control MCP running** on your Maestro. The Cloud AI Server uses
  it to carry out home commands. You find it in the **Aragon Control MCP**
  section of the Maestro page. Remote access for AI assistants is not
  needed for this.
- **Up-to-date Maestro software.**

## Step 1 – Give the Maestro its own access (recommended)

Scaleway can give an *application* its own access that can only use the AI
models. Then the key you enter in the Maestro cannot do anything else in
your Scaleway account.

1. In the Scaleway console, click **IAM & API keys** in the drop-down menu
   at the top right.
2. Open the **Applications** tab and click **Create application**.
3. Enter a name, for example *Aragon Maestro*, and click
   **Create application**.
4. Open the **Policies** tab and click **Create policy**.
5. Enter a name, for example *Aragon Maestro AI*, and select the
   application you just created as the **principal**.
6. Add a rule: select the permission set **GenerativeApisModelAccess**
   (under Generative APIs – Serverless), click **Next**, choose your
   Project as the scope, and finish the policy.

You can skip this step and create the key for your own user instead
(Step 2, "bearer"), but that key then has all the rights your user has.

## Step 2 – Create the API key

1. In **IAM & API keys**, open the **API keys** tab and click
   **+ Generate API key**.
2. **Bearer:** select the application from Step 1 (or yourself).
3. **Description:** for example *Aragon Maestro*.
4. **Expiration:** choose when the key stops working. A key that never
   expires needs no renewal; a key with an end date limits the risk if it
   is ever exposed. If you choose an end date, note it in your calendar.
   Your Scaleway Organization may allow only a maximum duration.
5. **Object Storage:** choose **No, skip for now**.
6. Click **Generate API key**.
7. Copy the **secret key**. It is shown only this once. (The access key,
   the shorter one starting with *SCW*, is not needed.)

## Step 3 – Switch it on in the Maestro

Open the Maestro page and go to the **Cloud AI Server** section.

1. **API key:** paste the secret key and click **Save**. The page then
   shows "A key is set." The key is stored on the Maestro and never shown
   again.
2. **Language spoken at home:** choose the language most people speak to
   the Maestro and click **Save**. Short commands are then understood much
   better; other languages still work.
3. Switch on **Use the cloud AI server**. After a moment the status shows
   **Ready**.
4. Optional: **Always use the cloud AI server.** Turn this on when the
   household speaks another language than the Maestro. Every voice command
   then goes to the AI service, also those the Maestro would have
   understood itself; otherwise the Maestro may mistake foreign words for a
   command. The voice application restarts when you change this.

Try it: say a command in your own words, for example "make it a bit
brighter here" or "close the shutters in the living room".

## What it costs

Scaleway bills by use. Prices in October 2026, before tax:

- Speech recognition (*whisper-large-v3*): €0.003 per minute of audio.
- Understanding the command (*qwen3-235b-a22b-instruct-2507*): €0.75 per
  million input tokens and €2.25 per million output tokens.
- Free tier: the first 1,000,000 tokens and 60 minutes of audio are free.

One voice command uses about 5,000 tokens and a few seconds of audio,
which comes to roughly €0.004. Only the commands the Maestro does not
understand by itself go to the service, so at 50 voice commands a day it
costs about €1 a month (our estimate). With **Always use the cloud AI
server** on, every command counts.

Scaleway cannot stop the service at a spending limit, but you can set
**billing alerts** in the Scaleway console to be warned by e-mail. Current
prices: [scaleway.com/en/pricing/model-as-a-service](https://www.scaleway.com/en/pricing/model-as-a-service/).

## What is sent to Scaleway

For each command that goes to the AI service: the recording of what you
said, its text, and the names of your rooms, devices and scenes, so that
the AI service can choose the right one. Questions about the weather, a
calculation or similar also send the result of that look-up. The service
runs in Paris, France.

## When the key stops working

When the key has expired or was deleted, the status on the Maestro page
says: "On, but the AI service refuses the API key. It may have expired."
The Maestro then handles commands by itself.

1. Create a new key as in Step 2.
2. Paste it in the **API key** field and click **Save**. The key is
   checked at once.
3. In the Scaleway console, delete the old key (**API keys** tab, menu
   next to the key, **Delete**).

## Status messages

| Status | What to do |
|---|---|
| Ready | Nothing. |
| Off | Switch on **Use the cloud AI server**. |
| On, but no API key is set | Enter the key (Step 3). |
| On, but the AI service refuses the API key | Create a new key (see above). |
| On, but Aragon Control MCP is not running | Start it in the **Aragon Control MCP** section. |
| On, but the AI service could not be reached just now | Check the internet connection. The Maestro works by itself meanwhile and tries again later. |
| Not responding | Restart the Maestro. If it stays, contact your installer. |
