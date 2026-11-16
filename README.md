<div align="center">

<img src="./docs/Ilmxchange logo.png" width="120" alt="IlmXchange Logo"/>

# IlmXchange
### Peer-to-Peer Study Material Marketplace for University Students

![Kotlin](https://img.shields.io/badge/Kotlin-Native_Android-7F52FF?style=for-the-badge&logo=kotlin&logoColor=white)
![Jetpack Compose](https://img.shields.io/badge/Jetpack_Compose-Material_3-4285F4?style=for-the-badge&logo=jetpackcompose&logoColor=white)
![Firebase](https://img.shields.io/badge/Firebase-Auth_%2B_Firestore_%2B_FCM-FFCA28?style=for-the-badge&logo=firebase&logoColor=black)
![Supabase](https://img.shields.io/badge/Supabase-Storage_%2B_Edge_Functions-3ECF8E?style=for-the-badge&logo=supabase&logoColor=white)
![Hilt](https://img.shields.io/badge/Hilt-Dependency_Injection-34A853?style=for-the-badge&logo=android&logoColor=white)

*Final Year Project · Upload study material, earn IlmCoins, unlock what you need. No real money involved.*

</div>

---

## 🎯 The Problem

Students already share notes, past papers and slides, but the way they do it is broken:

- **Sharing is scattered.** Material lives in chat groups and photo galleries, and is lost within weeks
- **There is no quality signal.** Nobody knows whether a set of notes is accurate before spending time on it
- **Contributors get nothing back.** The students who write the best notes have no incentive to share them
- **Discovery is manual.** Finding the right past paper for the right subject means asking around

**IlmXchange** replaces that with one marketplace. Students upload study material, earn **IlmCoins** when others unlock it, and spend those coins on material they need. Every document can be previewed before it is unlocked, and every purchase can be rated.

---

## 📸 Screenshots

<table align="center">
  <tr>
    <td align="center"><img src="./docs/screenshots/dark mode.jpeg" width="180"/><br/><sub>🌙 Dark Mode</sub></td>
    <td align="center"><img src="./docs/screenshots/leaderboard.jpeg" width="180"/><br/><sub>🏆 Top Contributors</sub></td>
    <td align="center"><img src="./docs/screenshots/my profile.jpeg" width="180"/><br/><sub>👤 Profile · Appearance</sub></td>
  </tr> 
  <tr>
    <td align="center"><img src="./docs/screenshots/search screen.jpeg" width="180"/><br/><sub>🔍 Search · Filter · Sort</sub></td>
    <td align="center"><img src="./docs/screenshots/my upload.jpeg" width="180"/><br/><sub>⬆️ Upload Materials</sub></td>
    <td align="center"><img src="./docs/screenshots/uploads material screen.jpeg" width="180"/><br/><sub>👁️ Preview Before You Buy</sub></td>
  </tr>
  <tr>
    <td align="center"><img src="./docs/screenshots/upload screen.jpeg" width="180"/><br/><sub>📄 Material Detail</sub></td>
    <td align="center"><img src="./docs/screenshots/wallet.jpeg" width="180"/><br/><sub>🪙 Wallet · Recent Activity</sub></td>
    
  </tr>
</table>

---

## ⚡ The Core Flows

**Contribute**

```
Student picks a PDF (max 20 MB) and fills in title, type, subject, price
          ↓
App renders the first page on-device as the thumbnail (or uses a custom cover)
          ↓
File is uploaded to Supabase Storage under the student's own folder
          ↓
Material metadata is written to Firestore
          ↓
Student earns +10 IlmCoins, and an Edge Function sends a push notification
```

**Acquire**

```
Student finds material in the feed or search
          ↓
Previews sample pages (watermarked) and reads ratings and reviews
          ↓
Unlocks with IlmCoins
          ↓
Uploader receives 85% of the price, and a transaction is recorded for both sides
          ↓
Student gets full access and can rate the material
```

---

## 🪙 The IlmCoin Economy

IlmCoins are a closed-loop in-app currency. They **cannot be bought, withdrawn, or exchanged for money**.

| Event | Coins |
|---|---|
| Sign up (welcome bonus) | **+50** |
| Upload a study material | **+10** |
| Someone unlocks your material | **+85% of its price** |
| Unlock someone else's material | **−price** (5 to 100 coins, or free) |

Rankings on the leaderboard are based on coin balance.

---

## 🛠️ Features

| # | Feature | What the student does | What the app does |
|---|---|---|---|
| 1 | **Authentication** | Signs up with email and password | Firebase Auth with email verification, password reset and account deletion |
| 2 | **Home Feed** | Browses recommended material | Category chips: Notes, Past Papers, Exam Guides, Summaries, Books, Slides |
| 3 | **Search** | Types a query and picks filters | Sorts by Newest, Top Rated, Lowest Price or Most Popular |
| 4 | **Preview** | Opens a sample before paying | Shows watermarked sample pages and locks the rest |
| 5 | **Unlock** | Spends IlmCoins | Deducts coins, credits the uploader, records the unlock |
| 6 | **Upload** | Selects a PDF and optional cover | Validates size, renders a thumbnail, uploads, publishes |
| 7 | **Ratings & Reviews** | Rates 1 to 5 stars with an optional comment | Updates the material's average rating and review list |
| 8 | **Wallet** | Checks balance and history | Lists every credit and debit with timestamps |
| 9 | **My Uploads** | Reviews own publications | Shows uploads, downloads and earnings, with delete support |
| 10 | **Leaderboard** | Checks rank | Podium for the top three, your position, coins to the next rank |
| 11 | **Push Notifications** | Enables alerts in Settings | Edge Function sends an FCM notification when new material is uploaded |
| 12 | **Profile & Settings** | Edits profile, switches theme | Light, Dark or System appearance, password change, cache clearing |

---

## ✨ Feature Details

### 🔍 Preview Before You Buy
Every material shows watermarked sample pages before any coins are spent, together with its rating, page count, file type and uploader. A material with no reviews yet is labelled **New** instead of showing a misleading score.

### ⬆️ Upload Pipeline
Uploads are validated before anything leaves the device: PDF only, 20 MB maximum, a title, a type and a subject are required. If no cover is supplied, the first page of the PDF is rendered on-device and used as the thumbnail. Custom covers accept JPG or PNG up to 5 MB, in 3:4 or 16:9.

### 🪙 Wallet
The wallet shows the live coin balance and a transaction history that distinguishes welcome bonuses, upload earnings and purchases. A built-in guide explains every way to earn coins.

### 🏆 Leaderboard
Top contributors are ranked by coins. The screen shows a podium, the full ranked list, summary statistics, and how many coins the student needs to reach the next rank. Empty podium positions are shown as open spots.

### 🌙 Theming
The whole app supports Light, Dark and System modes, switchable from the Profile screen. The theme sets every Material 3 colour role explicitly, so no default colours leak into menus, dialogs or pressed states.

---

## 🏗️ Architecture

**Pattern:** MVVM with a Repository layer and Hilt dependency injection
**UI:** Jetpack Compose, single activity, Navigation Compose
**State:** `StateFlow` in ViewModels, collected with `collectAsStateWithLifecycle`

```
app/
└── java/com/ilmxchange/app/
    │
    ├── IlmXchangeApp.kt                    # Hilt application class
    ├── MainActivity.kt                     # Single activity, theme controller
    │
    ├── common/
    │   ├── navigation/NavRoutes.kt         # All route definitions
    │   └── util/
    │       ├── PdfThumbnailGenerator.kt    # First-page thumbnail from a PDF
    │       └── Resource.kt                 # Loading / Success / Error wrapper
    │
    ├── data/
    │   ├── model/                          # Material, Review, TransactionRecord, UserProfile
    │   ├── remote/FirebaseDataSource.kt
    │   └── repository/
    │       ├── AuthRepository.kt
    │       └── MaterialRepository.kt       # Materials, unlocks, ratings, wallet
    │
    ├── domain/
    │   ├── pricing/PricingCalculator.kt
    │   └── usecase/GetMaterialUseCase.kt
    │
    ├── di/                                 # Hilt modules: App, Firebase, Repository, Supabase
    │
    ├── notifications/FcmService.kt         # Push notification receiver
    │
    └── ui/
        ├── components/BrandComponents.kt   # Dark header, coin badge, glass bottom nav
        ├── theme/                          # Color, Theme, Type, Shape, ThemeMode
        ├── screens/                        # Splash, Onboarding, Login, SignUp, HomeFeed,
        │                                   # Search, MaterialDetail, PreviewViewer, Rating,
        │                                   # UploadMaterial, MyUploads, Wallet, Leaderboard,
        │                                   # Profile, Settings, Terms, Privacy, NavigationRoot
        └── viewmodels/                     # One ViewModel per screen

supabase/
└── functions/
    └── notify-upload/index.ts              # Edge Function: push notification on new upload
```

---

## ⚙️ Key Engineering Decisions

### 1. A Split Backend, Chosen for Cost
Firebase Storage and Cloud Functions require the paid Blaze plan, so they are not used. **Firebase** handles Authentication and Firestore, while **Supabase** handles file storage and Edge Functions. The project runs entirely on free tiers.

### 2. Trust Through Previews and Ratings, Not Moderation
There is no in-app moderation team. Quality is controlled by letting students inspect material before paying and rate it afterwards, so poor material loses visibility on its own.

### 3. A Closed-Loop Currency
IlmCoins have no cash value and cannot be bought. That removes payment processing and financial regulation from the project, and keeps the focus on exchanging knowledge. Abuse such as creating multiple accounts to farm rewards is prohibited in the Terms of Service.

### 4. Real Thumbnails
Feed cards show the actual first page of the uploaded document instead of a stock placeholder, generated on-device by `PdfThumbnailGenerator` unless the uploader supplies a cover.

### 5. One Design System
`BrandComponents.kt` holds the shared dark header, coin badge and glass-effect bottom navigation, and `Theme.kt` defines the full palette for light and dark modes. Screens use theme roles instead of hardcoded colours, so a palette change is a one-file change.

### 6. Honest Empty and Edge States
New materials show "New" instead of a rating, empty leaderboards show open podium spots, and every list has an explicit empty state, so the UI never displays numbers the data does not support.

---

## 🎨 Design System

| Role | Colour | Used for |
|---|---|---|
| **Primary** (Deep Pine) | `#0C3B2E` | Headers, primary buttons, key actions |
| **Secondary** (Sage) | `#6D9773` | Chips, borders, secondary surfaces |
| **Tertiary** (Amber) | `#B46617` | Accents and badges |
| **Reward** (Gold) | `#FFBA00` | Coins, star ratings and rewards only |
| **Canvas** | `#F8FAF8` | App background |

UI designs were prepared in Google Stitch and implemented screen by screen.

---

## 🛠️ Tech Stack

| Layer | Technology | Reason |
|---|---|---|
| Language | Kotlin 2.3.20 | Modern Android standard |
| UI | Jetpack Compose (BOM 2026.06.00) + Material 3 | Declarative UI with a single source of truth |
| Architecture | MVVM + Repository | Clean separation, testable code |
| Dependency Injection | Hilt 2.57.1 with KSP | Compile-time DI |
| Authentication | Firebase Authentication | Email and password with verification |
| Database | Cloud Firestore | Materials, users, ratings, transactions |
| File Storage | Supabase Storage | Documents and cover images |
| Serverless | Supabase Edge Functions (TypeScript) | Upload notification fan-out |
| Notifications | Firebase Cloud Messaging | Push alerts |
| Images | Coil | Async image loading |
| Glass Effect | Haze | Blurred bottom navigation bar |
| Navigation | Navigation Compose | Single-activity navigation |
| Build | AGP 8.6.0, Gradle 8.13, TOML version catalogs | Reproducible builds |

---

## 🗃️ Data Model

| Collection | Purpose |
|---|---|
| `users/{uid}` | Profile, university, bio, coin balance |
| `users/{uid}/transactions` | Per-user wallet history |
| `users/{uid}/unlocked_materials` | Materials the user has unlocked |
| `materials` | Published study material and its metadata |
| `ratings` | Reviews, keyed by `materialId` |

**Required composite indexes** (Firestore will print a creation link in Logcat if one is missing):

| Collection | Fields |
|---|---|
| `materials` | `authorId` ascending, `timestamp` descending |
| `ratings` | `materialId` ascending, `timestamp` descending |

---

## 🚀 Getting Started

**Prerequisites:** Android Studio Koala or later, JDK 17.

```bash
git clone https://github.com/AreejFatima33/ilmxchange.git
cd ilmxchange
```

### 1. Firebase
1. Create a Firebase project and enable **Email/Password** authentication, **Firestore** and **Cloud Messaging**
2. Download `google-services.json` and place it in `app/`
3. Publish the Firestore security rules and create the indexes listed above

### 2. Supabase
1. Create a Supabase project and a Storage bucket (the bucket name is configured in `SupabaseModule.kt`)
2. Add storage policies: authenticated read, and owner-only write scoped to the user's own folder
3. Add your credentials to `local.properties`:

```properties
SUPABASE_URL=https://<your-project-ref>.supabase.co
SUPABASE_ANON_KEY=<your-anon-key>
```

### 3. Push notifications (optional)
```bash
cd supabase
supabase login
supabase link --project-ref <your-project-ref>
supabase secrets set FCM_SERVICE_ACCOUNT_JSON='<service account JSON on one line>'
supabase functions deploy notify-upload
```

### 4. Build
```bash
./gradlew assembleDebug
```

**Minimum SDK:** TODO · **Target SDK:** Android 15 (API 35)

> ⚠️ Never commit `google-services.json`, `local.properties` or a Firebase service account file. They are listed in `.gitignore`.

---

## 🗺️ Roadmap

- [ ] Server-side coin transactions for purchases and rewards
- [ ] Saved Material (bookmarks)
- [ ] In-app camera capture for cover photos
- [ ] Offline reading of unlocked material
- [ ] Plus Jakarta Sans typography from the design system
- [ ] Automated tests for repositories and ViewModels

---

## 👩‍💻 Developer

**Areej Fatima** · Flutter & Android Developer

[![Portfolio](https://img.shields.io/badge/Portfolio-Visit-F0A500?style=flat-square)](https://areejfatima33.github.io/areej-portfolio/)
[![LinkedIn](https://img.shields.io/badge/LinkedIn-Connect-0A66C2?style=flat-square&logo=linkedin)](https://www.linkedin.com/in/areej-dev01/)
[![GitHub](https://img.shields.io/badge/GitHub-Follow-181717?style=flat-square&logo=github)](https://github.com/AreejFatima33)
[![Play Store](https://img.shields.io/badge/Play_Store-See_Apps-34A853?style=flat-square&logo=google-play)](https://play.google.com/store/apps/developer?id=Areexa+Studios)

---

<div align="center">
  <sub>Built as Final Year Project · Native Android · Kotlin · Jetpack Compose</sub>
</div>
