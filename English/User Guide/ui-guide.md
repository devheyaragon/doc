# Aragon Maestro: User Interface Guide

This guide explains the Maestro's web page: what each menu item and section
does and where to find it. It is written for you and for AI assistants that
answer questions about Aragon. The names in **bold** are the words shown on
the page in English; the page also exists in German and French (menu
**LANGUAGE**).

It describes the page of Aragon Maestro 2.6.4. It is updated when the page
changes.

## Opening the page

| Where you are | What to open |
| ------------- | ------------------------------ |
| At home | Type `find.heyaragon.com` in the browser and enter your network (for example `192.168.1.`). The search lists the Maestro; click it. You can also type the Maestro's IP address directly. |
| Away from home | With **Remote Connection** set up (menu **SYSTEM**), open the Maestro's Tailscale address. |

The page has a menu bar at the top and one long page below it. Menu items
either jump to a section of the page, open a dialog, or open a separate page
in a new tab.

## The menu bar

### VOICE CONTROL

| Item | What it does |
| ------------- | ------------------------------ |
| **See what's heard** | Opens a page that shows what Aragon heard and how it understood it. Useful when a command is not carried out. |
| **Voice Commands Guide** | Jumps to the section **USE**: example commands for lights, blinds, scenes, music and Super Scenes, with short videos. |
| **AI Assistant (Q&A)** | Jumps to the questions, tasks and reminders you can ask the on-board AI assistant. |
| **Train new words** | Teaches Aragon the names of your rooms, devices and scenes. Run it after you add or rename a room or device in the gateway. It takes a minute or two. You can include SONOS favorites. |
| **Troubleshoot** | Jumps to the section **TROUBLESHOOT**. |
| **Restart Voice App** | Restarts the voice application without restarting the Maestro. The first thing to try when voice control stops reacting. |
| **Set rapid or thoughtful** | Sets how long Aragon waits for you to finish speaking (**Thoughtfulness**). Suggested: experienced 0-1, average 1-3, beginner 2-4. |
| **Fine-tune Recognition** | Sets the **Confidence Threshold** of the on-board speech recognition (0.8-0.85 recommended; lower is faster, higher is more accurate). Not used while **Always use cloud AI** is on. |

### AUTOMATION

| Item | What it does |
| ------------- | ------------------------------ |
| **Super Scenes** | Jumps to the section **SUPER SCENES** (Good Morning, Good Night, I'm home, I'm leaving). |
| **Home Automation Rules** | Jumps to the section **HOME AUTOMATION RULES**: timed rules written in plain language. |
| **Presence Monitoring** | Opens the presence monitoring page in a new tab. |
| **Sound Event Charts** | Opens the charts of the sounds the satellites detected (Node-RED dashboard). |

### SETUP

| Item | What it does |
| ------------- | ------------------------------ |
| **Gateway Configuration** | Opens the dialog for your smart home gateway: its IP address (**Discovered Gateways** lists the ones found), user name and password where needed, and the connection **Status**. After a change, allow up to two minutes. |
| **Satellites** | Jumps to the section **SATELLITES**. |
| **Select Voice** | Opens the dialog to choose the Maestro's voice. **Say it !** plays a test. Option: send spoken answers to the SONOS speaker in the same room. |

### NODE-RED

| Item | What it does |
| ------------- | ------------------------------ |
| **Open flow editor** | Opens the Node-RED editor in a new tab. |
| **Open Dashboard** | Opens the Node-RED dashboard. |
| **Node-RED library GitHub** | Opens Aragon's library of Node-RED examples. |
| **Reset Node-RED** | Restarts Node-RED (**Restart**) or installs it again (**HARD reset**). A hard reset can also erase all settings except the gateway. |
| **Set Node-RED password** | Sets the Node-RED password back to the factory default. |

### SYSTEM

| Item | What it does |
| ------------- | ------------------------------ |
| **About & Updates** | Opens the page with the software versions and updates (see below). |
| **Remote Connection** | Jumps to the section **REMOTE CONNECTION**. |
| **Aragon Control MCP** | Jumps to the section **ARAGON CONTROL MCP**. |
| **Support access** | Jumps to the section **SUPPORT ACCESS**. |
| **Cloud AI** | Jumps to the section **CLOUD AI**. |
| **Aragon AI Chat** | Opens the Aragon chat in a new tab. With cloud AI on, the chat can also switch devices and, when turned on under **MCP Servers**, check the Maestro. |
| **Talk to Home** | Opens a page where you type or speak a command (voice input works in Chrome, Safari and Samsung Internet). It shows how the command was understood. |
| **Vikunja To-Do App** | Opens the to-do and reminder app. |
| **Netdata Dashboard** | Opens live charts of the Maestro's processor, memory and network. |
| **Logfiles** | Shows the Maestro's log files. |
| **Reboot Aragon** | Restarts the Maestro after you confirm. It takes a few minutes until Aragon is ready again. |
| **Power-off** | Shuts the Maestro down after you confirm. Wait until the red power LED (PWR) is completely off (up to 3 minutes) before you unplug it. |

