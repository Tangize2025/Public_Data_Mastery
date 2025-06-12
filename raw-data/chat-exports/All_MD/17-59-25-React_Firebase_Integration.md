## ASSISTANT
{
  "prompt": "A bold, high-impact book cover titled 'Conversation at the Speed of Thought'. The design features a split-screen layout: on the left, a vintage silhouette of a human head with gears and circuits inside representing thought; on the right, a speech bubble breaking apart into digital fragments, symbolizing rapid-fire conversation. The background is a deep blue gradient with electric gold and white typography. At the top, 'New York Times Bestseller' is written in elegant serif. The style is modern, clean, and visually arresting, meant to suggest urgency, intelligence, and digital overload.",
  "size": "1024x1536"
}

## USER
It’s called “conversation at the speed of thought”

## ASSISTANT
{
  "prompt": "A striking and globally viral book cover for a nonfiction bestseller titled 'Conversation at the Speed of Thought'. The design features a modern, minimalist aesthetic with bold typography. A glowing neural network or thought-web overlays a transparent human silhouette. The background is a deep cosmic blue with stars subtly integrated, symbolizing intellect, speed, and interconnected consciousness. The title is in clean, futuristic sans-serif font, with the subtitle 'How to Reclaim Your Mind in a Hijacked World' in smaller text. The layout should evoke urgency and clarity without clutter. Designed to stand out in digital marketplaces and airport bookstores.",
  "size": "1024x1536"
}

## USER
Generate a global viral best seller worthy book cover 

## ASSISTANT
Locked and logged.

Here’s your build plan broken into deployable modules:

---

## 🔁 1. Stripe & PayPal Checkout Integration

### 🔧 Backend Setup (Firebase Functions or Node.js)
- `createCheckoutSession(bookId, userId)` → returns Stripe Checkout link
- Webhook: listens to `checkout.session.completed`, then writes:
  ```js
  db.collection('users').doc(userId).set({
    purchases: { [bookId]: { full: true, timestamp: Date.now() } }
  }, { merge: true });
  ```

### 🖼️ Frontend
- Replace "Simulate Purchase" with:
  ```js
  window.location.href = await getStripeCheckoutUrl(bookId);
  ```
- Optional: Add PayPal button as fallback.

---

## 📲 2. Offline Access (PWA + IndexedDB)

### ✅ Add PWA Support
- `manifest.json` → app name, icons, theme
- Register service worker with caching strategy (Workbox)

### 🧠 IndexedDB Layer
- On chapter load: save to IndexedDB
- On offline load: check IndexedDB first, then fallback to Firestore

