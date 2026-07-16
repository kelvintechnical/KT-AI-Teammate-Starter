# Build Your Project AI Teammate
**Hands for Humanity / Kinston Teens AIM 2026**

Welcome. Today you are going to do something most people only *talk* about:

1. Connect to a real AI model (Google Gemini)
2. Give it a job on your production team
3. Have a real conversation with it
4. Chain two AI calls together — the start of an **agent**

No coding experience needed. Work through the steps in order. Each one builds on the last. If something looks confusing, keep going — the “what does this mean?” notes under each code block are for you.

> **Class slideshow (follow along with Kelvin):**  
> [TA_KT_AI — Google Slides](https://docs.google.com/presentation/d/1wPBxUh-1op4w7B6bKpgvAgZduuZcQ0PKv5erRqPEjAM/edit?slide=id.g3f53ebb41e6_11_1192#slide=id.g3f53ebb41e6_11_1192)

> **Model for today:** use `gemini-3.1-flash-lite` everywhere below.  
> If you see a **404** on an older model name, switch to this one.  
> If you see a **503** (high demand), wait 30 seconds and Run again — or ask your instructor.

---

## Tiny Python cheat sheet (read once)

You don't need to memorize this. Come back when something breaks.

| You'll see… | What it is | Why it matters | Say it like this |
|---|---|---|---|
| `=` | **Stores** a value in a named box (variable). | Without `=`, Python can't remember things for later. | "Put this in a box labeled `client`." |
| `==` | **Compares** two values — asks "are these the same?" | Easy to mix up with `=`. Wrong one = weird bugs. | "Are these equal? Yes or no." |
| `"text"` | A **string** — text in quotes. | The AI reads and writes strings. Quotes tell Python it's words, not code. | "This is a sentence, not a command." |
| `something.part` | **Dot** means "look inside." | Lets you open nested tools: `client.chats.create` | "Open the chats folder inside client." |
| Indentation | Leading spaces = "this line belongs inside the block above." | Python has no `{ }`. Wrong spaces = crash. | "These lines live inside the loop." |
| `.text` | Pulls the **plain-text answer** out of a response. | Without it you see a messy package, not a sentence. | "Unwrap the actual words the bot said." |

---

## Step 1 — You’re in the right place

You imported this starter into Replit from GitHub. You should see:

- **Left** → `main.py` (your code — starts nearly empty on purpose)
- **Right** → this README (your instructions)

> **Before you write any code**, skim Steps 2–4 so you know what’s coming. Then come back and follow them carefully.

---

## Step 2 — Get your free Gemini API key

An **API key** is like a password that proves *you* are allowed to use Google’s AI. It is **free** and needs **no credit card**.

1. Go to **[https://ai.google.dev/aistudio](https://ai.google.dev/aistudio)**
2. Sign in with a Google account
3. Click **"Get API key"**
4. Click **"Create API key"**
5. Copy the key — a long string of letters and numbers

> ⚠️ **Keep your key private.** Do not share it in chat, post it online, or paste it into `main.py`.

---

## Step 3 — Store the key safely in Replit Secrets

Secrets let your code use a private value without ever showing it on screen.

1. Left sidebar → click **Secrets** (under Setup / Tools)
2. Click **"New Secret"** at the top — **not** "New configuration"
3. **Key name** — type exactly: `GEMINI_API_KEY`  
   *(spelling and capitalization must match)*
4. **Value** — paste your API key
5. Click **"Add Secret"**

> ✅ Your key is safe. Your code will read it from behind the scenes. Never paste it into `main.py`.
>
> ⚠️ If you add it under **Configurations** instead of **Secrets**, the bot will not find your key. Cancel that and use **+ New Secret**.

---

## Step 4 — Install the library and connect

### First: install the toolkit (Shell)

Click the **Shell** tab (bottom panel — or Tools → Shell) and run:

```
pip install google-genai
```

Wait until the cursor comes back. That command downloads Google’s Gemini toolkit so Python can use it.

> If Shell says **Requirement already satisfied**, you’re good — move on.

### Then: open `main.py` and paste this

```python
from google import genai
import os

client = genai.Client(api_key=os.environ["GEMINI_API_KEY"])
```

Click **Run**. No error message? You’re connected. Go to Step 5.

> ⚠️ A red underline under `google` that says **Cannot resolve imported module** is often just the editor. Trust **Run** / **Console**, not the squiggle. If Run works, keep going.

<details>
<summary>🔍 What does this code mean? (click to open)</summary>

Each line uses four questions: **What is it?** **What is it for?** **Why does it matter?** **Say it like this.**

**Line 1 — `from google import genai`**
- **What:** An import line — loads a tool from Google's package.
- **For:** Gives your program access to Gemini.
- **Why:** Python doesn't know about Gemini until you import it.
- **Like:** "Bring over the Gemini tray from Google's toolbox."

**Line 2 — `import os`**
- **What:** Imports Python's operating-system helper.
- **For:** Reads environment variables — including your Secret.
- **Why:** Your API key lives in Secrets, not in your code file.
- **Like:** "Borrow the helper that can read hidden keys."

**Line 4 — `client = genai.Client(api_key=os.environ["GEMINI_API_KEY"])`**
- **What:** Creates a connection object and saves it as `client`.
- **For:** Opens your authenticated line to Google's AI.
- **Why:** Every future message goes through this connection.
- **Like:** "Save your phone line to Gemini in a box named `client`."

**Parts inside Line 4:**
- `os.environ["GEMINI_API_KEY"]` — looks up the Secret by exact name. Misspell it → `KeyError`.
- `genai.Client(...)` — hands Google your key so it knows you're allowed in.
- `api_key=...` — named setting: "use this key for this connection."

</details>

---

## Step 5 — Send your first message

Add these lines **below** what you already have:

```python
chat = client.chats.create(model="gemini-3.1-flash-lite")
print(chat.send_message("Hello! Who are you?").text)
```

Click **Run**. Look in the **Console** (not Shell) for the answer.

> ✅ **Checkpoint:** If the bot answers with a sentence about itself — you just talked to a large language model (LLM). That’s the same kind of tech behind ChatGPT, Claude, and Gemini… except *you* built the connection.

<details>
<summary>🔍 What does this code mean?</summary>

**`chat = client.chats.create(model="gemini-3.1-flash-lite")`**
- **What:** Opens one conversation with a chosen AI model.
- **For:** Saves that conversation as `chat` so later messages can remember earlier ones.
- **Like:** "Open a group chat and save it as chat."

**`print(chat.send_message("Hello! Who are you?").text)`**
- **What:** Sends text, unwraps the answer, shows it.
- **For:** Your first real talk with the model.
- **Like:** "Ask, peel off `.text`, print the sentence."

**Read inside → out:** `send_message()` → `.text` → `print()`

</details>

---

## Step 6 — Give your teammate a job

A chatbot with no job will ramble. A chatbot with a **job description** becomes a teammate.

You’re going to set a `system_instruction` — rules the AI follows in the background for the whole conversation. The user never types this. *You* decide the role.

**Delete** the two lines from Step 5. Then **pick ONE role** below and paste that block into `main.py` (keep your Step 4 connection code at the top).

> **Reminder:** AIM is the **Kinston Teens summer program** working with Hands for Humanity.  
> Your portfolio is a **communications & marketing** portfolio (flyers, writing, photo/video, docs) — **not** an AI/ML engineering project.  
> Do **not** invent what “AIM” stands for.

---

### 🎨 Graphic Design

```python
from google.genai import types

chat = client.chats.create(
    model="gemini-3.1-flash-lite",
    config=types.GenerateContentConfig(
        system_instruction=(
            "You are a graphic design helper for the Hands for Humanity AIM portfolio. "
            "AIM is the Kinston Teens summer program — a communications and marketing portfolio, "
            "not an AI/ML project. Never invent what AIM stands for. "
            "Help with logos, donation/recruitment/event graphics, and social visuals. "
            "If they do not say what they are making or who it is for, ask ONE short question first. "
            "Default tool: Google Gemini image generation. "
            "Workflow: (1) give 2–3 design ideas as a numbered list with one reason each, "
            "(2) refine the idea they pick, "
            "(3) give ONE full Gemini image prompt they can copy and paste "
            "(style, colors with hex codes, layout, and 'no text' or the exact words), "
            "(4) tell them to open Gemini, paste, generate, and download. "
            "Do not push Canva first — only mention Canva after they have the image. "
            "Never say you can see images. "
            "Do not invent org facts — ask if something is missing."
        )
    )
)
```

---

### ✍️ Content Writing *(default example)*

```python
from google.genai import types

chat = client.chats.create(
    model="gemini-3.1-flash-lite",
    config=types.GenerateContentConfig(
        system_instruction=(
            "You are a writing coach for the Hands for Humanity AIM portfolio. "
            "AIM is the Kinston Teens summer program — a communications and marketing portfolio, "
            "not an AI/ML project. Never invent what AIM stands for. "
            "Help with the mission statement, short histories, and captions. "
            "Use a clear teen voice. "
            "If they have no facts or length yet, ask ONE short question first. "
            "Give one draft, then one short note on why it works. "
            "Do not invent facts, quotes, or names — ask if something is missing."
        )
    )
)
```

---

### 📸 Photo & Video

```python
from google.genai import types

chat = client.chats.create(
    model="gemini-3.1-flash-lite",
    config=types.GenerateContentConfig(
        system_instruction=(
            "You are a photo and video helper for the Hands for Humanity AIM portfolio. "
            "AIM is the Kinston Teens summer program — a communications and marketing portfolio, "
            "not an AI/ML project. Never invent what AIM stands for. "
            "Help with B-roll shot lists, facility photo checklists, "
            "and a 2-minute documentary outline (beginning, middle, end). "
            "Use numbered lists. "
            "If they ask about consent, give a checklist first: ask before filming faces, "
            "guardian OK for minors, say how the video will be used, and people can say no. "
            "If they do not say what or where they are filming, ask ONE short question first. "
            "Do not invent places or people — ask if something is missing."
        )
    )
)
```

---

### 📋 Project Management

```python
from google.genai import types

chat = client.chats.create(
    model="gemini-3.1-flash-lite",
    config=types.GenerateContentConfig(
        system_instruction=(
            "You are a project management helper for the Hands for Humanity AIM portfolio. "
            "AIM is the Kinston Teens summer program — a communications and marketing portfolio, "
            "not an AI/ML project. Never invent what AIM stands for. "
            "Help with timelines, digital folder structures, and requirements checklists "
            "for teen production teams. "
            "Keep answers as bullet lists. End with the single most important next action. "
            "If they do not say a deadline or deliverable, ask ONE short question first. "
            "Do not invent org facts — ask if something is missing."
        )
    )
)
```

---

> ✅ After pasting your role, click **Run**. No output yet is fine — you haven’t started the conversation loop. Move to Step 7.

<details>
<summary>🔍 What does this code mean?</summary>

**`from google.genai import types`**  
Extra helper pieces for building settings (like form fields Google expects).

**The nested part looks scary. Read it like Russian nesting dolls:**

1. Outer doll: `client.chats.create(...)` — start a chat  
2. Middle doll: `config=types.GenerateContentConfig(...)` — a settings bundle  
3. Inner doll: `system_instruction=(...)` — the job description text  

**`system_instruction` is not a user message.**  
It’s the AI’s role for the whole thread: “You are a writing coach…” / “You are a project management helper…”  
Change that string → change how the bot behaves. Same model, different teammate.

**Why the parentheses around the long string?**  
Python lets you wrap a string in `( )` so you can break it across lines cleanly. Makes long instructions readable.

</details>

---

## Step 7 — Make it remember (chat loop)

Add these lines **at the bottom** of `main.py`:

```python
while True:
    msg = input("You: ")
    if msg == "quit":
        break
    print("Bot:", chat.send_message(msg).text)
```

Click **Run**. The console shows `You:` — type something and press Enter.

Try **2–3 prompts** about your real team work. When you’re done, type `quit` and press Enter.

**Strong prompt tip:** say *who it’s for* + *how you want it back* (list, short, etc.).

### Try these (pick your team)

**Graphic Design**
- Give me 3 logo ideas for Hands for Humanity. Number them. One short reason for each.
- Write one full Gemini image prompt for a Hands for Humanity logo I can copy and paste.

**Content Writing**
- Write a short mission statement for Hands for Humanity in about 100 words. If you need facts, ask me one question first.
- Write 3 Instagram captions for our launch. Each under 150 characters. Friendly, not cheesy.

**Photo & Video**
- Give me a B-roll shot list for our facility tour — 8 shots, numbered, one line each.
- Before we film people, give me a short consent checklist.

**Project Management**
- Give me a simple folder structure for our Hands for Humanity project files.
- Make a two-week timeline with milestones for graphics, writing, and video. End with one next action.

> 📝 **Log what you asked and what the bot answered.** Create a notes file in Replit (Library → ⋮ → New file → e.g. `prompt_log.txt`) and keep your prompts + responses there for portfolio AI documentation.

<details>
<summary>🔍 What does this code mean?</summary>

**`while True:`**  
“Keep repeating forever…” — until something inside stops it.

**`msg = input("You: ")`**  
Pause. Show `You:`. Wait for you to type + hit Enter. Store what you typed in `msg`.

**`if msg == "quit":`**  
`==` means **compare** (are they equal?).  
`=` means **store**. Mixing these up is the #1 beginner bug.

**`break`**  
Leave the loop right now. Program continues after the loop (or ends).

**`print("Bot:", chat.send_message(msg).text)`**  
Send your message into the **same** `chat` (so it remembers earlier turns), then print the reply.

**Indentation matters.**  
Everything indented under `while True:` is *inside* the loop. Python uses spaces instead of `{ }` braces. Copy carefully.

</details>

---

## Step 8 — Checkpoint

Pause. Check yourself:

- ✅ Connected to a real AI model (`gemini-3.1-flash-lite`)
- ✅ Gave your bot a specific job with `system_instruction`
- ✅ The bot remembers the conversation (`chat` keeps history)
- ✅ Had a multi-turn chat and logged it

### Full file check (Writing default)

Your `main.py` should match this (Connect + Writing job + chat loop).  
Other roles: same file, but swap the Step 6 block from your team section above.

```python
from google import genai
from google.genai import types
import os

client = genai.Client(api_key=os.environ["GEMINI_API_KEY"])

chat = client.chats.create(
    model="gemini-3.1-flash-lite",
    config=types.GenerateContentConfig(
        system_instruction=(
            "You are a writing coach for the Hands for Humanity AIM portfolio. "
            "AIM is the Kinston Teens summer program — a communications and marketing portfolio, "
            "not an AI/ML project. Never invent what AIM stands for. "
            "Help with the mission statement, short histories, and captions. "
            "Use a clear teen voice. "
            "If they have no facts or length yet, ask ONE short question first. "
            "Give one draft, then one short note on why it works. "
            "Do not invent facts, quotes, or names — ask if something is missing."
        )
    )
)

while True:
    msg = input("You: ")
    if msg == "quit":
        break
    print("Bot:", chat.send_message(msg).text)
```

All four true? You’re ready for Step 9.

---

## Step 9 — First step toward an agent

An **agent** (simple version) is a program where **one AI’s output becomes the next AI’s input** — like a mini production pipeline.

**After** the `while` loop (below the indented `break` block), add this. Swap the `question` for something your team actually needs:

```python
question = "Write a 100-word mission statement for Hands for Humanity. If you need facts, ask one question first."
answer = chat.send_message(question).text

review = client.models.generate_content(
    model="gemini-3.1-flash-lite",
    contents=f"You are an editor. Make it clearer and stronger. Flag any line that sounds made up:\n{answer}"
).text

print("FIRST DRAFT:", answer)
print("\nIMPROVED:", review)
```

Click **Run** and read both outputs.

> 💡 **The big idea:** the second call has *no memory* of your chat. It only sees the text you put in `contents`. That’s the whole lesson — Draft → Review is the simplest agent pipeline.

<details>
<summary>🔍 What does this code mean?</summary>

**First call (uses your chat memory):**  
- Save a prompt in `question`  
- Send it with `chat.send_message`  
- Store the reply text in `answer`

**Second call (fresh, standalone):**  
- `client.models.generate_content(...)` is a **one-shot** request — not part of the chat history  
- `contents=f"...{answer}"` is an **f-string**: the `f` before the quote means “fill in the blanks.” `{answer}` gets replaced with whatever draft you just got  
- `\n` means “new line” (a line break inside the text)

**Pipeline in plain English:**  
Writer AI drafts → you catch the draft as text → Editor AI improves that text → you print both.

</details>

---

## Step 10 — Take it home

Your Repl URL **is** your bot. Come back anytime. Keep building. Use it on real Hands for Humanity portfolio work.

### Responsible use
- 🔒 No real names, phone numbers, or passwords in your prompts
- 🚫 Don’t ask the AI to invent facts, quotes, or stats — always verify
- 👀 Always read AI output before publishing or sharing
- 📋 Document every AI interaction you use in your portfolio

### What to put in your portfolio
1. Screenshot of your working Repl console  
2. The role / `system_instruction` you chose — and why  
3. A log of 2–3 prompts + the AI responses you used  
4. One sentence: what worked well · one sentence: what you’d improve  

---

## Quick reference

| Thing | Where |
|---|---|
| Your API key | Replit Secrets → `GEMINI_API_KEY` |
| Your code | `main.py` |
| Model name | `gemini-3.1-flash-lite` |
| Image making (Graphic team) | Gemini Create images (free) — Canva optional after |
| Library docs | https://ai.google.dev/gemini-api/docs |

### When something breaks

| Problem | Likely fix |
|---|---|
| `KeyError: 'GEMINI_API_KEY'` | Secret name mistyped — must be exactly `GEMINI_API_KEY` (Secrets, not Configurations) |
| Red underline: Cannot resolve `google` | Editor lag — click **Run**. If Console works, ignore the squiggle |
| `ModuleNotFoundError: No module named 'google'` | Shell: `pip install google-genai` then Run again |
| `404` model not found / not available | Use `gemini-3.1-flash-lite` |
| `503` high demand | Wait 30 seconds and Run again |
| Indentation / unexpected indent | Spaces under `while True:` got messed up — re-paste Step 7 |
| Messy object instead of text | You forgot `.text` |
| Bot ignores your role | Make sure Step 6 `system_instruction` is still in `main.py` and you deleted the Step 5 test chat |
| Bot invents AIM as AI/ML | Remind it: AIM is Kinston Teens summer program; communications portfolio only |

---

*KT-AI-Teammate-Starter · Kinston Teens AIM 2026 · Hands for Humanity*
