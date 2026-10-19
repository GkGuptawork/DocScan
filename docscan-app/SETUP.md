# DocScan app: setup (about 15 minutes, one time)

You get a real installable app: own icon on the home screen, opens full-screen, loads instantly, live camera for shooting bill after bill, and it works with your existing Sheet.

**How it fits together**
- `Code.gs` stays in Apps Script as the private backend (Gemini key and Sheet access live there).
- The app files (`docscan-app` folder) are hosted free on GitHub Pages.
- You sign in once per phone with the Server URL + your PIN.

---

## Part 1: Update the backend (Apps Script)

1. Open your DocScan Apps Script project.
2. Replace the whole of `Code.gs` with the new `Code.gs`. (You can delete the old `Index` HTML file, it is no longer used.)
3. Click the gear icon (**Project Settings**) → **Script properties** → **Add script property**
   - Property: `APP_PIN`
   - Value: your PIN, **6 or more characters**. Use something long that you do not use elsewhere.
4. **Deploy → Manage deployments → ✏️ Edit**
   - Execute as: **Me**
   - Who has access: **Anyone**
   - Version: **New version** → **Deploy**
5. Copy the **Web app URL** (ends with `/exec`). Keep it handy.

Your Sheet link, headers, model and Gemini key are already saved from before, nothing to re-enter.

> "Anyone" only means the URL can be reached. Every request is refused without your PIN, and after 8 wrong PINs the server locks for 15 minutes.

## Part 2: Put the app online (GitHub Pages)

1. Unzip `docscan-app.zip`. You get a folder with `index.html`, `sw.js`, `manifest.webmanifest`, `config.js` and an `icons` folder.
2. On github.com create a new **public** repository named `docscan`.
3. **Add file → Upload files**, drag in everything from the unzipped folder (keep the `icons` folder), then **Commit**.
4. Repository **Settings → Pages** → Source: **Deploy from a branch** → Branch: **main** / root → Save.
5. After a minute your app is live at `https://YOUR-USERNAME.github.io/docscan/`

(Alternative: drag the folder onto app.netlify.com/drop for an instant link.)

Optional: put your Web app URL inside `config.js` before uploading so you never type it on the phone.

## Part 3: Install on your phone

- **Android (Chrome):** open the app link → menu ⋮ → **Install app**, or use the **Install DocScan** button in Settings.
- **iPhone (Safari):** open the link → Share → **Add to Home Screen**.

Open it from the home screen icon, paste the **Server URL**, enter the **PIN** → Unlock. Done.

---

## Using it
- **Scan documents:** camera stays open. Tap the round button for each bill. Every shot goes into your Sheet in the background. Tap **Done** when finished.
- **Upload images / PDF:** pick one or many files.
- **Entries:** tap to open and fix a value. Hold a card to select many, then Copy, Send or Print.
- **Offline:** the app opens offline and shows your last saved list. Scans taken offline wait and upload when you are back online (keep the app open).
- **Lock and sign out** is in Settings.

## Updating later
Upload the changed file to GitHub. If you change app files, also change `VERSION` in `sw.js` (for example `docscan-v3-2`) so phones pick up the update on next open.

## Important
- Your Gemini API key appeared in a screenshot earlier. Create a new key in Google AI Studio, delete the old one, and paste the new one in Settings.
- To change the PIN: edit `APP_PIN` in Script properties, then sign in again on each phone.
- If sign-in says "Unexpected reply": the deployment is not set to **Anyone**, or you did not pick **New version**.
