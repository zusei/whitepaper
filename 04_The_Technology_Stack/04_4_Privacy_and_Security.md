# 04.4 Privacy and Security: The Zero-Knowledge Standard

**Focus:** User Anonymity & Data Sovereignty  

---

## The "Surveillance" Problem

Traditional location apps (like Google Maps or Foursquare) operate on a **Data Extraction Model**. You give them your location; they sell your profile to advertisers. They know *who* you are, *where* you are, and *what* you buy.

Zusei operates on a **Data Verification Model**. We need to know *that* a human is present, but we do not need to know *who* that human is.

We achieve this through a **Zero-Knowledge (ZK)** architecture that uncouples **Identity** from **Activity**.



---

## The Identity Layer: Powered by Wallet

To onboard non-crypto natives (the "Chicken Rice Uncle" or the "Orchard Road Shopper"), we cannot ask them to write down a 12-word seed phrase.

We utilize **Embedded Wallet** infrastructure to create an invisible, non-custodial identity layer.

### 1. Frictionless Onboarding
* **The User Action:** Signs in with a phone number (SMS) or Email.
* **The Wallet Magic:** Privy uses **Shamir’s Secret Sharing (SSS)** to split the user's private key into multiple "shares."
    * *Share A:* Stored on the User's device.
    * *Share B:* Stored securely by Wallet.
* **The Result:** The user gets a fully functional Web3 wallet (to hold $Z-WATTs) without ever seeing a private key. It feels like a Web2 login, but functions like a self-custodial vault.

### 2. The Firewalled Identity
Crucially, the **Merchant never sees the User's login details.**
* When you scan a QR code at a cafe, the Merchant receives a wallet address hash (e.g., `0x7a...B2`), **not** your email (`john@gmail.com`) or phone number.
* This prevents merchants from building spam lists or harvesting personal data for retargeting.

---

## The "Bar Bouncer" Analogy (Understanding ZK-Proofs)

To explain our privacy model, consider the difference between showing your **ID Card** vs. using **Zusei**.

| Scenario | The Action | What is Revealed? | Privacy Level |
| :--- | :--- | :--- | :--- |
| **Traditional ID** | You hand your ID card to a bouncer to prove age. | Name, Address, Date of Birth, ID Number. | **Zero Privacy** (Over-sharing). |
| **Zusei (ZK-Proof)** | You scan the ZConnect node. | "Is this user physically here? **YES**." | **Total Privacy** (Minimal Verification). |

Zusei proves the **fact** (Presence) without revealing the **data** (Identity).

---

## What the Merchant Sees (The Dashboard)

We provide merchants with **Aggregated Intelligence**, not individual surveillance.

A merchant dashboard looks like this:
* **✅ Live Metrics:** "Current Occupancy: 42 Pax."
* **✅ Efficiency:** "EEV Score: High (Good)."
* **✅ Anonymized Loyalty:** "Visitor #8821 has visited 5 times this month."
* **❌ Personal Data:** No names, no emails, no home addresses.

This ensures that merchants get the **economic value** of the data (footfall analytics) without the **liability** of holding sensitive personal information (PDPA/GDPR compliance).

---

## Data Sovereignty

In the Zusei protocol, the user owns their "Presence Graph."
* **The Graph:** A history of all the places you have verified.
* **The Power:** In the future, a user can choose to *opt-in* to share this data with specific brands for extra rewards.
    * *Example:* "Share my coffee shop history with Starbucks to get a free drink?" -> **User clicks YES.**
* **Default State:** The default state is always **Private**. Data never leaves the user's wallet unless they explicitly sign a permission request.

## Conclusion

Zusei is not a tracking app; it is a **Proof-of-Humanity** app. By using Privy for invisible keys and Zero-Knowledge logic for verification, we build a Human Energy Grid that respects the human behind the data.
