# MP Manager (MPM) Official Web Platform & Billing Gateway

The official multi-page web portal, product showcase, documentation, download center, and automated Paddle checkout gateway for **MP Manager (MPM)**. Deployed globally on Cloudflare Workers edge network.

---

## 🌐 Live Web Routes

| Route | Description |
| :--- | :--- |
| **`/`** or **`/home`** | Modern homepage featuring security stats, core capabilities, architecture overview, and download CTAs. |
| **`/features`** | Deep technical breakdown of AES-256-GCM encryption, 150,000-round PBKDF2-HMAC derivation, Emergency Guardian Matrix, and P2P Chat Relay. |
| **`/pricing`** | Full pricing tiers (Free $0, Pro $2/mo, Ultra $4/mo, Family $7/mo) with web-checkout modal and in-app upgrade details. |
| **`/download`** | Download hub for Windows 10 & 11 (64-bit AMD64), SHA-256 checksum verification command, system requirements, and Android APK roadmap. |
| **`/download/exe`** | Direct download redirect for `MP-Manager-Setup-1.4-amd64.exe`. |
| **`/faq`** | Interactive accordion FAQ covering cryptography, lost master password policies, multi-device sync, and Paddle subscriptions. |
| **`/support`** | Customer support contacts, documentation quick links, and app self-diagnostic telemetry instructions. |
| **`/privacy`** | Legal privacy policy tailored to zero-knowledge vault protection and Paddle Merchant of Record processing. |
| **`/terms`** | Terms of service and subscription renewal policies. |
| **`/checkout`** | Automated Paddle billing endpoint used by both the desktop application and web portal (`?email=...&plan=...`). |
| **`?_ptxn=...`** | Seamless Paddle checkout overlay and return handler. |

---

## ⚡ Paddle Payment Gateway & Streamlined Overlay

1. **Automated Transaction Creation:**
   - Both the MP Manager desktop application and the web pricing modal forward users to:
     ```
     https://mpm.ahmadqazii1188.workers.dev/checkout?email=user@example.com&plan=pro
     ```
   - The worker dynamically generates a Paddle transaction via the Paddle API (`/transactions`) with custom metadata (`app: mp_manager`, `user_email`, `plan_key`).

2. **Streamlined Checkout Overlay:**
   - When Paddle returns `_ptxn` in the URL, the page immediately initializes Paddle.js and opens the checkout modal overlay.
   - The fallback placeholder box ("Click Here if Checkout Doesn't Open") has been removed in favor of a sleek, dark background with an unobtrusive loading pulse that transitions instantly into the Paddle checkout.

3. **Success Page:**
   - Once payment is verified, customers are shown a verified confirmation screen advising them to return to the MP Manager desktop app and click **"🔄 Check Subscription Status"**.

---

## 🚀 Deployment & Configuration

### Prerequisites
- Node.js (v18+)
- Cloudflare Wrangler CLI:
  ```bash
  npm install -g wrangler
  ```

### Local Testing
```bash
npx wrangler dev
```

### Deploying to Cloudflare Workers
```bash
npx wrangler deploy
```

### Environment Secrets in Cloudflare
Set the Paddle API Key securely in Cloudflare:
```bash
npx wrangler secret put PADDLE_API_KEY
```
*(Optional) You can also set custom Price IDs via secrets:*
```bash
npx wrangler secret put PRICE_PRO
npx wrangler secret put PRICE_ULTRA
npx wrangler secret put PRICE_FAMILY
```

---

## 📁 Repository Structure

```
10 - MPM Web Official/
├── index.js          # Core Cloudflare Worker script: multi-page router, HTML page renderers, and Paddle API gateway
├── index.html        # Standalone, static HTML release of the website for offline or static hosting
├── wrangler.toml     # Cloudflare Worker configuration & environment definitions
└── README.md         # Repository documentation
```
