# Relish

**A Gemini chef that turns any recipe into a grocery cart, then cooks it with you by voice.**

AI Builder Cup · Retail and Commerce

[Landing page](https://getrelish.app) · [Test the app](https://chef-leo.web.app/) · iOS beta live on TestFlight

---

## The problem

Cooking at home is the most common shopping trip there is, and it is still the most broken one.

- **Recipes moved to video.** The dish you want to cook tonight lives in a 9-minute YouTube video or a reel, not a list you can shop from.
- **Groceries moved to quick commerce.** Instamart can deliver in minutes, but you still have to work out what is missing, type each item in, and guess which pack size you need.
- **Nothing connects the two.** So you scrub a video, scribble a list, check the fridge, switch apps, search item by item, then cook with wet hands while pausing the video every 30 seconds.

The intent to buy is right there ("I want to make rajma tonight"), but the commerce flow starts from a blank search bar.

## Our solution

Relish closes the loop from **craving to cart to cooked** in one conversation, with Gemini doing the work and you staying in control of the money.

| | What you do | What Gemini does |
|---|---|---|
| **1. See it** | Paste a recipe video, or tell Leo what you feel like eating | **Gemini 3.8 Flash** watches the video and writes a structured recipe: ingredients, quantities, steps, timings |
| **2. Shop it** | Say what's missing, tap once | Relish checks the recipe against your fridge list. **Gemini** turns "add rajma and ginger" into list items, then picks the right Instamart pack for each one and fills your real Swiggy cart |
| **3. Cook it** | Talk. Show Leo the pan. | **Gemini 3.8 Live** guides you step by step by voice, watches through the camera, and runs the timers, with no taps |

You review the real bill from Swiggy and pay in the Swiggy app. **Relish fills the cart. It never places or pays for an order.**

> Want the experience rather than the feature list? Read **[A Tuesday evening with Relish](./STORY.md)** (3 minute read).

---

## What makes it different

**Commerce that starts from intent, not search.** Every other grocery flow starts with you typing "paneer". Relish starts with "I want to make palak paneer" and works backwards to a cart: what the dish needs, what you already have, what is missing, which pack covers it.

**An agent, not a chatbot.** Leo doesn't read text at you. It hears you, sees your pan, and acts through tools (next step, repeat, set a timer, open the camera, write up the recipe). The same agent pattern is what lets it act on your cart next.

**Built into a daily task.** Nobody wants another shopping app. People cook every day. Relish puts commerce inside the moment you already have: standing in the kitchen, deciding what to make.

| | Guides by voice | Sees your pan | Recipe to cart |
|---|:---:|:---:|:---:|
| Recipe apps | | | partly |
| Video platforms | | | |
| Voice assistants | partly | | |
| **Relish** | **yes** | **yes** | **yes** |

---

## The AI features

### Leo, a live cooking agent on Gemini 3.8 Live
- Real-time voice in both directions, in **25 reply languages**, with **3 chef personas** (Leo, Nonna and Hiro), each with its own voice.
- **Vision:** turn the camera on and Leo looks at the pan ("that's the colour you want, add the tomatoes now").
- **Tool calls**, not just talk: next, previous and repeat step, parallel timers, camera on, off and flip, and writing up a finished recipe.
- **Record a cook:** the roles reverse. You cook a family dish from memory, Leo takes notes, and it comes back as a proper recipe with your name on it.

### Any video into a recipe, on Gemini 3.8 Flash
- Paste a YouTube link, or share a saved cooking clip from any app. Gemini (through the Files API for shared videos) returns a structured recipe with quantities, steps and timings.

### A kitchen that keeps its own list
- **Kitchen check:** before you cook, the recipe is compared with your fridge list, so you see exactly what's missing.
- **Speak your list:** "add milk, eggs are done" updates the list through Gemini.
- **Auto-deduct:** starting a cook takes the ingredients you're about to use off your fridge list.
- **Auto meal plans:** Gemini plans a 7-day menu around your diet and cuisines.

---

## Swiggy Instamart, built in

This is a real integration, running on Swiggy's Instamart MCP server, not a deep link to a search page.

1. **Connect Swiggy once.** You sign in on Swiggy's own page (OAuth with PKCE). Relish never sees your phone number or OTP.
2. **Tap "Get 6 items on Instamart"** on your shopping list. Everything still to buy goes in one go.
3. **Gemini matches every item to a real pack** at your delivery address, in **one structured call** for the whole list. It picks the smallest in-stock pack that covers the recipe, flags substitutions ("unsalted instead of salted"), and retries with a better search term when nothing fits ("toor dal" becomes "arhar dal").
4. **You review.** Untick, change units, or swap to another pack.
5. **Relish adds them to your real Instamart cart** and shows Swiggy's real bill: items, fees, GST, to pay.
6. **Open Swiggy to pay.** Delivered in minutes.

In testing, 7 grocery items were searched on Instamart in about **3.3 seconds**.

**Safe by design.** The backend can only call five Swiggy tools (addresses, search, and cart read, update and clear). Checkout, payment and order tools are blocked in code even if someone adds them by mistake. Swiggy tokens are encrypted per user, and Gemini only ever sees product names and prices.

---

## Where this goes: agentic commerce in the kitchen

Today, Gemini fills your cart and you pay. The next step is that you just say it.

> **You:** "Leo, we're out of paneer and cream for tonight."
> **Leo:** "Found paneer 200 g and fresh cream 200 ml, about ₹170 with delivery, here in 15 minutes. Shall I order?"
> **You:** "Yes, go ahead."
> **Leo:** "Ordered. Let's start the rice while it's on the way."
>
> *Concept. Prices and timings are illustrative.*

| Stage | What it means | Status |
|---|---|---|
| **Now** | Gemini fills your Instamart cart from your shopping list. You pay in Swiggy. | **Built** |
| **Next** | Missing items go to the cart **straight from the kitchen check or the week's meal plan**, and Leo adds to your cart **by voice, mid-cook**, as a Gemini Live tool. "Add another onion." | Next on the roadmap |
| **Later** | Leo places the order on your spoken yes. Swiggy's MCP already documents a headless checkout with UPI. | Concept |

**Guardrails before Leo can ever spend:** a spoken yes for every order, a spending cap you set, the bill on screen first, and a cancel window. Orders are irreversible, so this step gets its own security review before it ships.

**Beyond Instamart.** The Swiggy connector sits behind one small, allowlisted interface on our backend. Relish is not a Swiggy app; it is the kitchen layer above any store. As other quick commerce and grocery platforms open their APIs or MCP servers, Relish can add them as new connectors and pick the best store per cart, by price, stock or delivery time. The same model extends naturally beyond groceries: ordering in when you'd rather not cook, or booking a table for the dish you just watched.

That is the bet: **the next generation of commerce isn't a better search bar. It's an agent that already knows what you're about to do and has the cart ready when you ask.**

---

## Built with Google

**10 Google services, from the model to the deploy.** Every AI call in Relish is Gemini.

- **Gemini 3.8 Live** for the real-time voice and vision agent
- **Gemini 3.8 Flash** for recipes, pack matching and meal plans
- **Gemini Files API** for videos shared from any app
- **Cloud Run** for the backend (FastAPI, Python 3.11), which scales to zero
- **Cloud Build**, **Artifact Registry** and **Secret Manager** for builds, images and keys
- **Firebase Auth, Firestore and Cloud Storage** for sign-in, data and photos

Plus **Swiggy Instamart MCP** for commerce, Expo and React Native for the app, and RevenueCat for subscriptions.

How it fits together, and how we kept a live AI agent safe and affordable: **[ARCHITECTURE.md](./ARCHITECTURE.md)**.

---

## Where we are

- **Live on TestFlight.** 5 iOS builds shipped.
- **Instamart integration live** on the backend for signed-in users in India.
- **Freemium:** 3 live cooks a month, 1 recipe import a week, and the Instamart cart are free. Pro unlocks more Gemini time.
