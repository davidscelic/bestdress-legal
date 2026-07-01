# BestDress Privacy Policy

**Effective date:** June 29, 2026  
**App:** BestDress (iOS)  
**Bundle identifier:** `com.bestdress.ios` (as set in `ios/Runner.xcodeproj`)  
**Operator:** BestDress (contact information below)

This Privacy Policy describes how BestDress (“we,” “us,” or “our”) handles information when you use the BestDress mobile application.

---

## Summary

BestDress is a **local-first** wardrobe app. In the **default App Store build** (no cloud AI API key configured at compile time), your wardrobe photos, profile details, ratings, and occasions are stored **on your device only**. We do not operate a BestDress user account system, backend database, advertising network, analytics SDK, or crash-reporting SDK in the current codebase.

If a build is compiled with a cloud vision API key (`BESTDRESS_AI_KEY`), photos you choose to analyze may be transmitted to that configured provider for AI processing, as described below.

---

## 1. Information we process

### 1.1 Information you provide

Depending on how you use the app, you may enter or create:

| Data | Examples in app | Default build storage |
|------|-----------------|----------------------|
| Profile | First name, style direction, color and brand preferences, style goals | On device (`SharedPreferences` via `LocalStore`) |
| Sizing (optional) | Height, weight, waist, chest, inseam, top/bottom/dress/jacket/shoe sizes | On device |
| Wardrobe items | Names, categories, colors, materials, brands, notes, photos | On device; photos in app documents directory (`bestdress_images/`) |
| Outfit ratings | Outfit photos, occasion, dress code, theme, colors, accessories, notes, AI/mock results | On device |
| Saved occasions | Name, dress code, theme, colors, location (typed text), notes | On device |
| Avatar | Optional profile photo | On device |

**We do not collect email addresses, passwords, or phone numbers in the app.** There is no in-app login to a BestDress server.

### 1.2 Information generated on your device

The app maintains local-only helper data to improve suggestions:

- **Style memory** — counts of occasions, colors, and verdicts you save; default sizes/colors per category (`StyleMemory`, key `bd_style_memory`).
- **AI scan usage counters** — counts of successful cloud AI scans for quota enforcement (`AiScanUsage`, key `bd_ai_scan_usage`). These counters are **not** uploaded.
- **Subscription state** — trial start date and subscription tier (`SubscriptionState`, key `bd_subscription`).

### 1.3 Subscription and purchase information

In **release builds**, subscriptions are processed by **Apple** through **StoreKit** (`in_app_purchase` / `in_app_purchase_storekit`). BestDress:

- Displays products and prices from the App Store.
- Receives purchase and restore outcomes from Apple’s APIs.
- Stores subscription tier and trial status **locally** on your device.

