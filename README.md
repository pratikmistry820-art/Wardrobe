# Wardrobe — your own free, shareable wardrobe app

This folder is the complete app. Put it online once (free) and anyone can use it from a link — on Android, iPhone or a computer — with no app store, no Claude and no subscription. Each person signs in with their own Google account, and their clothes and photos are saved in a private, hidden app folder in **their own Google Drive**. You (the person who hosts it) can't see anyone's data.

**Cost:** ₹0. GitHub Pages hosting is free, the Google Cloud project is free, and each user's data uses their own Google Drive storage (15 GB free; about 0.3 MB per photo).

**Time needed once:** about 25–30 minutes, on a computer.

In the steps below, replace `YOUR-USERNAME` with your GitHub username. The examples assume you name the repository `wardrobe`.

---

## Part 1 — Put the app online with GitHub Pages (≈10 min)

1. Create a free account at **github.com/signup** (skip if you already have one).
2. Top-right **+** → **New repository**.
   - Repository name: `wardrobe`
   - Visibility: **Public** (free GitHub Pages needs a public repository — only the app's code is public; nobody's wardrobe data is ever stored here).
   - Tick **Add a README file** → **Create repository**.
3. On your computer, unzip `wardrobe-app.zip`. In the repository page click **Add file → Upload files** and drag in **all the files inside the folder** (`index.html`, `config.js`, `sw.js`, `manifest.webmanifest`, `privacy.html`, `README.md` and the `.png` icons) — the files themselves, not the folder. Click **Commit changes**.
4. Go to **Settings → Pages**. Under **Build and deployment** set **Source: Deploy from a branch**, **Branch: main**, folder **/ (root)** → **Save**.
5. Wait 1–2 minutes and refresh. GitHub shows **"Your site is live at https://YOUR-USERNAME.github.io/wardrobe/"**. Open it — the app already works in *"this device only"* mode. Keep this address; you need it below.

## Part 2 — Turn on "Sign in with Google" (≈15 min)

1. Open **console.cloud.google.com** and sign in with your Gmail. Accept the terms if asked.
2. **Create a project:** click the project picker at the top → **New project** → name it `Wardrobe App` → **Create**, then make sure it is selected.
3. **Enable the Drive API:** in the top search bar type `Google Drive API` → open it → **Enable**.
4. **Set up the sign-in screen:** search for `Google Auth Platform` (older menus call it *APIs & Services → OAuth consent screen*) → **Get started**:
   - App name: `Wardrobe` · User support email: your email → **Next**
   - Audience: **External** → **Next**
   - Contact information: your email → **Next** → tick the agreement → **Create**.
5. **Permissions:** left menu **Data Access** → **Add or remove scopes**. Tick:
   - `…/auth/drive.appdata` — *See, create, and delete its own configuration data in your Google Drive*
   - `openid`, `…/auth/userinfo.email`, `…/auth/userinfo.profile`

   → **Update** → **Save**. (These are all "non-sensitive" permissions: the app can only see the files it creates itself, never your other Drive files.)
6. **Create the key:** left menu **Clients** → **Create client** → Application type **Web application** → Name `Wardrobe web`, then:
   - **Authorized JavaScript origins** → Add URI: `https://YOUR-USERNAME.github.io` (no slash, no path)
   - **Authorized redirect URIs** → Add URI: `https://YOUR-USERNAME.github.io/wardrobe/` (**with** the slash at the end)

   → **Create**. Copy the **Client ID** (looks like `123456789-abc…apps.googleusercontent.com`). Ignore the client secret — the app doesn't use it.
7. **Let everyone sign in:** left menu **Audience** → under *Publishing status* (it says **Testing**) click **Publish app** → **Confirm**. Without this, only up to 100 people you list as test users can sign in.
8. *(Optional, nicer sign-in screen)* **Branding:** add App home page `https://YOUR-USERNAME.github.io/wardrobe/` and Privacy policy `https://YOUR-USERNAME.github.io/wardrobe/privacy.html`.

## Part 3 — Connect the app to your key (≈2 min)

1. On GitHub open your repository → click **config.js** → the **pencil (Edit)** icon.
2. Paste the Client ID between the quotes: `googleClientId: "123456789-abc….apps.googleusercontent.com"` → **Commit changes**.
3. Wait a minute, open `https://YOUR-USERNAME.github.io/wardrobe/` and tap **Sign in with Google**.

> Google says new settings "may take 5 minutes to a few hours" to apply. If sign-in fails right after setup, wait a little and try again.

## Part 4 — Move your existing wardrobe from the Claude version

1. In the **Claude version** of the app: **Settings → Export & backup → Full backup with photos (ZIP)** and save the file.
2. In the **new app** (signed in): **Settings → Restore from backup** → pick that ZIP → **Restore**. Records appear immediately; photos upload to your Drive in the background (keep the app open a minute).

## Part 5 — Install on phones and share

- **Android (Chrome):** open the link → menu **⋮** → **Install app** (or *Add to Home screen*).
- **iPhone (Safari):** open the link → **Share** → **Add to Home Screen**.
- **Share it:** send the link (WhatsApp, email…). Each person signs in with their own Google account and gets their own private wardrobe.
- **One wardrobe for the whole family:** sign in with the **same** Google account on each family phone and use **Family Members** inside the app. Different Google accounts = separate wardrobes.

---

## Updating the app later
Upload a new `index.html` to the repository (Add file → Upload files — a file with the same name replaces the old one). Phones pick up the new version the next time they open the app online. Users' data is not affected.

## Troubleshooting
| What you see | Fix |
|---|---|
| **Error 400: redirect_uri_mismatch** | The redirect URI in Google Cloud must be exactly `https://YOUR-USERNAME.github.io/wardrobe/` — same spelling, with the final `/`. |
| **Error: origin_mismatch** | The JavaScript origin must be just `https://YOUR-USERNAME.github.io` — no `/wardrobe`, no final slash. |
| **"Access blocked … has not completed the Google verification process"** or only some people can sign in | You haven't published the app: **Audience → Publish app**. |
| **"Sign-in is not set up" on the welcome screen** | `config.js` still has an empty Client ID, or GitHub hasn't redeployed yet (wait 1–2 min). |
| **"Sync paused — reconnect"** | Google sign-ins last about an hour. The app renews it when you open it; tap **Reconnect** if it asks. |
| **Company / school Google account can't sign in** | The organisation's admin may block outside apps — use a personal Gmail. |

## Good to know
- **Where is the data?** In each user's Google Drive, in a hidden "application data" folder. To see its size or delete it: Google Drive → ⚙ Settings → **Manage apps** → Wardrobe. Inside the app: **Settings → Danger zone**.
- **Offline:** the app opens and works without internet; changes sync automatically when you're back online.
- **Two phones editing the same item at the same moment:** the most recent save wins. Edits to different items always merge correctly.
- **AI photo suggestions** from the Claude version are not included (they needed Claude). Colour detection, duplicate warnings, QR labels, outfits, packing lists, reports and everything else work fully.
- **Backups:** Settings → Export & backup → *Full backup with photos (ZIP)* any time.
