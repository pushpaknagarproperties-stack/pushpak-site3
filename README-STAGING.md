# UPDATE build 04Nov-18 (sabse pehle yeh padho)
NAYA: Admin mobile app (PWA). Root me 7 nayi files: admin.html, manifest-admin.webmanifest, sw.js, icon-192.png, icon-512.png, icon-maskable-512.png, apple-touch-icon.png. Install page: /admin
Sirf 2 files badli hain: `index.html` aur `api/data.js` (data.js me ab Kalamboli bhi allowed hai). Baaki files pehle jaisi hain.
1. GitHub -> `staging` branch -> Add file -> Upload files -> sirf `index.html` aur `api/data.js` drop karo -> Commit directly to the staging branch.
2. Vercel me deployment Ready hone do -> Preview link kholo -> Ctrl+Shift+R.
3. Title ke saath `build 04Nov-18` aur `/api/data` par "prefix":"stg:" dikhna chahiye.
Naya kya hai: AI Smart Listing Importer (Lazy Parser tab), location wali Property ID (PP-001...), Save fix, BHK format, photo slider + zoom, EMI calculator, mobile par WhatsApp/Call bar, bulk actions, Duplicate, WhatsApp share, missing-details checklist, alt text.

# STAGING copy - panvelprojects.in (Phase 1)
Yeh copy sirf `staging` branch ke liye hai. `main` branch ko mat chhuona. Live site abhi jaisi hai waisi hi rahegi.

## Is version me kya naya hai
- Har property par domain tickbox (.in / .org / master), type (flat/plot/villa/shop/office), status (draft/active/inactive/archived), safe unique ID, created/updated time.
- Property tabhi dikhegi jab: domain ticked ho (ya blank = auto) AND location rule allow kare AND status active ho.
- Server par har property ki checking (galat location/type/status/duplicate ID reject).
- Audit log (kisne, kab, kya badla), "Backup now" aur "Restore", Excel/CSV import.
- RERA / CIDCO claims default OFF (Settings me switch). "TEST-RERA" kabhi dikhaya nahi jaata.
- Sample 13 projects hata diye gaye hain. Ab sirf aapka import kiya hua data dikhega.
- SAFETY: staging hamesha `stg:` prefix wala alag database use karta hai. Agar DB_PREFIX set nahi hai to server aur page dono saving rok dete hain (laal banner).

## Step 1: Files upload (GitHub, sirf staging branch)
1. GitHub repo kholo. Upar branch dropdown me `staging` chuno (main nahi).
2. Add file -> Upload files. Is folder ki SAARI files drag karo: index.html, vercel.json, package.json, .gitignore, robots.txt, sitemap.xml, aur poora `api` folder (5 files: _lib.js, data.js, lead.js, login.js, password.js).
3. Commit changes -> "Commit directly to the staging branch" chuno.

## Step 2: Preview link
1. Vercel -> pushpak-site -> Deployments. Naya deployment jiska branch `staging` hai, wahi Preview hai. Uska Visit link kholo (woh `...vercel.app` jaisa hoga).
2. Agar status "Checks Failed" hai to site update nahi hogi. Pehle wali "Deployment Checks" wali dikkat theek karo.
3. Preview link ke aage `/api/data` lagao. Dikhna chahiye: "prefix":"stg:", "stagingBlocked":false, "databaseReachable":true.
   - stagingBlocked true ya prefix khaali ho to RUKO. Vercel me DB_PREFIX = stg: sirf Preview par add karke Redeploy karo.
4. Page ke upar peela/orange banner "STAGING - test data only" dikhna chahiye. Laal banner dikhe to Persist/Import mat karo.

## Step 3: Import (31 properties)
1. Preview link kholo -> Login -> superadmin (staging me wahi password jo Vercel ke SUPER_HASH se bana hai).
2. Admin -> "Import & Backup" -> file chuno (.xlsx ya .csv; .xlsx ke liye internet chahiye, nahi to CSV banao).
3. Dikhna chahiye: "31 rows - 31 ready - 0 with problems". Phir "Import ready rows" dabao.
4. "Synced to server" ya "Import done: 31 added" dikhe. Admin -> Database me 31 rows aani chahiye.

## Step 4: 5 tests (dekhne ke liye preview link ke aage ?site=... lagao)
- Master (panvelprojects.in jaisa):  PREVIEW-LINK/?site=master
- .in jaisa:                          PREVIEW-LINK/?site=in
- .org jaisa:                         PREVIEW-LINK/?site=org
(Nila banner "PREVIEW" dikhega. Naye data ke liye 10-30 second ruko ya Ctrl+Shift+R dabao.)

Import ke turant baad properties ki gintee (hamare offline test se):
| View | Kitni properties dikhni chahiye |
|---|---|
| master | 29 |
| .in | 24 |
| .org | 11 |

| Test | master | .in | .org |
|---|---|---|---|
| TEST-1 (Ulwe, sirf .in tick) | nahi | HAAN | nahi |
| TEST-2 (Pushpak Sec-3, teeno tick) | HAAN | HAAN | HAAN |
| TEST-3 (Ulwe, sirf .org tick) | nahi | nahi | nahi |
| TEST-4 (Pushpak Sec-5, teeno tick) | HAAN | HAAN | HAAN |
| TEST-5 (Pushpak Sec-12, blank = auto) | HAAN | HAAN | nahi |

Deactivate test: Admin -> Database -> TEST-4 ka Live switch band karo. 10-30 second baad TEST-4 teeno views me gayab. (Ab ginti: master 28, .in 23, .org 10.) Switch wapas on karo.

## Step 5: Baaki checks
- Admin -> Audit log: import aur TEST-4 deactivate ki entries dikhni chahiye.
- Admin -> Import & Backup -> "Backup now": ek .json file download honi chahiye.
- Admin -> Settings: "Show RERA / CIDCO claims" OFF rahe. Cards par koi RERA badge nahi dikhna chahiye.
- Admin -> Database -> koi property Edit karke domain tick badlo, save karo, ?site= views me asar dekho.

## Zaroori dhyan
- Yeh copy `stg: true` ke saath bani hai, sirf testing ke liye. Isko `main` me merge MAT karo. Live release ke liye alag production ZIP milega.
- Staging me robots.txt "Disallow: /" hai aur page par noindex hai, taaki Google isko index na kare.
- Sites 2 aur 3 ke naye ZIP production release me milenge. Staging me unka test `?site=in` / `?site=org` se hota hai.
- Photos abhi nahi hain. Photo links baad me.