BestDress **does not** receive or store your payment card number or full Apple ID. Payment is handled entirely by Apple. You can manage or cancel subscriptions at [Apple Subscriptions](https://apps.apple.com/account/subscriptions), which the app may open via `url_launcher`.

### 1.4 Photos and camera

BestDress requests permission to use your **camera** and **photo library** only when you choose to add or rate an outfit (`NSCameraUsageDescription`, `NSPhotoLibraryUsageDescription`, `NSPhotoLibraryAddUsageDescription` in `Info.plist`).

- Images you select are copied into the app’s private documents directory (`ImageService`).
- Images are shown inside the app for wardrobe and rating features.
- Images are resized on pick (max width 1600 px, quality 88) before storage.

---

## 2. AI photo processing

### 2.1 Default App Store build (no API key)

When BestDress is built **without** `BESTDRESS_AI_KEY`:

- Outfit rating uses the **on-device mock engine** (`MockRatingEngine`). It does **not** analyze image pixels.
- Closet item inference uses the **on-device mock** (`MockItemInferenceService`). It infers from filename keywords, your profile, and closet context—not from the image itself.
- **No photos or outfit data are sent to BestDress or third-party AI servers.**

### 2.2 Optional cloud vision builds (API key configured)

When a build is compiled **with** `--dart-define=BESTDRESS_AI_KEY=...`:

- **Outfit rating** (`CloudRatingService`) may send the outfit image (base64 JPEG in the request body) plus occasion context text to the configured endpoint.
- **Closet item inference** (`CloudItemInferenceService`) may send the item image plus a short text prompt to the same class of endpoint.
- Default endpoint: `https://api.openai.com/v1/chat/completions` (overridable via `BESTDRESS_AI_ENDPOINT`).
- Default model name: `gpt-4o-mini` (overridable via `BESTDRESS_AI_MODEL`).
- Transmission occurs **only** when you take an explicit rating or closet-scan action, not in the background.
- If the network call fails or quota is exceeded, the app falls back to the on-device mock and still produces a result.
- System prompts instruct the provider to analyze **clothing only**, not to identify people or make medical claims (`VisionPrompts`).

**You should assume the configured AI provider processes images and text you send according to that provider’s privacy policy.** BestDress does not operate that infrastructure in the default shipped configuration.

### 2.3 AI scan quotas

When cloud vision is enabled, the app enforces local scan limits (`AiScanQuotaService`) per trial/subscription plan. Usage counts are stored on device only.

---

## 3. What we do **not** use

Based on the current repository, BestDress does **not** integrate:

- Firebase, Crashlytics, or other Google analytics/crash SDKs  
- AdMob or other advertising SDKs  
- RevenueCat or third-party subscription SDKs (subscriptions use Apple StoreKit directly)  
- OpenAI, Gemini, or Claude SDKs as packaged dependencies (optional HTTP to a configurable endpoint only when a build-time key is set)  
- Supabase, AWS SDKs, or proprietary BestDress cloud storage  
- Push notification providers  
- Third-party authentication (Sign in with Apple, Google, etc.)  
- Cross-app or cross-site tracking / advertising identifiers  

The in-app Privacy screen states: *“BestDress does not include advertising, third-party tracking, or analytics in this version.”*

---

## 4. Data storage and retention

| Data | Location | Retention |
|------|----------|-----------|
| Profile, wardrobe, ratings, occasions, style memory, subscription snapshot, AI quota | Device (`SharedPreferences` + app documents) | Until you delete the app or tap **Erase** in the Me tab (`LocalStore.clearAll()`) |
| Wardrobe/rating photos | App documents directory | Until deleted with the item, erased via **Erase**, or app uninstall |
| Cloud AI images (key-enabled builds only) | Transmitted to configured provider | Governed by that provider; not retained on a BestDress server (none exists) |

We do not operate a centralized backup or sync service in the current app.

---

## 5. Data sharing

**Default build:** We do not share your data with third parties because data does not leave your device except:

- **Apple** — for in-app purchases and subscription management (Apple’s privacy policy applies).

**API-key builds:** Images and related text you submit for AI analysis are sent to the **configured vision API endpoint** (OpenAI-compatible by default). That provider is a third party for privacy purposes.

We do not sell personal information.

---

## 6. Your choices and rights

- **Erase all data:** Me → **Erase** removes profile, closet, looks, occasions, subscription snapshot, style memory, and AI quota from the device.
- **Permissions:** You can deny camera or photo access in iOS Settings; the app degrades gracefully.
- **Subscriptions:** Manage or cancel through Apple’s subscription settings.

### 6.1 GDPR (EEA/UK users)

Where GDPR applies, our legal bases are typically:

- **Contract / app functionality** — providing wardrobe and outfit-rating features you request.
- **Consent** — camera/photo permissions and optional profile fields you choose to enter.

Because the default app stores data locally and we do not operate a BestDress account system, many GDPR requests (access, deletion) can be fulfilled by using **Erase** or uninstalling the app. For API-key builds, contact us regarding provider-side processing.

### 6.2 CCPA/CPRA (California residents)

In the default configuration we do not “sell” or “share” personal information as defined by the CCPA. We do not use data for cross-context behavioral advertising. California residents may contact us with privacy requests (see Section 10).

---

## 7. Children’s privacy

BestDress is not directed at children under 13 (or the applicable age in your jurisdiction). We do not knowingly collect personal information from children. The app does not implement an age gate. If you believe a child has provided information through the app, contact us and delete the app data via **Erase** or uninstall.

---

## 8. Security

Data in the default build remains on your device, protected by your device’s operating system security. API keys, when used, are embedded at build time (`String.fromEnvironment`); they are not entered by end users in the app UI.

No method of transmission or storage is 100% secure. Cloud AI builds depend on HTTPS to the configured endpoint.

---

## 9. International transfers

The default app does not transfer data internationally because data stays on your device.

API-key builds may transfer images and text to servers operated by the configured provider, which may be located outside your country.

---

## 10. Changes to this policy

We may update this Privacy Policy. The effective date at the top will change when we do. Continued use after changes constitutes acceptance of the updated policy.

---

## 11. Contact

The app references **https://bestdress.app** for legal pages (`AppConstants.privacyUrl`, `AppConstants.termsUrl`).

For privacy questions, use the contact method published on **https://bestdress.app** or open an issue in the BestDress project repository if you obtained the app through a developer distribution channel.

---

## 12. Platform-specific notes

### Apple App Store

- Privacy nutrition labels should match your **actual shipped build** (default local-only vs. API-key cloud vision).
- Privacy Policy URL in App Store Connect must be publicly accessible. A hosted copy of this document satisfies that requirement.

### Android

An Android project exists in the repository, but the product README describes BestDress as an **iPhone** app. Android manifests in the repo do not declare camera/storage permissions in the main manifest; behavior on Android may differ if distributed.

---

*This policy was generated from a source-code audit of the BestDress repository as of June 29, 2026.*
