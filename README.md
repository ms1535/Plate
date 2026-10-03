# Plate — calorie and weight tracker

A web app for Michael and Brittany. It installs to the home screen on Android and iPhone, works offline, and syncs through Firebase.

## What's in the folder

| File | Purpose |
|---|---|
| `index.html` | The app |
| `config.js` | Firebase and USDA settings (the only file you edit) |
| `sw.js`, `manifest.webmanifest`, `icons/` | Offline support and home-screen install |
| `firestore.rules` | Database security rules (pasted into Firebase, not hosted) |

With `config.js` left blank, the app runs in **demo mode**: everything works, but data stays on that one device.

## Step 1: Firebase (about 10 minutes)

1. Go to https://console.firebase.google.com and sign in with your Google account.
2. **Create a project.** Name it `plate`. Google Analytics can be turned off.
3. **Add a web app.** On the project home page, click the `</>` icon. Name it `plate`. Skip Firebase Hosting.
4. Firebase shows a `firebaseConfig` block. Copy the six values into `config.js`, or send the block to Claude.
5. **Turn on sign-in.** Build → Authentication → Get started → Sign-in method → **Email/Password** → Enable → Save.
6. **Create the database.** Build → Firestore Database → Create database → choose a US location → **Start in production mode**.
7. **Set the rules.** In Firestore, open the **Rules** tab. Replace everything with the contents of `firestore.rules` → Publish.

The free (Spark) plan covers two people easily. No credit card is needed.

## Step 2: USDA key (2 minutes, recommended)

The built-in `DEMO_KEY` allows about 30 searches per hour, shared by everyone on your Wi-Fi.

1. Go to https://api.data.gov/signup/ and enter your name and email.
2. The key arrives by email. Put it in `config.js` as `usdaApiKey`.

## Step 3: Host it on GitHub Pages

1. Create a new repository on GitHub, for example `plate`. It can be public; `config.js` values are not secret (the database rules protect your data).
2. Upload every file and the `icons` folder.
3. Settings → Pages → Source: **Deploy from a branch** → `main` / root → Save.
4. After about a minute, the app is at `https://<your-username>.github.io/plate/`.
5. In Firebase: Authentication → Settings → **Authorized domains** → Add domain → `<your-username>.github.io`.

## Step 4: Install on each phone

- **Android (Chrome):** open the link → ⋮ menu → **Install app**.
- **iPhone (Safari):** open the link → Share → **Add to Home Screen**. It must be Safari for the install and camera to work.

## Step 5: Link your accounts

1. Each person creates an account with their own email and password.
2. Michael: Profile tab → Household → **Share code**.
3. Brittany: Profile tab → **Join someone else's household** → enter the code.

After that, custom foods, saved meals, and recipes are shared. Food logs, weights, and calorie goals stay private to each person.

## How the calorie goal works

- Resting burn uses the Mifflin-St Jeor formula (sex, age, height, latest weight).
- Daily burn = resting burn × activity level.
- Goal = daily burn − 500 calories per pound per week of loss.
- The goal never goes below 1,500 (men) or 1,200 (women). A manual goal in the Profile tab overrides the formula.
- Macro targets use a 30% protein / 40% carbs / 30% fat split.

## Updating the app

Replace `index.html` in the GitHub repo. Phones pick up the new version the next time the app is opened with a connection. If an update ever seems stuck, change `plate-v1` to `plate-v2` in `sw.js`.

## Food data sources

- USDA FoodData Central: whole foods and US branded products.
- Open Food Facts: packaged foods and barcode lookups. If a barcode isn't found, create it once from the label; after that, scanning finds it for both of you.
