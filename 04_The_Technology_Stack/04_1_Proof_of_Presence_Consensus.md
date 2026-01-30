# 04.1 Proof of Presence (PoP): The Consensus Mechanism

**Focus:** Technical Architecture & Fraud Prevention  
**Key Concept:** Multi-Factor Physical Authentication (MFPA)

---

## The "Unfakeable" Standard

In a digital world, "identity" is a private key. In the physical world, "presence" is a convergence of signals.

To verify a user is truly at a location—and not sitting on a couch in another country using a GPS spoofer—Zusei utilizes a **Tri-Layer Consensus Model**. A "Proof of Presence" is only valid if all three layers achieve consensus within a <500ms window.

### Layer 1: The Macro-Spatial Lock (GPS Geofence)
* **The Check:** The user's device queries the GNSS (Global Navigation Satellite System).
* **The Logic:** The app validates if the user's coordinates fall within a varying radius (e.g., 30m–50m) of the Merchant's registered "Energy Node" coordinates.
* **The Limitation:** GPS is prone to "drift" indoors and can be easily spoofed by developer tools on Android/iOS. Therefore, GPS is treated as a **necessary but insufficient** condition. It acts as the initial filter.

### Layer 2: The Micro-Environmental Lock (Signal Fingerprinting)
To prove the user is *inside* the building (not just standing on the street corner), Zusei analyzes the ambient electromagnetic environment.
* **Wi-Fi RSSI (Received Signal Strength Indicator):** The app scans for the Merchant's specific Wi-Fi BSSID (MAC address). It measures the signal strength (e.g., > -60 dBm implies close proximity).
* **Bluetooth Beacons:** If the ZConnect hardware is BLE-enabled, we perform a cryptographic handshake over Bluetooth.
* **The Defense:** This prevents "Drive-By Mining" where a user drives past a shop to collect rewards without entering. If the signal profile doesn't match an "indoor" signature, the Proof is rejected.

### Layer 3: The Cryptographic Handshake (Dynamic QR)
This is the core security layer, powered by the **ZConnect** node.
* **Static QR vs. Dynamic QR:** A printed paper QR code is insecure; it can be photographed and shared online. Zusei requires a **Time-Based One-Time Password (TOTP)** displayed on a screen (e.g., an iPad, POS screen, or dedicated e-ink device).
* **The Mechanism:**
    1.  The ZConnect Node generates a new QR code every 15 seconds.
    2.  **Payload:** `TokenID + Timestamp + Nonce + Merchant_Private_Key_Signature`.
    3.  **The Interaction:** The user scans this code. The app bundles this "Fresh Token" with the GPS and RSSI data.
* **The Security:** Because the QR code expires in 15 seconds, a photo of the code sent to a friend remotely will fail validation by the time they try to scan it.

---

## The Consensus Algorithm

The `zusei_ai_engine.py` processes these three inputs to calculate a **Presence Confidence Score (0.0 - 1.0)**.

$$Score = (GPS_{weight} \times 0.2) + (RSSI_{weight} \times 0.3) + (QR_{validity} \times 0.5)$$

* **Threshold:** Only a Score of **> 0.95** triggers the Smart Contract to mint $Z-WATT.
* **Rejection:** If the QR is valid but the GPS is 5km away (Remote Scanning Attack), the Score drops to 0.5 and the transaction is flagged as fraud.

---

## Attack Vector Mitigation

| Attack Type | The Zusei Defense |
| :--- | :--- |
| **GPS Spoofing** | Fails Layer 2 (Wi-Fi/BLE mismatch) and Layer 3 (Cannot see screen). |
| **Photo Replay** | Fails Layer 3 (TOTP expires in 15s). |
| **Sybil (Bot Farm)** | Fails Physics. One device cannot be in multiple geofences simultaneously. |
| **Man-in-the-Middle** | All payloads are signed by the Merchant's Private Key stored in the secure element of the ZConnect node. |

---

## Conclusion

Zusei's Proof of Presence is not just "location tracking." It is a **Cryptographic Proof of Physics.** By combining satellite data, local radio waves, and a time-locked visual handshake, we ensure that every $Z-WATT minted represents a genuine human interaction with the physical energy grid.
