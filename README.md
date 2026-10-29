# Lockbox: your own password manager (iPhone + Android)

Lockbox is a Keeper-style password vault that installs on your phone's home screen.
Everything is encrypted **on your phone** with your master password before it is saved
on the phone or sent to Google Drive. Google only ever gets an unreadable file.

| Feature | Details |
|---|---|
| Encryption | AES-256-GCM. The key comes from your master password via PBKDF2-SHA256 (600,000 rounds). |
| Records | Title, username, password, website, folder, notes, favorite |
| Organizing | Folders, favorites, instant search |
| Generator | 8 to 64 characters, choice of character sets, strength meter |
| Safety | Auto-lock (default 5 min, or 1 min after leaving the app), clipboard clears after 30 s |
| Google Drive sync | Encrypted vault kept in a hidden app folder of **your** Drive. Merges changes from all your devices. |
| Backup files | Export an encrypted `.lockbox` file (share it to Drive), or import one and merge it |
| Offline | Works with no internet. Sync happens when you tap the sync button. |

> ⚠️ **There is no password recovery.** If you forget your master password, nobody can open the vault (that's the point). Use a long passphrase you will remember.

---

## Step 1: Put the app online (free, about 5 minutes)

Phones only install web apps from an **https** address. GitHub Pages is free:

1. Create a free account at https://github.com and click **New repository**. Name it `lockbox` and make it **Public**. (The code is public, but your passwords never go there.)
2. Click **uploading an existing file** and drag in everything from this folder (`index.html`, `manifest.webmanifest`, `sw.js` and the `icons` folder). Click **Commit**.
3. Go to **Settings → Pages**, set *Source* to **Deploy from a branch**, then *Branch* to **main** and **/(root)**, and click **Save**.
4. After about a minute your app is live at `https://YOUR-USERNAME.github.io/lockbox/`

(Netlify Drop at https://app.netlify.com/drop also works: drag the folder in.)

## Step 2: Install on your phones

- **iPhone:** open the link in **Safari**, tap **Share** and then **Add to Home Screen**.
- **Android:** open the link in **Chrome**, tap **⋮** and then **Install app** (or *Add to Home screen*).

Open Lockbox from the home-screen icon and create your master password.

> On iPhone, always use the home-screen icon. iOS can clear data for websites opened only in Safari, but it keeps data for apps added to the home screen. Keep Drive sync or backups on in any case.

## Step 3: Turn on Google Drive sync (one-time, about 5 minutes)

Google requires every app that uses Drive to have its own "Client ID". You create one for yourself:

1. Go to https://console.cloud.google.com, create a project (e.g. "Lockbox"), and select it.
2. **APIs & Services → Library**: search **Google Drive API** and click **Enable**.
3. **APIs & Services → OAuth consent screen** (or *Google Auth Platform → Branding*):
   - User type **External**, app name "Lockbox", and your email as the support and developer contact.
   - **Audience → Test users**: add your Gmail address (`shyleshmv92@gmail.com`).
   - **Data access / Scopes**: add `.../auth/drive.appdata`.
4. **APIs & Services → Credentials → Create credentials → OAuth client ID**
   - Application type: **Web application**
   - **Authorized JavaScript origins**: `https://YOUR-USERNAME.github.io`
     (origin only, without the `/lockbox` path)
   - Click **Create** and copy the **Client ID** (ends in `.apps.googleusercontent.com`).
5. In Lockbox, open **Settings (⚙) → Google OAuth Client ID**, paste it, and tap **Save**.
6. Tap the **sync button** (↻) and sign in with Google. Done.

Your vault is saved as `lockbox.vault` in Drive's hidden *app data* folder. It doesn't
clutter your Drive, and other apps can't read it. You can see it under
drive.google.com → ⚙ Settings → **Manage apps**.

**Notes**
- While the app is in "Testing" mode, Google may ask you to sign in again about once a week. That's normal for personal apps.
- When you edit something, the app syncs by itself if you signed in during the last hour. Otherwise just tap ↻.

## Setting up your second phone

Install the app (Step 2). On the welcome screen, tap **Restore from Google Drive**, paste the
same Client ID, sign in, and enter your master password. Both phones now share the vault.
Tap ↻ to sync. Changes from both phones are merged record by record.

If you change the master password on one phone, the other phone asks for the new one at its next sync.

## Backups without the Google setup

**Settings → Export backup** creates an encrypted `.lockbox` file. On your phone, choose
**Drive** in the share sheet to save it to Google Drive. To restore, tap **Import / merge**
(or *Restore from a backup file* on the welcome screen) and choose the file. Your phone's file picker can browse Google Drive.

## Files

```
index.html            the whole app (UI, encryption, sync)
sw.js                 offline support
manifest.webmanifest  home-screen install info
icons/                app icons
```
