# 04.1 Proof of Presence (PoP): The Consensus Mechanism

**Focus:** Technical Architecture & Fraud Prevention  
**Key Concept:** Tri-Layer Physical Consensus

---

## The "Unfakeable" Standard

In the digital world, "identity" is a private key. In the physical world, "presence" is a convergence of signals.

To verify a user is truly at a location—and not sitting on a couch in another country using a GPS spoofer—Zusei utilizes a **Tri-Layer Consensus Model**. A "Proof of Presence" block is only mined if all three layers achieve consensus within a <500ms window.



### Layer 1: The Macro-Spatial Lock (Satellite)
* **The Check:** The Zusei Client queries the GNSS (Global Navigation Satellite System).
* **The Logic:** The app validates if the user's coordinates fall within the registered geofence radius (e.g., 30m–50m) of the Node.
* **The Role:** This acts as the **"Coarse Filter."** It filters out 90% of lazy remote attacks but is treated as insufficient on its own due to standard GPS drift and OS-level spoofing tools.

### Layer 2: The Micro-Environmental Lock (RF Signature)
To prove the user is *inside* the building (High-Intent) rather than standing on the street corner (Low-Intent), Zusei analyzes the ambient electromagnetic environment.
* **Wi-Fi RSSI (Signal Strength):** The app scans for the Merchant's specific BSSID (MAC address). It measures the signal attenuation (e.g., > -60 dBm implies <5m proximity).
* **BLE Handshake:** If the ZConnect hardware is present, a low-energy Bluetooth beacon creates a secondary proximity check.
* **The Defense:** This prevents **"Drive-By Mining"** (collecting rewards while driving past a shop). If the RF signature does not match the "Indoor Profile," the Proof is rejected.

### Layer 3: The Cryptographic Handshake (Hardware TOTP)
This is the "Ground Truth" layer, powered by the **ZConnect** hardware node.
* **Visual Cryptography:** We reject static QR codes (which can be photographed and shared). Zusei requires a **Time-Based One-Time Password (TOTP)** displayed on the Node's screen (OLED/iPad).
* **The Mechanism:**
    1.  The ZConnect Node generates a dynamic 2D barcode every 15 seconds.
    2.  **Payload:** `TokenID + Timestamp + Nonce + Node_Private_Key_Signature`.
    3.  **The Interaction:** The user scans this code. The app bundles this "Fresh Token" with the Layer 1 & 2 data and signs it with the User's Private Key.
* **The Security:** Because the token expires in 15 seconds, a photo sent to a bot farm remotely will fail validation due to network latency and expiration.

---

## The Consensus Algorithm

The `zusei_ai_engine.py` processes these three inputs to calculate a **Truth Confidence Score (0.0 - 1.0)** before writing to the ledger.

$$Score = (GPS_{weight} \times 0.2) + (RF_{weight} \times 0.3) + (Hardware_{validity} \times 0.5)$$

* **Mining Threshold:** Only a Score of **> 0.95** is accepted as "Ground Truth" to mint ZP.
* **Fraud Flagging:** If the QR is valid but the GPS is 5km away (Remote Scanning Attack), the Score drops to 0.5, the transaction is rejected, and the user's "Trust Score" is penalized.

---

## Attack Vector Mitigation

| Attack Type | The Zusei Defense |
| :--- | :--- |
| **GPS Spoofing** | Fails Layer 2 (RF Mismatch) and Layer 3 (Cannot see the screen). |
| **Photo Replay** | Fails Layer 3 (TOTP expires in 15s). |
| **Sybil (Bot Farm)** | Fails Physics. A single device cannot be in two geofences simultaneously. |
| **Man-in-the-Middle** | All payloads are signed by the Node's Private Key stored in the Secure Element (SE). |

---

## Conclusion

Zusei's Proof of Presence is not "location tracking." It is a **Cryptographic Proof of Physics.**

By combining satellite telemetry, local radio waves, and a time-locked hardware handshake, we ensure that every unit of data sold to an AI Agent represents a genuine, unfakeable human interaction with the physical world.