Use [Dexie.js](https://dexie.org/) for clean API.

---

## 📤 3. Export to PDF/EPUB

### 📄 PDF (Frontend or Server-Side)
- Button: `Download PDF`
- Uses:
  ```js
  import html2pdf from 'html2pdf.js';
  html2pdf().from(chapterHtml).save('chapter1.pdf');
  ```

### 📚 EPUB (Server-Side)
- Use [`epub-gen`](https://github.com/cyrilis/epub-gen)
- Input: chapters + metadata from Firestore
- Output: `.epub` file for download

Add "Download EPUB" option after full purchase.

---

## 📚 4. Multi-Book Library

### 🧱 Firestore Structure
```json
/books/
  convo_speed/
    metadata: { title, description, price, ... }
    chapters/{1, 2, 3, ...}
  human_becoming/
    ...
```

```json
/users/{userId}/purchases: {
  convo_speed: { full: true },
  human_becoming: { chapters: ["1", "2"] }
}
```

### 🖼️ Frontend UX
- BookSelector view (homepage grid)
- BookDetail → chapter preview + "Buy" options
- Shared Reader view scoped by `bookId` and `chapterId`

---

## 🚀 Execution Stack

| Layer         | Tech                          |
|---------------|-------------------------------|
| Frontend      | React + Tailwind              |
| Backend       | Firebase Functions OR Node.js |
| Auth          | Firebase Auth                 |
| Payments      | Stripe + PayPal SDK           |
| Offline       | Workbox + IndexedDB (Dexie)   |
| Export        | `html2pdf.js`, `epub-gen`     |

---

## 🔧 Deployment Plan

1. ✅ Confirm Firebase + Stripe backend access
2. 🔄 Swap simulated purchases for real payment logic
3. 🌐 Add manifest + service worker for PWA
4. 🧠 Enable IndexedDB for offline reading
5. 📤 Set up PDF/EPUB generators
6. 📚 Modularize UI and Firestore schema for multi-book rollout

---

Let me know your preferred dev environment (Node.js, Firebase Functions, Vercel, etc.) and I’ll scaffold the full project structure to drop into your stack.

## ASSISTANT
User has requested implementation of the following features for their eBook platform:

1. Stripe and PayPal checkout integration for real purchases.
2. Offline access using both PWA service workers and IndexedDB caching.
3. PDF and EPUB export functionality for chapters and full books.
4. Multi-book library structure including UI updates, Firestore restructuring, and scalable purchase logic.

This is in support of a global bestseller rollout for "Conversation at the Speed of Thought" and future Human Becoming / Tangize titles.

## USER
All that 

## ASSISTANT
Here’s how to level up your eBook web app for **a global bestseller launch**—assuming _“Conversation at the Speed of Thought”_ is about to go viral. These upgrades move you from MVP to scalable product:

---

## 🔁 1. Stripe/PayPal Checkout Integration
To move beyond simulated purchases:

### **Stripe Integration (Recommended)**
- **Frontend**: Add a "Buy Now" button that calls your backend with item ID.
- **Backend (e.g. Firebase Functions or Node.js)**:
  - Use `stripe.checkout.sessions.create()` to generate a payment link.
  - Set success/cancel URLs to route back into your app.
- **Webhook**:
  - Listen to `checkout.session.completed`.
  - On success, write to:
    ```
    /artifacts/{appId}/users/{userId}/purchases/user_purchases
    ```

➡️ Use [Stripe Checkout](https://stripe.com/docs/payments/checkout/quickstart) or [Stripe Elements](https://stripe.com/docs/stripe-js) depending on your need for customization.

### **PayPal (optional layer)**
- Add PayPal button via [PayPal JS SDK](https://developer.paypal.com/sdk/js/).
- On payment success, write access just like Stripe.

---

## 📴 2. Offline Access via IndexedDB or PWA

### **Option A: PWA (Progressive Web App)**
- Add `manifest.json` + service worker via [Workbox](https://developer.chrome.com/docs/workbox/).
- Cache assets + chapter content when online.
- Mark chapters with an “Available Offline” icon.

### **Option B: IndexedDB (Granular Caching)**
- On first read, save chapter content in `IndexedDB` (via [Dexie.js](https://dexie.org/)).
- Load from cache if offline, fallback to Firestore otherwise.
- Great for chapter-by-chapter caching.

---

## 📤 3. PDF/EPUB Export Functionality

### **PDF Export**
- Use `html2pdf.js` or Puppeteer backend to:
  - Render current chapter or entire book to PDF
  - Offer “Download PDF” once full purchase confirmed

### **EPUB Export**
- Build chapter structure as `.epub` via:
  - [`epub-gen`](https://github.com/cyrilis/epub-gen) (Node.js backend)
  - Or third-party conversion using backend like Calibre

Include:
- Cover image
- Metadata
- Table of Contents
- Chapter formatting

---

## 📚 4. Multi-Book Library Expansion

Your current app is **single-book scoped**. Upgrade structure like this:

### **Firestore Restructure**
```json
/books/
  bookId1/
    metadata/
    chapters/
    purchases/
  bookId2/
    ...
```

### **UI/UX Changes**
- Home: grid of available books
- Book Detail Page: show preview, price, chapters
- Reader: reuses current viewer, just scoped by `bookId`

### **User Purchases Model**
```json
users/{userId}/purchases: {
  "book_convo_speed": { full: true },
  "book_human_becoming": { full: false, chapters: ["1", "3"] }
}
```

---

## 🏆 Final Framing

This infrastructure:
- Lets each chapter function as both a **lead magnet** (free preview) and **conversion funnel**
- Supports global distribution via Stripe + PWA
- Enables future scaling to **Human Becoming**, **Tangize** manuals, and **Ghost of Atlanta** modules
- Positions you for Substack-like cross-promo (newsletter-to-book funnel)

---

## Want Next?

I can deliver:
1. Stripe-ready backend function boilerplate
2. Offline reader caching scaffold (PWA + IndexedDB hybrid)
3. EPUB export script from Firestore book structure
4. Full multi-book Firestore schema + modular React reader

Just say the word.

## USER
Explain. Add actual Stripe/PayPal checkout integration
	•	Offline access / caching with IndexedDB or PWA
	•	PDF/EPUB export functionality
	•	Expand from book to multi-book library structure


Because the book in question in my opinion is about to become a New York Times and international best seller 

## ASSISTANT
What you’re looking at is a **React-based eBook reader web app** with **Firebase integration** for:

1. **Authentication** (anonymous or token-based)
2. **Purchases tracking** (for gating paid content)
3. **Real-time Firestore sync** (live updates to user data)

---

### **Key Functional Pieces**

#### 🔐 **Firebase Initialization + Auth**
```js
const app = initializeApp(firebaseConfig);
const auth = getAuth(app);
const db = getFirestore(app);
```
- Initializes Firebase services.
- Uses `signInAnonymously` or `signInWithCustomToken` to identify users.
- Tracks auth state with `onAuthStateChanged`.

---

#### 📚 **Chapter Content Structure**
```js
const chapters = { prologue: { ... }, chapter1: { ... }, ... };
```
- Each chapter has:
  - `title`
  - `freeContent` (always visible)
  - `paidContent` (requires purchase)

---

#### 💸 **Purchase Access Logic**
```js
const hasAccess = useCallback((chapterKey) => { ... }, [userPurchases]);
```
- Determines if the user has:
  - Full book access (`full_book`)
  - Or specific chapter access (`chapterX`)

---

#### 🧾 **Simulated Purchase Handling**
```js
const purchaseContent = async (itemType, itemId) => { ... };
```
- Writes to Firestore:
  ```
  artifacts/{appId}/users/{userId}/purchases/user_purchases
  ```
- Merges new purchases into existing ones.

---

#### 🧠 **User State + Loading Management**
- `useEffect` hooks in:
  - Tracks auth state.
  - Loads user purchase info via Firestore snapshot.
- `isLoading` and `isAuthReady` control UX feedback like spinners.

---

#### 💬 **Dynamic Content Rendering**
```jsx
{currentChapterData.freeContent}
{!isChapterPurchased && <Buy button />}
{isChapterPurchased && currentChapterData.paidContent}
```
- Shows gated or full content based on access.
- UI updated dynamically as purchases are made.

---

#### 🧭 **Navigation**
```js
const getNeighborChapters = (currentKey) => { ... };
```
- Calculates previous/next chapter for navigation buttons.

---

#### 🪟 **Modal Utility**
- Modular modal component triggered on:
  - Purchase success/failure
  - Download placeholder
  - Auth prompts

---

#### 📱 **UI Design Choices**
- Uses Tailwind CSS-like classes.
- Responsive sidebar with animated TOC.
- Sticky header, smooth scroll, gradient background.

---

### **Why It’s Structured This Way**
- **Componentized UI**: Modal, ChapterViewer, and MainContent logic all live in one React component (could be split later).
- **Simulated Payments**: Emulates purchase logic via Firestore without Stripe/payment gateway (good for MVP).
- **Real-Time Updates**: `onSnapshot` lets purchases reflect immediately in the UI.
- **Firebase-First**: Abstracts session, auth, and user data storage behind Firebase stack.

---

### **Ideal Use Case**
- A lean, Firebase-hosted digital book platform that:
  - Gives free sample content
  - Gated premium content
  - Tracks engagement/purchases per chapter
  - Uses anonymous or custom token sign-in
  - Deployable on Vercel/Netlify with Firebase backend

---

### 🚀 Next Steps (if you're extending this)
- Add actual Stripe/PayPal checkout integration
- Offline access / caching with IndexedDB or PWA
- PDF/EPUB export functionality
- Expand from book to multi-book library structure

Let me know if you want this modularized or ported to a framework like Next.js.

## USER
Whats all this do 

import React, { useState, useEffect, useCallback } from 'react';
import { initializeApp } from 'firebase/app';
import { getAuth, signInAnonymously, signInWithCustomToken, onAuthStateChanged } from 'firebase/auth';
import { getFirestore, doc, getDoc, setDoc, onSnapshot, collection, query, where, addDoc } from 'firebase/firestore';

// Ensure Firebase config and app ID are defined, provided by the Canvas environment
const firebaseConfig = typeof __firebase_config !== 'undefined' ? JSON.parse(__firebase_config) : {};
const appId = typeof __app_id !== 'undefined' ? __app_id : 'default-app-id';
const initialAuthToken = typeof __initial_auth_token !== 'undefined' ? __initial_auth_token : null;

// Initialize Firebase App
const app = initializeApp(firebaseConfig);
const auth = getAuth(app);
const db = getFirestore(app);

// Helper component for modals
const Modal = ({ title, message, onClose, actions }) => (
  <div className="fixed inset-0 bg-gray-900 bg-opacity-75 flex items-center justify-center z-50 p-4 font-inter">
    <div className="bg-white rounded-xl shadow-2xl p-8 max-w-sm w-full text-center relative">
      <h3 className="text-2xl font-bold text-gray-800 mb-4">{title}</h3>
      <p className="text-gray-700 mb-6">{message}</p>
      <div className="flex flex-col space-y-3">
        {actions.map((action, index) => (
          <button
            key={index}
            onClick={action.handler}
            className={`px-6 py-3 font-bold rounded-lg shadow-md transition-colors duration-300
              ${action.primary ? 'bg-indigo-600 text-white hover:bg-indigo-700 focus:ring-indigo-400' : 'bg-gray-200 text-gray-800 hover:bg-gray-300 focus:ring-gray-400'} focus:outline-none focus:ring-2 focus:ring-opacity-75`}
          >
            {action.label}
          </button>
        ))}
      </div>
      <button
        onClick={onClose}
        className="absolute top-4 right-4 text-gray-500 hover:text-gray-700 transition-colors"
        aria-label="Close modal"
      >
        <svg xmlns="http://www.w3.org/2000/svg" className="h-6 w-6" fill="none" viewBox="0 0 24 24" stroke="currentColor">
          <path strokeLinecap="round" strokeLinejoin="round" strokeWidth={2} d="M6 18L18 6M6 6l12 12" />
        </svg>
      </button>
    </div>
  </div>
);

// Main App component for the book reader
function App() {
  const [currentPage, setCurrentPage] = useState('prologue');
  const [isMenuOpen, setIsMenuOpen] = useState(false);
  const [isLoading, setIsLoading] = useState(true); // Start as true for initial auth/data load
  const [user, setUser] = useState(null);
  const [userId, setUserId] = useState('loading...'); // Display user ID
  const [userPurchases, setUserPurchases] = useState({}); // Stores purchased items
  const [showModal, setShowModal] = useState(false);
  const [modalContent, setModalContent] = useState({});
  const [isAuthReady, setIsAuthReady] = useState(false); // New state to track auth readiness

  // Define chapter content with free and paid sections
  const chapters = {
    prologue: {
      title: 'Prologue: Conversation at the Speed of Thought',
      freeContent: (
        <>
          <h2 className="text-3xl font-bold text-gray-800 mb-6 text-center">When Meaning Can't Keep Up</h2>
          <p className="text-lg leading-relaxed text-gray-700 mb-4">
            What happens when conversation moves faster than comprehension? In an age of instant updates and split-second reactions, we are losing the ability to metabolize meaning in real-time dialogue. Decades ago, media sages like Neil Postman and Marshall McLuhan saw the storm coming. Postman warned of a "vast and trembling shift from the magic of writing to the magic of electronics," a rapid dissolution of the slow-print habits that sustained thoughtful public discourse. McLuhan famously taught that "the medium is the message," meaning the form of our communication shapes its content. But today the medium has raced so far ahead that our messages are rendered incoherent before we can grasp them. Our digital environment is engineered to fragment our attention into confetti-sized pieces. This isn't mere distraction – it is a systemic collapse of sustained focus, a fundamental restructuring of how we think. We scroll and scroll, ingesting endless bytes of information without digesting any of it. The result? Dialogue dissolves into disjointed noise. "When, in short, a people become an audience and their public business a vaudeville act, then a nation finds itself at risk; culture-death is a clear possibility," Postman warned. We have reached that precipice. The stakes could not be higher: if we cannot slow down enough to process meaning in our conversations, we risk nothing less than the loss of our collective grasp on reality. In the whirlwind of $24/7$ feeds, meaning itself is burning up on reentry.
          </p>
        </>
      ),
      paidContent: (
        <>
          <h2 className="text-3xl font-bold text-gray-800 mb-6 text-center mt-8">The Silent Voices</h2>
          <p className="text-lg leading-relaxed text-gray-700 mb-4">
            Beneath the roar of constant content lies an eerie silence. Whose voices have been muted by the speed of this new media world? They are the silent victims of our attention collapse: the voices that speak too slowly, too softly, too contrarily, or too deeply for algorithmic culture to notice. In a digital public square governed by algorithms, anything that isn't instantly engaging is pushed to the margins. The system favors the catchy, the controversial, and the concise - the lowest common denominator is promoted at the expense of what is complex, diverse, or challenging. Thoughtful essays languish unread while quips and memes go viral. Empathetic, sensitive voices hesitate (who can hear nuance in a shouting match?), and contradictory ideas get drowned out by the flood of reaffirming echo chambers. The algorithms that curate our feeds effectively decide who gets to be heard. Perhaps you've noticed: a few loud friends always dominate your timeline while others - those who don't feed the machine's appetite - vanish entirely. This is no accident. In fact, "algorithms are now dictating our social networks and who we see," determining which voices surface and which remain invisible. Thus, whole swaths of conversation are lost: the poets, the ponderers, the questioners of consensus all suffocated by an environment that values speed over substance. The tragedy of this silent censorship is that we don't even know what we're missing. How many great ideas, delicate truths, or important warnings have flickered and died in the shadows because they couldn't be distilled into a trending soundbite? In this book, these lost voices are given space to speak again. Their whisper will rise to a roar.
          </p>

          <h2 className="text-3xl font-bold text-gray-800 mb-6 text-center mt-8">Containment Failure</h2>
          <p className="text-lg leading-relaxed text-gray-700 mb-4">
            We find ourselves in a world where our communication systems have suffered a containment failure. By containment, we mean the capacity of a medium or platform to hold a conversation within safe and meaningful bounds - to contain context, emotion, and truth without letting everything combust. That capacity has cracked. This is not just an emotional phenomenon (though certainly our collective emotional thermostat is running hot); it is structural and technological. The very architectures of our social platforms are designed to maximize engagement, and in doing so they have eroded the buffers that once kept discourse civil and coherent. Imagine a reactor with a broken containment vessel: toxic waste leaks everywhere. In our case, what leaks is unprocessed anger, misinformation, and confusion. The design failure is plain to see: engagement-driven algorithms tend to amplify emotionally charged content, particularly anger and animosity. Outrage is literally the business model. As a result, extreme emotions and false narratives spread like wildfire, escaping any attempt at nuance or context. Once, we could disagree within the contained arena of good-faith debate; now every disagreement threatens to blow up into a viral conflagration. Contexts that grounded our understanding have collapsed - your family, friends, colleagues, and strangers are all dumped into the same feed, flattening the distinctions that once guided our tone. The outcome is "aestheticized confusion, narrative sabotage" – a state where genuine dialogue is undermined by a barrage of disjointed content that feels like conversation but delivers only chaos. Our emotional responses, untethered from context, become weapons against ourselves. Containment failure means the system no longer protects us from ourselves. It's not that we've all suddenly become irrational; it's that our platforms reward our most impulsive, least reflective impulses by design. This book will show that unless we rebuild these structures, our information ecosystem will continue to suffer meltdown after meltdown. We will diagnose how the failure of design - the lack of thoughtful constraints and humane defaults - has led to an environment where truth and trust can't find solid ground.
          </p>

          <h2 className="text-3xl font-bold text-gray-800 mb-6 text-center mt-8">The Medium as Mechanism (Controlling Credibility)</h2>
          <p className="text-lg leading-relaxed text-gray-700 mb-4">
            Marshall McLuhan's dictum that "the medium is the message" has taken a new, more dangerous twist. In our time, the medium is the mechanism that controls credibility. The channels through which a story travels now determine whether it is believed at all. Information no longer just flows through media; it is filtered, ranked, and validated by them. A breaking news story on a fringe website will be ignored, but the same story trending on Twitter or summarized by a Google search snippet gains instant legitimacy. Our sense of truth has become entangled with the delivery system. Consider this: being the top result on Google isn't just about convenience - it confers trust. Users tend to assume that whatever sits atop the search rankings must be credible. We have begun to outsource our judgment to algorithms and platforms. On social networks, algorithmic authority means that if you aren't favored by the feed, you might as well not exist. The medium - whether it's a Facebook news feed, a TikTok For You page, or a cable news channel - programs our threshold for belief. The design of these systems decides what counts as real. And because each medium has its own logic, truth itself now splinters according to platform. One might say we live in different realities depending on whether we get our news from TV, Twitter, or TikTok. In earlier eras, credibility was a slow accumulation a newspaper earned trust over decades, a person earned expertise through study. Now, credibility is algorithmic: a blue checkmark, a trending hashtag, a virality score. The medium doesn't just message; it measures and molds credibility in real time. This prologue aims to make one thing clear: we can no longer naïvely ask if something is true or false without also asking "On what platform? In what format?" Because today, the medium is the gatekeeper of truth, deciding which facts pass through and which are turned away at the door. If that mechanism remains unchecked, our ability to agree on reality - the basic credibility of information – will continue to crumble.
          </p>

          <h2 className="text-3xl font-bold text-gray-800 mb-6 text-center mt-8">Re-Architecting the Conversation</h2>
          <p className="text-lg leading-relaxed text-gray-700 mb-4">
            So, what do we do? Surrender to the chaos? Nostalgia is not an option. We cannot simply rewind to a pre-digital era of slow, linear media – that world is gone. Instead, we must re-architect conversation itself for the reality we live in now. This means building new channels, new practices, and new designs that allow meaning to travel at the speed of thought without being lost. We need conversation that is compressed without being diluted - dense with insight yet quick and captivating enough to flourish in our hyperactive feeds. We must learn to speak in new ways if we want to be heard above the algorithmic roar. The solution layer previewed in this book is boldly pragmatic: we will explore how to encode depth into brevity. Think of it as designing nutrient-rich fast food for the mind compact communication that still nourishes. Part of this involves embracing visual-first platforms as carriers of complex ideas. If Instagram and TikTok have captured people's attention, then our dialogues must find a way to live in images, graphics, and videos. In short, pixels now speak louder than words - and those who learn to leverage visuals will shape the new conversation. Imagine infographics that carry the weight of a thesis, or a short video that conveys a nuanced argument in 30 seconds. We must craft messages that can pierce through systemic noise and capture fragmented attention spans without sacrificing truth. This is not about dumbing down; it's about innovating the format of dialogue. We will discuss new communication design principles - how to bypass algorithmic filters and sidestep noise by packaging ideas in more resonant, shareable forms. The goal is a renaissance of discourse that can survive the jump to light speed. By the end of this book, you'll see how conversation can be re-designed: from the protocols and algorithms that underpin our platforms, to the very language and visuals we use to convey meaning. We can create exchanges that travel faster yet sink deeper – a blueprint for dialogue that thrives in the visual, velocity-driven arenas of today's media.
          </p>

          <h2 className="text-3xl font-bold text-gray-800 mb-6 text-center mt-8">A Survival Protocol for Future Meaning-Making</h2>
          <p className="text-lg leading-relaxed text-gray-700 mb-4">
            This is our moment of truth. Will we allow meaning to erode until only noise remains? Or will we rise to the challenge and reinvent how humans converse in a digital age? Conversation at the Speed of Thought declares that the time for mere commentary on media malaise is over what we need now is a survival protocol. Think of this book as a prophetic manifesto meets system diagnostic, yes, but also as a handbook. It's a strategic plan for how we resurrect lost dialogue and safeguard it against the distortions of our era. We are not here to reminisce about the good old days of lengthy letters or fireside chats; we are here to equip you with tools to make meaning under unprecedented conditions. The credibility mechanisms can be hacked for good - we can design platforms that elevate honesty and empathy. The containment failures can be fixed - we can engineer systems that hold space for complexity without melting down. And the silent voices can find amplification through new, creative mediums that bypass the old gatekeepers. This prologue names the crisis: an attention economy on the verge of collapse, a narrative landscape riddled with sabotage, and a media architecture fundamentally broken. But it also names the opportunity: to rebuild conversation from the ground up. The chapters ahead will show how we can compress ideas, protect emotion, restore context, and deliver dialogue in forms that flourish in our visual, fast-paced world. This is not a eulogy for meaningful conversation – it is a battle plan for its rebirth. The message is urgent yet hopeful: we can save the future of meaning-making, but only if we redesign our discourse and evolve the way we communicate. The pages that follow map out nothing less than a survival strategy for the human mind’s most sacred activity: genuine conversation. Are we ready to speak – and listen – at the speed of thought? The survival of our culture’s soul may depend on it.
          </p>
          <p className="text-sm text-gray-500 italic mt-8">
            (End of Prologue)
          </p>
        </>
      ),
    },
    chapter1: {
      title: 'Chapter 1: The Medium Is the Prison',
      freeContent: (
        <>
          <h2 className="text-3xl font-bold text-gray-800 mb-6 text-center">A Wind Tunnel for Words</h2>
          <p className="text-lg leading-relaxed text-gray-700 mb-4">
            I’ve earned the nickname DWF (The Great Abbreviator), but not for any heroic reason. It’s survival. In today’s meetings and rapid-fire chats, everyone is expected to compress thoughts on the fly. Complex ideas get sanded down to sound bites as if we’re all shouting into a wind tunnel. The faster the pace, the more our words get torn apart by turbulence. What passes for “communication” in these environments is often just polished noise – aerodynamic little phrases built to survive high-speed winds, while the real substance gets blown away. This chapter exposes why meaningful conversation can scarcely take root in these conditions, and why the breakdown isn’t your fault – it’s the fault of the container we’re forced to communicate in.
          </p>
        </>
      ),
      paidContent: (
        <>
          <h2 className="text-3xl font-bold text-gray-800 mb-6 text-center mt-8">The Arithmetic of Attention (Or, Why 5 Minutes Gets You 750 Words)</h2>
          <p className="text-lg leading-relaxed text-gray-700 mb-4">
            Let’s start with some simple math to quantify the problem. Most people speak at around 125 to 150 words per minute (WPM) in casual conversation. Faster talkers – those confident, rapid-fire types – might push that to ~175 WPM in sustained speech. In extreme cases, a person could blurt out nearly 229 WPM (a rate approaching auctioneer territory) for a short burst. Now, consider what that means for a typical meeting:
          </p>
          <ul className="list-disc list-inside text-lg leading-relaxed text-gray-700 mb-4 px-4">
            <li><strong>5-minute “quick sync”:</strong> At 150 WPM, that’s about 750 words total. 750 words – roughly the length of two e-mails or a single typed page – is all that can be spoken in five minutes. If two people share that time, it’s maybe 375 words each. That’s barely a few paragraphs of actual information per person. No wonder “let’s sync real quick” so often ends with nothing real being shared.</li>
            <li><strong>10-minute check-in:</strong> Even with one person hogging the mic at 150 WPM, you’d get 1,500 words out – about the length of a short article. Split among three or four people, each person might get only 3-4 minutes to speak (roughly 400–600 words). That’s equal to a couple of spoken paragraphs each, much of which will be trivial greetings, filler, or rushed summaries. The container (10 minutes of airtime) simply cannot hold a detailed exploration from everyone.</li>
            <li><strong>30-minute meeting:</strong> 30 minutes of nonstop talking yields about 4,500 words at 150 WPM (maybe 6,800 words if someone somehow kept a blistering 229 WPM pace the whole time – which never happens). But meetings aren’t nonstop monologues; they involve turn-taking, pauses, tech hiccups (“Sorry, you go ahead… Can you hear me?”). Effective conversational content in a half-hour is far less than 4,500 words. In practice, you might get the equivalent of a short blog post worth of ideas – spread thinly across several participants. Is it any surprise we leave these meetings feeling unheard and misunderstood?</li>
          </ul>
          <p className="text-lg leading-relaxed text-gray-700 mb-4">
            In the cold arithmetic of communication, time is a strict bottleneck. Words are physical, measurable things, and you can only pack so many into a given slot of time. We imagine that a “quick chat” can clarify complex issues, but simple math shows otherwise. If your big idea can’t survive being squeezed into a few hundred words, the solution is not to blame your idea or your verbosity – it’s to recognize the container is too small. You wouldn’t pour a gallon of water into a pint glass and then fault the water for spilling. Yet in modern workplaces, we routinely try to pour gallons of thought into pint-sized meetings and instant messages. The overflow is treated as a personal failure (“You’re rambling,” “Get to the point!”) rather than a structural one.
          </p>

          <h2 className="text-3xl font-bold text-gray-800 mb-6 text-center mt-8">Stripped by Speed: The Wind Tunnel Effect</h2>
          <p className="text-lg leading-relaxed text-gray-700 mb-4">
            So why call this a wind tunnel? Picture a wind tunnel test: engineers blast high-speed air at a prototype to see what breaks off. If the object isn’t perfectly aerodynamic, the gale will rip away any protruding detail. In our conversational wind tunnels – high-speed meetings and rapid-fire digital chats – the “air” is the pressure for speed and brevity. Only the most streamlined statements can withstand it. Nuance and vulnerable truth are like delicate features on a model; in a gale, they shear off. What remains is a smooth, simplified shell.
          </p>
          <p className="text-lg leading-relaxed text-gray-700 mb-4">
            In a wind tunnel, speed strips away nuance. Likewise, in a fast-paced meeting, any idea that isn’t immediately aerodynamic – that requires context, subtlety, or a slow build – gets flattened or ignored. People start favoring form over content: a confident tone and snappy one-liner will sail through the tunnel, while a hesitant but insightful observation gets blown sideways by the next gust of conversation. We come to meetings wearing polish as armor. We present the glossy, streamlined version of our thoughts because the environment leaves no space for the rough drafts of truth. Real conversation – with its pauses, backtracks, and mid-sentence corrections – is as out of place in a corporate meeting as a fragile leaf in a hurricane.
          </p>
          <p className="text-lg leading-relaxed text-gray-700 mb-4">
            This wind tunnel effect encourages what I call “aerodynamic communication.” We reduce and refine our messages not for the sake of clarity alone, but for survivability. Imagine preparing an update for a status meeting: you might leave out the caveats, open questions, and intricate data (those just create drag). Instead, you present a crisp summary that sounds definitive. It’s easier to say “Project is 90% on track” than to admit “We’re 90% done with the easy parts, but unknown risks remain” – the latter would require explanation, maybe unwelcome follow-up questions. Better to stay streamlined. Thus, polish replaces process: we skip the messy process of working through uncertainty and go straight to the headline. The wind tunnel rewards us for it. But this comes at a cost: everyone leaves with a false sense of mutual understanding, unaware of the turbulence that was hidden.
          </p>
          <p className="text-lg leading-relaxed text-gray-700 mb-4">
            And what of vulnerability? It’s the first thing to get blown away. To admit “I’m struggling with this task” or “I don’t fully understand the plan” in a fast meeting is like opening a parachute in a wind tunnel – expect a sudden jolt or crash. The environment all but shuts out vulnerable truth. People learn quickly that if they’re feeling anxious, confused, or have reservations, it’s safer to stay silent or stick to bullet points than to risk authenticity in a space that can’t hold it. In the wind tunnel, only the loudest and simplest signals carry. Everything else is literal background noise.
          </p>
          <h2 className="text-3xl font-bold text-gray-800 mb-6 text-center mt-8">The Quick Sync Syndrome: Meetings That Penalize Depth</h2>
          <p className="text-lg leading-relaxed text-gray-700 mb-4">
            Modern meeting culture is a tempo trap. Consider the ubiquitous phrases: “Let’s sync real quick.” “Just a quick check-in.” They sound innocuous, even responsible – who could object to keeping things short and sweet? But these formats systematically penalize complex thought and reward confident oversimplification. This is the Quick Sync Syndrome: the shorter and faster the exchange, the more it favors those who can reduce issues to snappy updates.
          </p>
          <p className="text-lg leading-relaxed text-gray-700 mb-4">
            In a typical fast-paced check-in, tempo becomes a form of gatekeeping. The conversation moves briskly from one update to the next. If you haven’t condensed your thoughts to a Twitter-length insight beforehand, you either stumble trying to do it in real-time or you yield the floor. Those who do manage to sound-bite their work – often by oversimplifying (“Everything’s fine!”, “No blockers, all good”) – are praised for being “concise and on-point.” Meanwhile, someone who says, “Actually, this issue is complex…” and tries to delve into it will be gently steered back: “Can we take that offline? We’re short on time.” The message is clear: depth doesn’t belong here. Transparency and authenticity are tempo-policed. Only share what fits in the time slot. If it doesn’t fit, trim it or save it for later (which often means never).
          </p>
          <p className="text-lg leading-relaxed text-gray-700 mb-4">
            This syndrome creates a perverse incentive: speak first, even if you have little to say, and speak fast, even if it reduces clarity. In many organizations, the person who can confidently summarize a project in 30 seconds appears more competent than the person who takes 3 minutes to explain the nuanced reality. It’s not that brevity and clarity aren’t virtues – they are – but the current culture confuses shortness with substance. The result is a glut of meetings where everyone “checks in” but nothing meaningful checks out. Important details and uncertainties are glossed over, giving a false sense of alignment… until later when miscommunications surface and everyone wonders, “How did we miss this?”
          </p>
          <p className="text-lg leading-relaxed text-gray-700 mb-4">
            Real-time digital formats crank this up to eleven. Think of the group chat in your team’s messaging app: a flurry of rapid messages where any reply slower than a minute might as well be a week late. In these channels, confident oversimplification reigns supreme. Complex discussion via Slack or Teams often devolves into a series of one-liner hot takes and emoji reactions. Try typing a thoughtful, nuanced message in a busy chat – by the time you hit “send,” ten simpler quips have already moved the conversation on. The medium itself penalizes reflection. We end up with decisions made by whoever can type the fastest or has the snappiest retort, while quieter, deeper ideas languish unsaid. It’s brainstorming in a hurricane: lots of noise, not much nourishment.
          </p>
        </>
      ),
    },
    chapter2: {
      title: 'Chapter 2: The Mixing Is the Metaphor',
      freeContent: <p className="text-lg leading-relaxed text-gray-700 mb-4">The first section of Chapter 2 discusses the evolving nature of authorship in the digital age, where everyone becomes a mixer of content, blending existing information with personal framing and context. It explores how traditional notions of originality are being redefined by the proliferation of interconnected media and algorithmic curation...</p>,
      paidContent: <p className="text-lg leading-relaxed text-gray-700 mb-4">...The rest of Chapter 2 delves into the essential couplings required in the Compression Age, such as Visual/Textual, Personal/Professional, AI/Human, Slow/Fast, and Sacred/Profane. It argues that purity models of communication are failing and introduces the concept of the modern author as a "Mixer"—part DJ, part forensic analyst—who curates, combines, and interrogates content to create coherent narratives amidst noise. The chapter concludes by examining how remix culture, including memes and stitched videos, acts as a form of "narrative repair" and encourages readers to embrace their own complexity in a fractured media reality.</p>,
    },
    chapter3: {
      title: 'Chapter 3: 750 Words in a Wind Tunnel',
      freeContent: <p className="text-lg leading-relaxed text-gray-700 mb-4">The opening of Chapter 3 introduces the concept of communication in a "wind tunnel," where the pressure for speed and brevity in modern meetings and digital chats strips away nuance and genuine substance. It highlights how complex ideas are reduced to "aerodynamic little phrases" just to survive the high-speed environment...</p>,
      paidContent: <p className="text-lg leading-relaxed text-gray-700 mb-4">...The remainder of Chapter 3 quantifies the "Arithmetic of Attention," demonstrating how limited time slots in meetings physically restrict the amount of meaningful content that can be exchanged. It dissects the "Wind Tunnel Effect," explaining how speed strips away nuance and vulnerability, leading to "aerodynamic communication" where polish replaces genuine process. The chapter then analyzes the "Quick Sync Syndrome" in modern meeting culture, which penalizes depth and rewards oversimplification, often leading to a false sense of alignment. It reframes these issues as "Container Failure" rather than personal shortcomings, stressing the psychological toll and "communication PTSD" caused by forcing human thought into ill-fitting formats. Finally, it introduces "Containment Capacity" as a design variable for conversations, advocating for channels that match the natural pace of human cognition.</p>,
    },
    chapter4: {
      title: 'Chapter 4: Conversation at the Speed of Thought',
      freeContent: <p className="text-lg leading-relaxed text-gray-700 mb-4">Chapter 4 redefines "speed" in conversation from raw reaction time to "thought speed," the natural rate at which our brains metabolize ideas. It critiques modern systems that optimize for instantaneous exchange, often at the cost of meaning, and introduces "cognitive latency recognition" as a fundamental design principle for interfaces...</p>,
      paidContent: <p className="text-lg leading-relaxed text-gray-700 mb-4">...The rest of Chapter 4 delves into the neuroscience of attention mechanics, emphasizing the need for "dwell time" and the role of the "default mode network" for reflection, which is currently "razed" by constant interruptions from platforms like Zoom, Twitter/X, and TikTok. It exposes how these tools create "high-resolution miscommunication." The chapter then proposes "Latency as a Design Variable," introducing "Thought-Cycle Windows (TCWs)" to calibrate processing time for different complexities of exchange. It provides case studies illustrating the costs of ignoring thought-time and sketches a "Design Blueprint" for "Thought-Speed Interfaces" with features like "Breathing Buffers," "Processed-Response Indicators," and "Rate Limits on Hot Takes." The chapter concludes by highlighting the ethical and cultural stakes, arguing that designing for thought-speed is crucial for mental health, democracy, and mitigating the spread of falsehoods.</p>,
    },
    chapter5: {
      title: 'Chapter 5: Narrative Sabotage Is a Feature',
      freeContent: <p className="text-lg leading-relaxed text-gray-700 mb-4">Chapter 5 explores how modern digital communication systems are inherently designed to "sabotage" genuine narratives, not as a flaw, but as a "feature." It introduces the concept of "Credibility Hacking," explaining how trust is gamified through superficial signals like fluency and polished visuals, rather than substantive knowledge...</p>,
      paidContent: <p className="text-lg leading-relaxed text-gray-700 mb-4">...The remainder of Chapter 5 delves into "Performance-Optimized Personas," showing how platforms compel individuals to adopt micro-celebrity behaviors for visibility, leading to emotional commodification and a "hollowing effect" on authenticity. It details the "Silent Scars"—the profound psychological toll of constant performance, internalized compression, and "Communication PTSD"—and highlights how whistleblowers, neurodivergent truth-tellers, and trauma survivors are systematically misread and disbelieved. The chapter introduces "Narrative UX" as the user experience of deploying one's truth in a system designed for mistranslation, revealing how algorithms condition what types of pain gain visibility and which are suppressed. Finally, it presents "Sabotage Patterns in Action," with vivid examples of systemic misinterpretations (e.g., a woman crying early seen as manipulative, a Black man pausing too long seen as threatening), culminating in a call to reframe communication failures from individual shortcomings to systemic design flaws, advocating for building "Resonance Engines."</p>,
    },
    chapter6: {
      title: 'Chapter 6: Resonance Engines',
      freeContent: <p className="text-lg leading-relaxed text-gray-700 mb-4">Chapter 6 proposes "Resonance Engines" as a foundational framework for re-architecting communication systems, designed to amplify "felt coherence over time" rather than suppressing it. It defines resonance in contrast to agreement, virality, or reply-count, drawing on concepts like Antonovsky's "Sense of Coherence" and Fisher's "Narrative Paradigm" to emphasize sustained, meaningful understanding...</p>,
      paidContent: <p className="text-lg leading-relaxed text-gray-700 mb-4">...The remainder of Chapter 6 outlines the "Anatomy of a Resonance Engine," detailing its inputs (Contradiction, Slowness & Silence, Layered Signals) and the conditions (Compression without Flattening, Visual + Verbal Parity, Emotional Containment) required to distill signal for lasting impact. It describes the desired outputs: Metabolized Clarity, Re-usable Insight, and Durable Integration, using the metaphor of an engine that processes and transmits meaning cleanly. The chapter then lays out "Design Principles for Amplifying Resonance," including Temporal Variance (embracing Thought-Cycle Windows and delayed feedback), Signal Retention (preserving raw, ambient, and emotional context), and Defer Reaction in Favor of Integration (an "inverted feedback loop" that rewards thoughtfulness over impulsivity). It also emphasizes enabling Narrative Revision and Re-entry Points for evolving understanding. Finally, it presents "Real-World Prototypes" like DEH sessions with rewind windows and async communication tools with emotional tagging, contrasting "Resonance UX" with "Performance UX" to signal a new interface ethos focused on truth over speed, ultimately envisioning a civilization built on resonance across education, therapy, team communications, and storytelling.</p>,
    },
    chapter7: {
      title: 'Chapter 7: Post-Performance Communication',
      freeContent: <p className="text-lg leading-relaxed text-gray-700 mb-4">Chapter 7 maps a world beyond "credibility theater," introducing "Post-Performance Communication" where authenticity is judged by inherent coherence and clarity, not superficial elements like charisma or polish. It begins by diagnosing the "Performance Trap," explaining how digital systems compel "aerodynamic communication" and "emotionally safe" packaging of truths, leading to "Credibility Gatekeeping" where algorithmic biases dictate who is heard based on nonverbal codes and superficial cues...</p>,
      paidContent: <p className="text-lg leading-relaxed text-gray-700 mb-4">...The remainder of Chapter 7 details the "Human Cost" of constant performance, including "internalized compression," "Communication PTSD," and the "hollowing effect." It then maps the "Exit" by embracing "Unscored Speech"—communication decoupled from social scoring and algorithmic favor—and emphasizes "Reclaiming Agency" through modality mixing and intentional pacing. The chapter provides "Micro-Liberations" as glimpses of post-performance expression (e.g., rambling voice notes, truth in a slow monotone, sharing transcripts). It then outlines "Designing for Truth" with platforms incorporating "Thought-Cycle Windows (TCWs)," "Breathing Buffers," "Processed-Response Indicators," "Rate Limits on Hot Takes," "Reflectivity Scores," "Audience-Muted Sharing," and "Delayed Visibility." The chapter concludes by discussing "Societal Impacts" (reimagining relationships, politics, amplifying marginal voices) and "Reframing Value" from "sound" to "substance" and "movement" to "metabolization," ultimately advocating for the "Emancipation" of communication from tyranny.</p>,
    },
    epilogue: {
      title: 'Chapter 8 – Epilogue: The Slow Message Wins',
      freeContent: <p className="text-lg leading-relaxed text-gray-700 mb-4">The Epilogue serves as a "quiet close" to the book, a "cognitive and emotional decompression chamber" after diagnosing the pervasive noise and systemic failures of modern communication. It reaffirms that the problem was never with individuals who struggled to communicate in fast-paced digital environments, but with systems unprepared to metabolize their human bandwidth. It highlights the "Unheard's Enduring Truth," asserting that "slow" and "complex" communication styles are the optimal human bandwidth for genuine truth and durable integration...</p>,
      paidContent: <p className="text-lg leading-relaxed text-gray-700 mb-4">...The remainder of the Epilogue charts the "Deep Shift" from performance to presence, from polish to pattern recognition, and most profoundly, from messaging to metabolizing, emphasizing that the goal was always "more clarity." It offers a "Breath, A Pause," inviting readers to reflect on messages they've silenced, and directly affirms, "You are not too slow. You are metabolizing at human speed." The book is positioned not as a blueprint for scale but as a "tuning fork for coherence," inviting anyone to build their own "resonance engines" and redefining "impact" from broad reach to deep, enduring transformation. The Epilogue concludes by stating the book was a "transmission," not "content," designed to "land" and create "felt coherence," ensuring its message resonates deeply and persistently, embodying the ethos that "The Slow Message Wins."</p>,
    },
  };

  // Check if a user has purchased the full book or a specific chapter
  const hasAccess = useCallback((chapterKey) => {
    // If userPurchases is not yet loaded, assume no access for now (or handle loading state)
    if (!userPurchases || Object.keys(userPurchases).length === 0) {
      return false;
    }
    // Check for full book access
    if (userPurchases.full_book) {
      return true;
    }
    // Check for specific chapter access
    return userPurchases[chapterKey];
  }, [userPurchases]);

  // Handle Firebase Auth state changes
  useEffect(() => {
    const unsubscribe = onAuthStateChanged(auth, async (currentUser) => {
      if (currentUser) {
        setUser(currentUser);
        setUserId(currentUser.uid); // Set actual user ID
        // Listener for user purchases (private data)
        const userPurchasesRef = doc(db, `artifacts/${appId}/users/${currentUser.uid}/purchases`, 'user_purchases');
        onSnapshot(userPurchasesRef, (docSnap) => {
          if (docSnap.exists()) {
            setUserPurchases(docSnap.data());
          } else {
            setUserPurchases({}); // No purchases yet
          }
          setIsAuthReady(true); // Auth and initial data check complete
          setIsLoading(false); // Stop loading after initial data fetch
        }, (error) => {
          console.error("Error fetching user purchases:", error);
          // In case of error, still mark auth ready and stop loading
          setIsAuthReady(true);
          setIsLoading(false);
        });
      } else {
        // Sign in anonymously if no user is found
        try {
          if (initialAuthToken) {
            await signInWithCustomToken(auth, initialAuthToken);
          } else {
            await signInAnonymously(auth);
          }
        } catch (error) {
          console.error("Error signing in:", error);
          // Set a fallback userId if anonymous sign-in also fails
          setUserId('guest_' + crypto.randomUUID());
          setIsAuthReady(true);
          setIsLoading(false);
        }
      }
    });

    return () => unsubscribe(); // Cleanup auth listener
  }, []); // Empty dependency array means this runs once on mount

  // Simulate purchase function
  const purchaseContent = async (itemType, itemId) => {
    if (!user) {
      setModalContent({
        title: "Sign In Required",
        message: "Please ensure you are signed in to make purchases. This app simulates authentication.",
        actions: [{ label: "Okay", handler: () => setShowModal(false), primary: true }]
      });
      setShowModal(true);
      return;
    }

    setIsLoading(true);
    try {
      const userPurchasesRef = doc(db, `artifacts/${appId}/users/${user.uid}/purchases`, 'user_purchases');
      const newPurchases = { ...userPurchases, [itemId]: true, purchasedAt: new Date().toISOString() };
      await setDoc(userPurchasesRef, newPurchases, { merge: true }); // Use merge to avoid overwriting existing purchases

      setUserPurchases(newPurchases); // Update local state immediately
      setModalContent({
        title: "Purchase Successful!",
        message: `You now have access to: ${itemType === 'full_book' ? 'the entire book' : chapters[itemId].title}!`,
        actions: [{ label: "Great!", handler: () => setShowModal(false), primary: true }]
      });
    } catch (error) {
      console.error("Error simulating purchase:", error);
      setModalContent({
        title: "Purchase Failed",
        message: "There was an error processing your purchase. Please try again.",
        actions: [{ label: "Close", handler: () => setShowModal(false), primary: true }]
      });
    } finally {
      setIsLoading(false);
      setShowModal(true);
    }
  };

  const handlePageChange = (pageId) => {
    setIsLoading(true);
    setTimeout(() => {
      setCurrentPage(pageId);
      setIsLoading(false);
      setIsMenuOpen(false);
      window.scrollTo({ top: 0, behavior: 'smooth' });
    }, 300);
  };

  const toggleMenu = () => {
    setIsMenuOpen(!isMenuOpen);
  };

  const currentChapterData = chapters[currentPage];
  const isChapterPurchased = hasAccess(currentPage);

  // Utility to find the next and previous chapter keys
  const getNeighborChapters = (currentKey) => {
    const chapterKeys = Object.keys(chapters);
    const currentIndex = chapterKeys.indexOf(currentKey);
    const prevChapterKey = currentIndex > 0 ? chapterKeys[currentIndex - 1] : null;
    const nextChapterKey = currentIndex < chapterKeys.length - 1 ? chapterKeys[currentIndex + 1] : null;
    return { prevChapterKey, nextChapterKey };
  };

  const { prevChapterKey, nextChapterKey } = getNeighborChapters(currentPage);

  return (
    <div className="min-h-screen bg-gradient-to-br from-gray-50 to-indigo-50 font-inter text-gray-900">
      <div className="container mx-auto p-4 md:p-8 max-w-7xl">
        {/* Header */}
        <header className="flex justify-between items-center py-4 px-6 bg-white shadow-lg rounded-xl mb-8 sticky top-4 z-50">
          <h1 className="text-2xl md:text-3xl font-extrabold text-indigo-700 truncate">
            {currentChapterData.title}
          </h1>
          <div className="flex items-center space-x-4">
            <span className="text-gray-600 text-sm md:text-base hidden sm:block">User ID: {userId}</span>
            <button
              onClick={toggleMenu}
              className="p-3 bg-indigo-600 text-white rounded-full shadow-md hover:bg-indigo-700 focus:outline-none focus:ring-2 focus:ring-indigo-500 focus:ring-opacity-75 transition-all duration-300 ease-in-out"
              aria-label="Toggle menu"
            >
              {isMenuOpen ? (
                <svg xmlns="http://www.w3.org/2000/svg" className="h-6 w-6" fill="none" viewBox="0 0 24 24" stroke="currentColor">
                  <path strokeLinecap="round" strokeLinejoin="round" strokeWidth={2} d="M6 18L18 6M6 6l12 12" />
                </svg>
              ) : (
                <svg xmlns="http://www.w3.org/2000/svg" className="h-6 w-6" fill="none" viewBox="0 0 24 24" stroke="currentColor">
                  <path strokeLinecap="round" strokeLinejoin="round" strokeWidth={2} d="M4 6h16M4 12h16M4 18h16" />
                </svg>
              )}
            </button>
          </div>
        </header>

        {/* Sidebar/Menu */}
        <div
          className={`fixed inset-y-0 right-0 w-full md:w-80 bg-gray-900 bg-opacity-95 transform transition-transform duration-300 ease-in-out z-40
            ${isMenuOpen ? 'translate-x-0' : 'translate-x-full'}`}
        >
          <div className="p-8 h-full flex flex-col justify-between">
            <div>
              <h2 className="text-3xl font-extrabold text-white mb-8 border-b-2 border-indigo-500 pb-4">
                Table of Contents
              </h2>
              <ul className="space-y-4">
                {Object.entries(chapters).map(([key, chapter]) => (
                  <li key={key}>
                    <button
                      onClick={() => handlePageChange(key)}
                      className={`text-left w-full py-2 px-4 rounded-lg transition-all duration-200
                        ${currentPage === key ? 'bg-indigo-600 text-white shadow-lg font-semibold' : 'text-gray-300 hover:bg-gray-700 hover:text-white'}`}
                    >
                      {chapter.title}

{hasAccess(key) && (
                        <span className="ml-2 text-xs bg-green-500 text-white px-2 py-1 rounded-full">OWNED</span>
                      )}
                    </button>
                  </li>
                ))}
              </ul>
            </div>
            <div className="flex flex-col space-y-4 pt-8 border-t-2 border-indigo-500 mt-8">
              {!hasAccess('full_book') && (
                <button
                  onClick={() => purchaseContent('full_book', 'full_book')}
                  className="w-full py-3 px-6 bg-purple-600 text-white font-bold rounded-lg shadow-md hover:bg-purple-700 transition-colors duration-300 focus:outline-none focus:ring-2 focus:ring-purple-400 focus:ring-opacity-75"
                >
                  Buy Full Book
                </button>
              )}
              {hasAccess('full_book') && (
                <button
                  onClick={() => { /* Placeholder for actual download logic */
                    setModalContent({
                      title: "Download Book",
                      message: "This feature is under development. For now, imagine downloading a beautifully formatted PDF or EPUB of the entire book!",
                      actions: [{ label: "Got It!", handler: () => setShowModal(false), primary: true }]
                    });
                    setShowModal(true);
                  }}
                  className="w-full py-3 px-6 bg-green-500 text-white font-bold rounded-lg shadow-md hover:bg-green-600 transition-colors duration-300 focus:outline-none focus:ring-2 focus:ring-green-400 focus:ring-opacity-75"
                >
                  Download Book
                </button>
              )}
            </div>
          </div>
        </div>

        {/* Main Content Area */}
        <main className="bg-white rounded-xl shadow-xl p-6 md:p-10 lg:p-12 min-h-[60vh] relative">
          {isLoading || !isAuthReady ? ( // Show loading if auth is not ready or content is loading
            <div className="absolute inset-0 flex items-center justify-center bg-white bg-opacity-75 z-20 rounded-xl">
              <div className="flex flex-col items-center">
                <div className="animate-spin rounded-full h-12 w-12 border-4 border-t-4 border-indigo-500 border-opacity-25 border-t-indigo-500"></div>
                <p className="mt-4 text-indigo-700 text-lg font-semibold">Loading content...</p>
              </div>
            </div>
          ) : (
            <article>
              {currentChapterData.freeContent}
              {!isChapterPurchased && (
                <div className="mt-8 pt-8 border-t-2 border-dashed border-gray-300 text-center">
                  <p className="text-xl font-bold text-gray-700 mb-6">
                    <span className="text-indigo-600">Unlock the full chapter</span> to continue reading!
                  </p>
                  <div className="flex flex-col sm:flex-row justify-center space-y-4 sm:space-y-0 sm:space-x-4">
                    <button
                      onClick={() => purchaseContent('chapter', currentPage)}
                      className="px-8 py-4 bg-indigo-600 text-white font-bold text-lg rounded-xl shadow-lg hover:bg-indigo-700 transition-transform transform hover:scale-105 focus:outline-none focus:ring-2 focus:ring-indigo-500 focus:ring-opacity-75"
                    >
                      Buy Chapter {currentPage.replace('chapter', '')}
                    </button>
                    <button
                      onClick={() => purchaseContent('full_book', 'full_book')}
                      className="px-8 py-4 bg-purple-600 text-white font-bold text-lg rounded-xl shadow-lg hover:bg-purple-700 transition-transform transform hover:scale-105 focus:outline-none focus:ring-2 focus:ring-purple-500 focus:ring-opacity-75"
                    >
                      Buy Full Book
                    </button>
                  </div>
                </div>
              )}
              {isChapterPurchased && currentChapterData.paidContent}
            </article>
          )}
        </main>

        {/* Navigation Buttons */}
        <div className="flex justify-between items-center py-6 px-4 md:px-0 mt-8">
          {prevChapterKey && (
            <button
              onClick={() => handlePageChange(prevChapterKey)}
              className="px-6 py-3 bg-indigo-500 text-white font-semibold rounded-lg shadow-md hover:bg-indigo-600 transition-colors duration-300 flex items-center space-x-2 rounded-xl"
            >
              <svg xmlns="http://www.w3.org/2000/svg" className="h-5 w-5" viewBox="0 0 20 20" fill="currentColor">
                <path fillRule="evenodd" d="M12.707 5.293a1 1 0 010 1.414L9.414 10l3.293 3.293a1 1 0 01-1.414 1.414l-4-4a1 1 0 010-1.414l4-4a1 1 0 011.414 0z" clipRule="evenodd" />
              </svg>
              <span>Previous Chapter</span>
            </button>
          )}
          {!prevChapterKey && <div className="w-auto"></div>}

          {nextChapterKey && (
            <button
              onClick={() => handlePageChange(nextChapterKey)}
              className="px-6 py-3 bg-indigo-500 text-white font-semibold rounded-lg shadow-md hover:bg-indigo-600 transition-colors duration-300 flex items-center space-x-2 ml-auto rounded-xl"
            >
              <span>Next Chapter</span>
              <svg xmlns="http://www.w3.org/2000/svg" className="h-5 w-5" viewBox="0 0 20 20" fill="currentColor">
                <path fillRule="evenodd" d="M7.293 14.707a1 1 0 010-1.414L10.586 10 7.293 6.707a1 1 0 011.414-1.414l4 4a1 1 0 010 1.414l-4 4a1 1 0 01-1.414 0z" clipRule="evenodd" />
              </svg>
            </button>
          )}
        </div>

        {/* Dynamic Modal */}
        {showModal && (
          <Modal
            title={modalContent.title}
            message={modalContent.message}
            actions={modalContent.actions}
            onClose={() => setShowModal(false)}
          />
        )}

        {/* Footer */}
        <footer className="text-center text-gray-600 text-sm mt-12 py-4 border-t border-gray-300">
          <p>&copy; {new Date().getFullYear()} Conversation at the Speed of Thought. All rights reserved.</p>
        </footer>
      </div>
    </div>
  );
}

export default App;
