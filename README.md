# [FIX] SideStore 0.7.0 Nightly + LiveContainer Login Loop & ADI Errors Workaround

[![Ko-fi](https://img.shields.io/badge/Ko--fi-Support%20me-FF5E5B?style=for-the-badge&logo=ko-fi&logoColor=white)](https://ko-fi.com/owlockgta)
[![PayPal](https://img.shields.io/badge/PayPal-Donate-00457C?style=for-the-badge&logo=paypal&logoColor=white)](https://www.paypal.com/paypalme/owlockgta)

Comprehensive, step-by-step troubleshooting guide and maintained mirror for resolving the persistent Apple ID login loop and Anisette failures in **SideStore 0.7.0 Nightly** (standalone or bundled with **LiveContainer**) triggered after Apple's GrandSlam backend changes (November 20, 2026).

---

## 🛑 Errors Solved by This Fix

* `Failed to refresh apps. Your session has expired. Please log in. (-1)`
* `Invalid server response: unparseable format — 503 Service Temporarily Unavailable (Apple)`
* `HTTP error (401): Apple PTM/TK endpoint returned HTTP 401`
* `ADI native error (-45063): ADIProvisioningStart failed (Pending ADI session (-45063)): -45063`
* `Device Registration Error — VPN tunnel iface is up but tunnel peer IP is not yet reachable (Cause: DeviceEndpointNotInitialized)`
* `Cannot complete action. Try again. (-22410)` or `(-45061) not provisioned for this server`

> **Note on Standard Troubleshooting**: Routine actions like *Clear Data Cache*, *Clear Keychain Items*, switching Wi-Fi/cellular, or reinstalling via iLoader v2.4.2 do **not** clear stale On-Device Anisette, locked `adi.pb` files, or blocked `<macOS;...>` client headers. You do **not** need to wipe your SideStore database or uninstall apps.

---

## 🛠️ Step-by-Step Fix (Zero Data Loss)

### Step 1: Disable On-Device Anisette
1. Open **SideStore** -> **Settings** -> **ADVANCED SETTINGS** -> **User Customizations**.
2. Turn **OFF** `On-Device Anisette`.
3. Force-close LiveContainer / SideStore from the iOS App Switcher and re-open it.

### Step 2: Flush Stale `adi.pb` & Headers
1. Go to **Settings** -> **User Customizations** -> **Reset adi.pb**.
2. Uncheck `Keep header customizations` / `Keep Headers`.
3. Tap **Reset & Sign Out** and confirm (*Cleared adi.pb!*).

### Step 3: Pin a Clean Remote Anisette Server
1. Go to **Settings** -> **Anisette Servers**.
2. Select **jkcoxson** (`https://mac-ani.jkcoxson.com` or `https://mac-ani.sidestore.io`).
3. Scroll down and set **Enable Auto Rotation** to **OFF**.

### Step 4: Override Client Info (`<macOS;...>` to `<Mac OS X;...>`)
1. Go to **Settings** -> **User Customizations** -> **Anisette Client Configuration**.
2. Under **Remote Server Sync**, tap **Fetch** -> **Save as Overrides**.
3. Replace **Client Info** with:
   ```text
   <MacBookPro13,3> <Mac OS X;26.6;25F84> <com.apple.AuthKit/1 (com.apple.akd/1.0)>
   ```
4. Verify **User Agent** is:
   ```text
   akd/1.0 CFNetwork/811.5.4 Darwin/16.7.0
   ```
5. Tap **Save Overrides**.

### Step 5: Kill Stale Memory Session & Log In
1. Force-close LiveContainer / SideStore from the iOS App Switcher (this terminates the lingering `-45063` ADI session stuck in memory).
2. Re-open **SideStore** -> **Settings** -> **Sign in with Apple ID** and submit your 2FA code.

### Step 6: Bypass `DeviceEndpointNotInitialized`
1. Immediately after entering the 6-digit 2FA SMS code, if you see `Device Registration Error (Cause: DeviceEndpointNotInitialized)`, **DO NOT tap Cancel**.
2. Switch to **LocalDevVPN**, turn it **ON** (or toggle it OFF then ON to reinitialize the tunnel interface).
3. Switch back to SideStore and tap **Retry**. SideStore will instantly finish registration and load your account page.

### Step 7: Refresh Apps & Sync JIT-Less Certificate
1. Go to **My Apps** -> **Refresh All** (resets timer back to 7 DAYS).
2. If using LiveContainer: Go to **LiveContainer** -> **Settings** -> **JIT-Less** -> **Import/Update Certificate from SideStore**.

---

## ⚡ Follow-up Fix: Error `-22410` or `-45061`
If you complete 2FA but encounter error `-22410` (*Could not complete action*) or `-45061` (*Not provisioned for this server*):
1. In SideStore, go to **User Customizations** -> **Reset adi.pb**.
2. Tap **Reset & Sign Out** while keeping your custom headers intact.
3. Reboot your device and re-attempt signing in with `LocalDevVPN` active.

---

## ☕ Support My Work / Apoya mi trabajo

Everything I document and build is 100% free and open-source. If this guide saved you hours of troubleshooting or avoided having to wipe your apps:

* **Ko-fi (Cards / PayPal)**: [ko-fi.com/owlockgta](https://ko-fi.com/owlockgta) *(0% platform fees)*
* **PayPal**: [paypal.me/owlockgta](https://www.paypal.com/paypalme/owlockgta)
* **Crypto (Binance Pay / USDT - Zero Network Fees)**:
  * **Pay ID / User ID**: `1111269376` (Receiver: `Owlock`)
  * **Direct App Link**: [app.binance.com/uni-qr/TBFvSGNT](https://app.binance.com/uni-qr/TBFvSGNT)

---

## 🔗 Community & Upstream References
* **Reddit Discussions**:
  * [r/SideStore Viral Thread](https://www.reddit.com/r/SideStore/comments/1y340sh/fix_solved_sidestore_070_nightly_livecontainer/) (23k+ views)
  * [r/sideloaded Viral Thread](https://www.reddit.com/r/sideloaded/comments/1y343i0/fix_solved_sidestore_070_nightly_livecontainer/) (9.5k+ views)
* **Upstream SideStore Issue Tracker**: [SideStore Issue #1745](https://github.com/SideStore/SideStore/issues/1745#issuecomment-7088086915)
* Author: **[@Owlock](https://github.com/Owlock)** / [u/OwlockGta](https://www.reddit.com/user/OwlockGta/)