### HELP and LANGUAGE

| Item | What it does |
| ------------- | ------------------------------ |
| **Help and Documentation** | Opens Aragon's guides and videos, stored on the Maestro. |
| **HeyAragon.com** | Opens Aragon's website. |
| **find.HeyAragon.com** | Finds Maestros in your network. |
| **LANGUAGE** | English, Français or Deutsch: the language of the page. If it differs from the voice assistant's language, the page shows a warning, because some features then do not work properly. |

## The page, from top to bottom

| Section | What you find there |
| ------------- | ------------------------------ |
| Left column | **About** (privacy), **Technology** (one Maestro and 1-10 satellites, WL or PoE, all in the same network). |
| **START** | Reminder: give your rooms and devices proper names in the gateway, so they can be controlled by voice. |
| **MAESTRO** | How to install the Maestro, then **Configuration of Gateway**: the **Gateway Configuration** button and the connection state, **Select Voice**, and **Train new words** for new device or room names. |
| **SATELLITES** | Setting up satellites (see below). |
| **USE** | How to speak to Aragon (wake word, a short pause, then the command) and the list of voice commands, with videos; below it the AI assistant for questions, tasks and reminders. |
| **TROUBLESHOOT** | **Restart Voice App**, **See what's heard**, **Set rapid or thoughtful**, **Fine-tune Recognition**, and **Download** for the log files. |
| **HOME AUTOMATION RULES** | **Manage Rules** and **Location Settings** (see below). |
| **SUPER SCENES** | The four Super Scenes and **Reset Super Scenes** (see below). |
| **REMOTE CONNECTION** | **Enable Remote Connection** / **Disable Remote Connection** (Tailscale; sign up for a free Tailscale account first), and other subnets to reach from away (CIDR, for example `192.168.9.0/24`) with **Save**. |
| **ARAGON CONTROL MCP** | Lets an AI assistant such as Claude or ChatGPT control your home. The connection address with **Copy**, the service with **Restart Service**, **Start Service** or **Stop Service**, the **Setup Guide**, the **Owner password** (what you type on the Maestro's sign-in page, not your Claude or ChatGPT password) and **Republish Address**. Needs **Remote Connection**. |
| **SUPPORT ACCESS** | Lets an AI assistant check your Maestro for problems; it can only read. **Turn on for 48 hours** (with the owner password), **Turn off now**, the connector address with **Copy**, the last access, the line for the Aragon chat, and the **User guide**. While it is on, a banner at the top of every page says so. |
| **CLOUD AI** | An AI service on the internet (Scaleway, Paris) that understands and answers voice commands: **Use cloud AI**, **Always use cloud AI**, the **Setup Guide**, the **API key** of your Scaleway account, and **Language and voice**: **Language spoken at home**, **Other languages are also spoken at home**, **Speak in the language spoken at home**, and the **Voice of the language spoken at home** (**Install**, **Use**, **Remove**, **Test voice**). |
| **ONLINE VOICE** | A natural voice from KugelAudio, paid by use: **Use the online voice (KugelAudio)**, the **KugelAudio API key**, **Voice per language** (**Use**, **Default**) and **Test**. Everything the Maestro says is then sent to KugelAudio as text. |
| **WEB SEARCH** | Internet searches through Mojeek, paid by use with your own key: **Use web search (Mojeek)** and the **Mojeek API key**. |
| Footer | Open source licences and contact. |

## Dialogs and pages in detail

### Satellites

In the section **SATELLITES**:

1. Connect the satellite (PoE cable, or USB power and Ethernet for the WL
   model). After about a minute it plays a sound.
2. Click **Refresh Satellites**. The list below shows the satellites found,
   by IP address.
3. Select a satellite and click **Show Details**. The dialog **Satellite
   Settings** opens.
4. **Sound Events**: choose the rooms where sounds are detected. It needs at
   least one satellite whose **Wakeword system** is set to Centralized.

In **Satellite Settings**:

| Setting or button | What it does |
| ------------- | ------------------------------ |
| **Room** | The room the satellite is in. |
| **Volume** | Loudness of the satellite. |
| **Wakeword** | The word that wakes the satellite. Choose one whose language matches the satellite's language. |
| **Wakeword system**, **Wakeword pronouciation**, **Sensitivity:** | How the wake word is detected. |
| **Single Device Response** | Only the first satellite that heard you answers. |
| **Play sound when activated** | A beep when the satellite wakes up: wait for it before speaking. |
| **No Visual Feedback** | No blue light. |
| **Use DeepFilterNet noise suppression** | Better understanding in noisy rooms. |
| **Wi-Fi** | Connects the satellite to Wi-Fi (2.4 GHz only). |
| **Open App**, **Restart App**, **Reboot**, **PowerOff** | Open the satellite's own page, restart its app, restart it, or switch it off (to start it again, unplug and plug in the cable). |
| **Upgrade** | Updates the satellite's software (several minutes, do not interrupt). |
| **Download Wakewords** | Loads the available wake words. |
| **Test Audio**, **Play a test sound** | Records 5 seconds and plays them back, or plays a test sound. |

### Super Scenes

The buttons **Good Morning**, **Good Night**, **I'm home** and **I'm leaving**
each open a dialog. For each, enter up to three voice commands that run when
you say the Super Scene, and if needed a condition (**When**, for example
daytime or nighttime). **Restore Default** brings back the original commands
of one scene; **Reset Super Scenes** resets all four.

### Home Automation Rules

**Manage Rules** opens the list of rules. **Add New Rule**, type the rule in
plain language (for example "Close all shutters at 10 PM on weekends"),
click **Validate**, then **Save**. Anything you can control by voice can be
a rule; rules repeat, they are not one-time actions. For rules at sunrise or
sunset, set your nearest major city under **Location Settings** →
**Change**. Write rules in the voice assistant's language.

### About & Updates

The page opened from **SYSTEM** → **About & Updates**:

| Part | What it does |
| ------------- | ------------------------------ |
| **1. Aragon is up-to-date** / **One or more updates are available** | The Aragon update, with **Update**. Do not restart or switch off the Maestro during an update. |
| **2. Voice Application (Action & Logic)** | Installs the voice application (**Install**), for example when a new version is shown. |
| **3. Voice Assistant (Natural Language Understanding)** | Installs the voice assistant (**Install**), for example to change its language when the page warns of a language mismatch. |
| **4. Change Gateway (Advanced)** | Switches to another type of gateway. |
| **5. Forget Gateway (Advanced)** | Clears all gateway settings (IP, user names, passwords). |
| **Show/Hide System update** | **6. System update**: updates all system files. Only when you have problems. |
| **Show/Hide Piper/TTS Update** | Updates the speech output files (large download). |
| **Show/Hide System time** | Time zone, network time synchronisation, date and time. |
| **ZRAM Settings** | Compressed memory used as swap. |
| **Show/Hide Licences** | The open source software used in Aragon. |
| **BACK** | Back to the main page. |

## How do I...?

| Task | Where |
| ------------- | ------------------------------ |
| Restart the Maestro | **SYSTEM** → **Reboot Aragon**, then confirm. If only voice control is stuck, first try **VOICE CONTROL** → **Restart Voice App**. |
| Switch the Maestro off | **SYSTEM** → **Power-off**; unplug only when the red PWR LED is off. |
| Change a satellite's wake word | **SATELLITES**: **Refresh Satellites**, select the satellite, **Show Details**, **Wakeword**. |
| Use a new room or device by voice | Name it in the gateway, then **VOICE CONTROL** → **Train new words**. |
| See why a command was not understood | **VOICE CONTROL** → **See what's heard**. |
| Change the Maestro's voice | **SETUP** → **Select Voice**. |
| Update Aragon | **SYSTEM** → **About & Updates**. |
| Get the log files | **TROUBLESHOOT** → **Download**, or **SYSTEM** → **Logfiles**. |
| Reach the Maestro from away | **SYSTEM** → **Remote Connection**. |
| Control the home from Claude or ChatGPT | **SYSTEM** → **Aragon Control MCP**, and its **Setup Guide**. |
| Let support or an AI assistant check the Maestro | **SYSTEM** → **Support access** → **Turn on for 48 hours**. |
| Use an AI service for voice commands | **SYSTEM** → **Cloud AI**, and its **Setup Guide**. |
| Chat with Aragon | **SYSTEM** → **Aragon AI Chat**. |
| Change the page language | **LANGUAGE**. |
| Find the guides | **HELP** → **Help and Documentation**, or doc.heyaragon.com. |
