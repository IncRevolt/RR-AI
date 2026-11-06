# RR AI — a private AI that runs on your own machine

From [Renewable Revolt](https://renewablerevolt.org), a veteran-owned 501(c)(3) nonprofit.

Your Renewable Revolt machine runs a real AI **on the computer itself** — not in a company's data center.  Nothing you type ever leaves the box: no account, no subscription, no limits, and it keeps working with the internet unplugged.  It isn't a bigger, faster cloud chatbot — it's **a private assistant you own**, for the work you'd never send to a stranger's server.  Here's what it is, how to set it up, and how to get the most out of it.

---

## What it is

### Local AI vs. the AI you're used to

The free AI on your phone or browser runs on massive hardware in a company's data center.  It's fast and knows a little about everything — but every word you type travels over the internet to that company, you're rate-limited ("come back later"), and it can change or disappear whenever they decide.

Your private AI runs entirely on **your** machine.  Nothing you type ever leaves the box.  No account, no sign-in, no monthly fee, no limits, and it keeps working with the internet unplugged.  The trade-off is honest: it's a smaller, hand-tuned model on modest hardware, so it's slower and not as encyclopedic as a giant cloud model.

So don't think of it as "a worse ChatGPT."  Think of it as a private assistant you own — for the work you'd never want to send to a stranger's server.

### What it's great at

- **Anything private or sensitive** — personal finances, medical notes, legal letters, journaling, business records, passwords you're organizing.  The stuff you should never paste into a free cloud chatbot.
- **Reading your documents and photos** — summarize a contract, pull the numbers off a receipt, explain a form.  Offline, on your machine.
- **Everyday writing** — drafting and rewriting emails, summarizing long text, brainstorming, explaining a concept in plain words.
- **Coding help** — a private pair-programmer.
- **Making images** — private, unlimited, no subscription.
- **Working with no internet** — on a plane, in a cabin, anywhere.
- **Using it as much as you want** — no message caps, ever.

### When to reach for a cloud AI instead

Your private AI is powerful, but it isn't the right tool for everything.  Use a web-connected AI (the chat page suggests a private one, **duck.ai**) when you need:

- **Today's news or anything recent** — your local AI is offline and has no live web.  It doesn't know what happened this week.
- **The hardest reasoning or math**, or deep niche expertise, where you need maximum accuracy.
- **A giant document** all at once — very long files can exceed what the local model holds in mind.
- **An answer in two seconds** — a photo or document read here takes a minute or two.  That's the cost of keeping it private and on your hardware.

---

## Download & set it up

There are two releases.  The bigger your machine's graphics card, the more it runs — the full model × machine matrix is in [`docs/stack.html`](docs/stack.html), and what's in each release is listed in its **release notes**.

| Release | For a graphics card with | What it does |
|---|---|---|
| **4–6 GB** | 4–6 GB | Private chat + image generation — the entry stack (one launcher). |
| **8–12 GB** | 8 GB or 11–12 GB | Adds **image & document reading**, a coding assistant, and more image styles (two launchers — the bigger card reads images faster and handles longer documents). |

### Opens in LibreWolf — keep it out of your main browser

RR AI opens in **LibreWolf** — a fast, private, open-source browser — if it's installed, so your AI chats and images stay out of your everyday browser.  Don't clutter your main browser: **install the free LibreWolf browser ([librewolf.net](https://librewolf.net))** before first launch and the AI opens there automatically.  Without it, the AI falls back to your default browser.

### Install

1. On the release page, download **all** the `.7z` parts (`…7z.001`, `.002`, …).
2. Right-click the `.001` part → 7-Zip → **Extract** to `C:\Users\Public\` (this recreates `C:\Users\Public\RenewableRevolt\`).
3. Double-click the **launcher** that matches your card — it opens the AI in your browser.  (AMD cards use the **Vulkan** launcher, NVIDIA the **CUDA** one.)

Only one model runs at a time — each launch shuts the previous one down first, so the graphics card is never overloaded.  If SmartScreen warns on a launcher the first time, choose **More info → Run anyway**.

---

## How to use it

### How to write a good prompt (works with any AI)

Most people get weak answers because they give weak prompts.  A few habits fix that:

1. **Say exactly what you want** — the task, and what a good result looks like.
2. **Give it the material** — paste the text or the document.  Don't make it guess.
3. **Name the format** — "a 5-bullet summary," "a polite 3-sentence email," "a table."
4. **Set the audience and tone** — "explain it like I'm new to this," "keep it formal."
5. **One job at a time** — break a big task into steps.  Smaller, focused asks get sharper answers, especially on a local model.
6. **Fix it in rounds** — if the first draft is off, tell it what to change ("shorter," "more formal," "you missed the deadline").  Don't start over.

**Weak:** "write about dogs."
**Strong:** "Write a warm, 100-word paragraph for my pet-sitting flyer about why golden retrievers are great with kids.  Simple, friendly language."

The second one gets a usable answer the first time.

### Tips for your RR machine

- **Pick the right model for the job.**  *Quick* is fastest for everyday chat.  *Assistant + Vision* is the deep one that reads images and documents.  *Coding* is for code.
- **For photos and documents, ask one image, one clear question** — it's faster and more accurate than a pile of files with no instructions.
- **Give it a moment.**  A vision read takes a minute or two — that's normal for a private model on your own hardware.
- **Keep the private stuff here.**  That's the whole point.  Finances, health, personal records — this is where they belong.

### One honest note

Every AI — cloud or local, big or small — can sound confident and still be wrong.  **Check anything that matters** (a date, a dollar figure, a medical or legal detail) before you act on it.  Your private AI is a sharp, capable assistant, not an oracle — and its real superpower isn't being the smartest model in the world, it's being the one that **never shares your business with anyone.**

---

*Model payloads ship as the sliced Release assets, never in git — which models are in which release is in each tier's **release notes**, and launcher-by-launcher differences are in [`docs/TIER-DIFFERENCES.md`](docs/TIER-DIFFERENCES.md).  Keeping the machine's OS current is a separate tool, the **rr-turbo-update** repo.*

## License

[Apache License 2.0](LICENSE) — © 2026 Renewable Revolt, Inc.  "Renewable Revolt", "RR Turbo", and "RR AI" are trademarks of Renewable Revolt, Inc.  Bundled third-party models remain under their own licenses (linked from the matrix).
