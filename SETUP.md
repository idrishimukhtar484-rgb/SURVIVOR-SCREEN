# SURVIVOR SCREEN / SURVIVOR SCREEN - Setup (Render)

## 1. Files
Repo (Private) mein root pe ye files:
- `server.js`
- `package.json`
- `render.yaml` (optional, Blueprint deploy ke liye)

Zip mat upload karo, files alag-alag daalo.

## 2. Render deploy
1. render.com -> **New +** -> **Web Service** -> GitHub repo select karo.
2. Runtime **Node**, Build `npm install`, Start `npm start`.
3. Plan **Free** (ya paid), Health Check Path `/health`.
4. **Create Web Service**. Deploy ke baad URL milega: `https://NAAM.onrender.com`
5. (Optional) Environment mein `PUBLIC_URL` = apna Render URL. Na do to `RENDER_EXTERNAL_URL` auto use hota hai.

Free plan 15 min idle ke baad sleep hota hai. Code har 10 min khud `/health` ping karta hai. Pakka rakhna ho to UptimeRobot se `/health` 5 min pe ping karo ya paid plan lo.

## 3. Deploy check
- `https://NAAM.onrender.com/health` -> `{"ok":true,...}`
- `https://NAAM.onrender.com/admin.html` -> Admin panel
- `https://NAAM.onrender.com/download` -> Download page
- `curl -i https://NAAM.onrender.com/api/admin/updates` -> `401` aaye to routes live hain
- Render Logs mein "Database rules published" dikhe.

## 4. Firebase (naya project - dekho ../FIREBASE_SETUP.md)
- Realtime Database ON, Authentication ON (jo sign-in method app use karta hai).
- Rules backend start pe khud publish hote hain. Fail ho to `database.rules.json` console mein paste karo.
- Service-account key, database URL aur web API key Render Environment mein daalo (`FIREBASE_SERVICE_KEY`, `FIREBASE_DATABASE_URL`, `FIREBASE_WEB_API_KEY`).

## 5. Pehla admin
- `/admin.html` kholo. Admin signup ke liye `ADMIN_SIGNUP_PIN` chahiye: `server.js` ke SETTINGS mein `ADMIN_SIGNUP_PIN` bharo ya Render Environment mein set karo.
- Signup ke baad PIN hata do ya badal do.

## 6. Payments (ZapUPI)
- Admin web -> App Settings -> **ZapUPI Payment Key** (Owner only). Redeploy nahi chahiye.
- ZapUPI dashboard mein webhook: `https://NAAM.onrender.com/api/pay/webhook/zapupi`

## 7. Image upload (Telegram)
- Bot ko private channel ka **Admin** banao (post permission).
- `TELEGRAM_CHANNEL_ID` aam taur pe `-100...` se shuru hota hai.

## 8. Android APK
- App ka backend base URL `https://NAAM.onrender.com` karo aur APK dobara build karo.
- Update ke liye `versionCode` har baar badhao (9 -> 10 -> 11), `versionName` jaise `2.0`, `2.1`, `2.2`.
- Same signing key se hi build karo.

## 9. App update flow
1. APK ko hosting pe upload karo (direct `.apk` link, `curl -I` pe 200).
2. Admin -> **App Updates** -> version, title, description, features, `downloadUrl` bharo.
3. **Publish** karo. Players ko push jaata hai aur `/download` page khud update hota hai.
4. Version format: `2.0`, `2.1`, `2.2` (3 part zaroori nahi).

## 10. Security
- Repo **Private** rakho. `server.js` mein Telegram/ZapUPI keys ka default fallback hai - behtar hai ye Render Environment mein set karo aur file se hata do.
- Keys kabhi leak ho to Firebase key aur Telegram bot token rotate karo.
