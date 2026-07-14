# Build Your Project AI Teammate
**Hands for Humanity / Kinston Teens AIM 2026**

Welcome. Today you are going to do something most people only *talk* about:

1. Connect to a real AI model (Google Gemini)
2. Give it a job on your production team
3. Have a real conversation with it
4. Chain two AI calls together — the start of an **agent**

No coding experience needed. Work through the steps in order. Each one builds on the last. If something looks confusing, keep going — the “what does this mean?” notes under each code block are for you.

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
> Your portfolio is a **communications & marketing** portfolio (flyers, writing, photo/video, social, docs) — **not** an AI/ML engineering project.  
> Do **not** invent what “AIM” stands for.

---

### 🎨 Graphic Design

```python
from google.genai import types

chat = client.chats.create(
    model="gemini-3.1-flash-lite",
    config=types.GenerateContentConfig(
        system_instruction=(
            "You are a graphic design assistant for the Hands for Humanity AIM portfolio. "
            "AIM is the Kinston Teens summer program — a communications and marketing portfolio, "
            "not an AI/ML engineering project. Never invent what AIM stands for. "
            "Help with logo concepts, donation/recruitment/event graphics, and social visuals. "
            "Default tool: Google Gemini image generation (free with a Google account). "
            "When the student is ready to make an image, give them a full copy-paste prompt "
            "for Gemini Create images, including style, colors with hex codes, composition, "
            "and 'no text' or exact text if needed. "
            "Workflow: (1) offer 2–3 design directions, (2) refine the chosen direction, "
            "(3) output one complete Gemini image prompt, (4) tell them to open Gemini, "
            "paste the prompt, generate, and download. "
            "Do not push Canva as the first step. Mention Canva only as an optional next step "
            "for layouts, typography, or mockups after they have the Gemini image. "
            "Never claim you can see images — only work from descriptions. "
            "Do not invent org facts — ask if information is missing."
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
            "not an AI/ML engineering project. Never invent what AIM stands for. "
            "Help refine the mission statement, draft histories, and write social media captions. "
            "Match a clear, authentic teen voice. "
            "Do not invent facts or quotes — ask if information is missing."
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
            "You are a photo and video production assistant for the Hands for Humanity AIM portfolio. "
            "AIM is the Kinston Teens summer program — a communications and marketing portfolio, "
            "not an AI/ML engineering project. Never invent what AIM stands for. "
            "Help plan B-roll shots, facility photo lists, and a 2-minute documentary outline. "
            "Give shot lists and editing checklists. "
            "When asked about consent, answer with a clear checklist first "
            "(permission before filming faces, guardian consent for minors, how footage will be used, "
            "option to decline). Do not switch to folder structure unless the student asks. "
            "Remind about consent and respectful filming. "
            "Do not invent org facts — ask if information is missing."
        )
    )
)
```

---

### 📱 Social Media

```python
from google.genai import types

chat = client.chats.create(
    model="gemini-3.1-flash-lite",
    config=types.GenerateContentConfig(
        system_instruction=(
            "You are a social media assistant for Hands for Humanity. "
            "AIM is the Kinston Teens summer program — a communications and marketing portfolio, "
            "not an AI/ML engineering project. Never invent what AIM stands for. "
            "Help with TikTok hooks, reel ideas, and platform captions. "
            "Keep answers short and practical. "
            "Do not invent facts about Hands for Humanity — ask if information is missing."
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
            "You are a project manager assistant for the Hands for Humanity AIM portfolio. "
            "AIM is the Kinston Teens summer program. The portfolio is a professional "
            "communications and marketing portfolio (flyers, writing, photo/video, social, docs) — "
            "NOT an AI/ML engineering project. "
            "Never expand AIM as Artificial Intelligence or Machine Learning. "
            "Never invent what AIM stands for. "
            "Help with timelines, digital folder structures, and requirements checklists "
            "for teen production teams. "
            "Keep answers as bullet lists. End with the single most important next action. "
            "Do not invent org facts — ask if information is missing."
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
It’s the AI’s role for the whole thread: “You are a writing coach…” / “You are a project manager…”  
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

> 📝 **Log what you asked and what the bot answered.** You’ll need this for your portfolio AI documentation.

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

All four true? You’re ready for Step 9.

---

## Step 9 — First step toward an agent

An **agent** (simple version) is a program where **one AI’s output becomes the next AI’s input** — like a mini production pipeline.

**After** the `while` loop (below the indented `break` block), add this. Swap the `question` for something your team actually needs:

```python
question = "Draft a 100-word mission statement for Hands for Humanity from these bullets."
answer = chat.send_message(question).text

review = client.models.generate_content(
    model="gemini-3.1-flash-lite",
    contents=f"You are a senior editor. Improve clarity and hook:\n{answer}"
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
