# 04.2 ZConnect Hardware: The Merchant Node

**Focus:** The Physical-Digital Bridge  
**Key Component:** Dynamic QR Generation

---

## The Hardware Philosophy: "Zero Friction"

For Zusei to scale to thousands of merchants in Singapore, we cannot require expensive, proprietary mining rigs. The **ZConnect Node** is designed to be hardware-agnostic, running on devices merchants already own or low-cost IoT modules.

We offer two tiers of hardware integration:

### Tier 1: The Software Node (Lite)
**Target:** Small F&B, Pop-up Stores  
**Hardware:** Any existing iPad, Android Tablet, or POS Screen.  
**Mechanism:** The merchant runs the Zusei Merchant App (or a web-wrapper pointing) which turns their screen into a **Dynamic Verification Terminal.**

### Tier 2: The IoT Node (Pro)
**Target:** High-Volume Retail, Chains, Franchise Outlets  
**Hardware:** A dedicated, low-power IoT device (ESP32 or Raspberry Pi) with an e-ink or OLED display.  
**Mechanism:** Hardwired to the shop’s power supply, this device runs a stripped-down version of the Zusei client. It broadcasts **Bluetooth (BLE) beacons** alongside the visual QR code for dual-layer verification.

---

## The Engine: `print_qr.php`

The core security of the ZConnect Node is powered by a lightweight, server-synced script. This script is responsible for generating the **Time-Based One-Time Password (TOTP)** that users scan.

### 1. The Generation Logic
Unlike a static URL (e.g., `zusei.com/shop/starbucks`), the ZConnect QR code is a cryptographic puzzle that changes every 15 seconds.

**The Algorithm:**
```php
$timestamp = floor(time() / 15); // 15-second windows
$nonce = random_bytes(8); 
$payload = hash_hmac('sha256', $merchant_id . $timestamp . $nonce, $secret_key);
$qr_content = "zusei://" . $merchant_id . "/" . $payload . "?ts=" . $timestamp;
