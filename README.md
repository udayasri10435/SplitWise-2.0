# SplitWise-2.0
This is a fantastic concept. "SplitWise 2.0" isn't just a CRUD app; it's a **Fintech Utility**. To move from a "project" to a "product," we need to go deep into the **User Experience (UX)** , the **Algorithmic Logic**, the **Data Pipeline**, and the **Monetization Funnel**.

Here is the **Full Deep Improvement Plan** for "SplitWise 2.0," transforming it into a sophisticated, production-ready web application.

---

### Phase 1: The "Smart Scan" Pipeline (Beyond Simple OCR)

Most OCR projects fail because they treat the receipt as a flat image. We need a **3-Stage Pipeline** to ensure accuracy.

**1. Pre-Processing (The "Image Chef")**
- *Improvement:* Don't send the raw image to Tesseract. Use a Canvas API or WebAssembly (e.g., `wasm-vips`) to increase contrast, deskew (straighten), and convert to grayscale locally before sending to the Cloud.
- *Logic:* This reduces Google Vision API costs by 40% because the AI has less "noise" to process.

**2. Semantic Parsing (NER - Named Entity Recognition)**
- *Improvement:* Raw OCR returns gibberish. We need a RegEx/ML layer to categorize lines.
- *Logic:* Identify **Total**, **Subtotal**, **Tax**, and **Line Items**.
- *The "Grouping" Hack:* If an item says "Burger x 2" at $24, your algorithm must automatically create two separate line items of $12 each to assign to different users.

**3. The "Ambiguity" UI (Crucial UX)**
- *Improvement:* Don't auto-split silently. Show a "Review Scan" modal.
- *Deep Logic:* Highlight words the AI is unsure about (confidence score < 80%) in yellow. Allow the user to tap the text to correct it *before* the split occurs. This builds trust.

---

### Phase 2: The Complex Balancing Algorithm (The "Brain")

Moving beyond basic CRUD means implementing the **"Debt Simplification"** algorithm. This is often called the **"Minimum Cash Flow"** problem.

- **The Problem:** If Sarah pays $50, Mike pays $20, and John pays $0, a basic app says: John owes Sarah $25 and Mike $15. That's two transactions.
- **The "2.0" Logic:** Use a **Greedy Algorithm** or **Graph Theory (Cycle Reduction)** to minimize transactions.
- *Implementation:*
  1. Calculate the net balance for each user (Total Paid - Total Owed).
  2. Separate users into "Creditors" (positive balance) and "Debtors" (negative).
  3. Use a two-pointer technique to match the largest debtor with the largest creditor.
- *Deep Improvement:* Add a **"Settle Up"** route that shows a visual "Flow Map" (using D3.js or Vis.js) showing money moving from Debtors to Creditors in the fewest steps possible.

---

### Phase 3: Full-Stack Architecture Deep Dive (Scalability)

To handle multiple groups and receipt images, the architecture must be stateless and efficient.

**Frontend (React/Next.js)**
- **State Management:** Use **Zustand** over Redux. It handles complex nested objects (Groups -> Users -> Items) with less boilerplate.
- **Offline-First:** Use **IndexedDB** (via Dexie.js). If the user scans a receipt on the subway, it saves locally and syncs to the server when the internet returns. This is a "Pro" feature differentiator.

**Backend (Node.js/Python - Django/DRF or FastAPI)**
- **Database Schema Shift:** Instead of a simple `Expense` table, use a **"Ledger"** pattern.
  - Table: `Transactions` (Who paid).
  - Table: `Splits` (Who owes what for which line item).
  - *Deep Logic:* Use **PostgreSQL JSONB** fields to store the "OCR Metadata" (bounding boxes of text). This allows you to visually overlay the text on the receipt image later for auditing.
- **Async Tasks:** Never process OCR in the request/response cycle. Push the image to an S3 bucket and fire a **Celery** (Python) or **BullMQ** (Node) job. Use WebSockets to notify the user when the scan is ready.

