# Connect People: going live (one-time setup, about 20 minutes)

Until you do this, the app runs in **demo mode**: data stays on each phone only, and the OTP is `123456`.

## 1. Create the Firebase project (free)
1. Go to https://console.firebase.google.com and sign in with a Google account.
2. **Add project**, give it a name, and you can turn Google Analytics off.
3. In the project, click the **Web** icon (`</>`) to add a web app. Copy the config it shows you
   (`apiKey`, `authDomain`, `projectId`, `appId`).
4. Open `connect-people.html`, find `const CONFIG`, and paste those four values into `firebase:{...}`.

## 2. Choose how people log in
In `CONFIG`, `authMode` is either:

| Mode | Cost | What it means |
|---|---|---|
| `"anonymous"` (default) | Free | One tap to join. No phone check. Good for a small pilot of people you know. |
| `"phone"` | Paid per SMS | Real SMS OTP. Needs the Firebase **Blaze** plan (a billing account with a card). |

**Anonymous mode:** go to **Build > Authentication > Sign-in method** and enable **Anonymous**.
Warning: anyone who opens your site can join, and signed-in members can see the phone numbers in profiles.
Share the link only with people you trust, until you switch to OTP.

**Phone mode:** enable **Phone** in the same place, upgrade to Blaze, then set `authMode:"phone"`.
Each SMS is charged. Check the current price for India on the Firebase pricing page, and set a
budget alert in Google Cloud. For free testing you can add up to 10 fictional test numbers under
Authentication > Sign-in method > Phone.

## 3. Create the database
1. **Build > Firestore Database > Create database**. Choose a region close to India (for example `asia-south1`) and **Production mode**.
2. Open the **Rules** tab, delete what is there, paste the whole content of `firestore.rules`, and click **Publish**.

## 4. Allow your website address
**Authentication > Settings > Authorized domains > Add domain**: add your GitHub Pages address, for example `yourname.github.io`.

## 5. Publish
Upload `connect-people.html` to your GitHub Pages repository (you can rename it to `index.html`).

## 6. Make yourself the admin
1. Open your live site and join (create your profile).
2. In Firebase, go to **Authentication > Users** and copy your **User UID**.
3. In **Firestore Database**, click **Start collection**, name it `admins`, and set **Document ID** to your UID.
   Add any one field (for example `active` = `true`).
4. Reload the app. You will now see a 🛡 **Admin** tab where reports appear.

## Good to know
- Posts disappear from the feed after their time, but stay in the database. You can delete old ones in the console.
- Deleting an account removes the profile and posts. Vouches and thanks stay as harmless records.
- If something fails with a "permission" message, the rules from step 3 were not published.