---

### Phase 4: Monetization Funnel (The "3-Day Wait" Fix)

Your monetization idea is good, but "removing the 3-day wait" implies you are handling bank transfers. That requires a **Fintech Partner** (e.g., Stripe Connect, Plaid, or Dwolla).

- **Freemium Model:**
  - **Free:** Ads visible in the "Group Feed." Can only scan 5 receipts per month. Basic "I owe you" reminders via email.
  - **Pro ($4.99/mo):**
    1. Unlimited Smart Scans.
    2. **Priority OCR:** Your backend routes Pro users to Google Vision (higher accuracy) and Free users to Tesseract.js (lower accuracy, client-side).
    3. **"Smart Settle":** Integrate with **Plaid API** to verify bank accounts. The app calculates who owes what and initiates a **batch payout** via Stripe, effectively removing the "3-day wait" because you front the liquidity (or use instant payout rails).

---

### Phase 5: Advanced Feature Additions (The "Wow" Factor)

To stand out from Splitwise, add these:

- **"Who's the Glutton?" (Analytics):** A dashboard showing spending habits. *"John has spent 60% of the group budget on Alcohol."* Uses simple data aggregation to create a fun social graph.
- **Recurring Expenses:** Allow users to set "Monthly Rent" as a recurring smart-scan. The app automatically generates the split on the 1st of every month without a photo.
- **Currency Agnostic:** If you scan a receipt in Euros but your group is in USD, use a **Free FX API** (like ExchangeRate-API) to convert it in real-time and lock the exchange rate to that specific date (important for accounting).

---

### Phase 6: The "Soft-Delete" & Audit Trail (Trust)

- *Deep Logic:* Never actually delete an expense. Use a `deleted_at` timestamp (Soft Delete).
- *Why?* If Sarah deletes a receipt because she made a mistake, Mike's balance changes. We need an **Audit Log** (Table: `ActivityLogs`) that tracks *every* change.
- *UI:* Show a "History" tab on every expense so users can see if an item was edited or removed. This prevents arguments ("I didn't delete that!").

---

### Phase 7: The UI/UX Philosophy (Mobile-First)

Since this is a web app but used primarily on phones:

- **PWA (Progressive Web App):** Enable "Install App" functionality. Use a `manifest.json` and a Service Worker to cache the CSS/JS so it loads instantly.
- **Gesture Logic:**
  - Swipe Right on an expense -> Mark as "Settled."
  - Swipe Left -> "Add a note/photo receipt."
- **The "Settlement" Screen:** Show a green checkmark next to users who are "Paid Up." Show a red progress bar for those who owe money.

---

### Summary of the Tech Stack Recommendation

| Layer | Technology | Why? |
| :--- | :--- | :--- |
| **Frontend** | Next.js 14 (App Router) | Server-side rendering for SEO + API routes built-in. |
| **Mobile** | PWA + Capacitor | Allows you to deploy to the Apple/Google App Store later easily. |
| **Auth** | Clerk / Supabase Auth | Handles social logins (Google/Facebook) with ease. |
| **Database** | PostgreSQL (Supabase/Neon) | Excellent for JSONB storage and complex join queries for balancing. |
| **Queue** | Redis (Upstash) | For handling the OCR job queue. |
| **OCR** | Google Cloud Vision API | Highest accuracy for structured text (receipts). |
| **Payments** | Stripe Connect | Handles the "Bank Transfer" piece and takes a tiny fee. |

### Final Pro-Tip for the Demo:
When presenting this, don't just show the code. **Bring a crumpled receipt** to the demo. Scan it live. Show the "Review" screen, then show the "Algorithm" screen where it suggests "Mike pays Sarah $12" instead of "Mike pays Sarah $5, John pays Sarah $7." **That visual graph is what gets you the "A+"** .
