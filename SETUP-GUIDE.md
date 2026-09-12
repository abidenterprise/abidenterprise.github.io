# 🏢 Abid Enterprise — Secure HR ERP (abidenterprise.github.io)

Online HR system with **login & encryption** — employee data is stored **ENCRYPTED (AES-GCM)** in your GitHub repository. Visitors to the site see **only a login screen**. Only users you create can open the records.

**বাংলা:** এটি আবিদ এন্টারপ্রাইজের সুরক্ষিত অনলাইন এইচআর সিস্টেম। সব ডেটা গিটহাবে **এনক্রিপ্টেড** অবস্থায় থাকে — বাইরের কেউ শুধু লগইন পেজ দেখতে পায়। আপনার তৈরি করা ইউজারনেম/পাসওয়ার্ড দিয়েই কেবল ঢোকা যায়।

---

## 🔐 How security works

| Layer | What it means |
|---|---|
| 🔒 Encrypted data | `data/employees.json` is AES-GCM encrypted. On github.com you only see unreadable text |
| 🔑 User accounts | You create usernames & passwords inside the app (User Accounts menu) |
| 🖼 Encrypted photos | Photos are encrypted and saved named by employee code (e.g. `photos/C-10006.enc`) |
| 🎟 Hidden token | Your GitHub token is stored encrypted — editors don't need their own token |
| 👁 Public site | Anyone opening abidenterprise.github.io sees ONLY the login page — no data leaks |

### User roles (ভূমিকা)

| Role | Can do |
|---|---|
| **Admin** (অ্যাডমিন) | Everything: manage users, settings, token, delete employees, import/export |
| **Editor** (সম্পাদক) | Add / edit / resign employees, import from Excel |
| **Viewer** (পর্যবেক্ষক) | View records & print only — cannot change anything |

---

## 🚀 Setup — one time (~10 minutes)

### Step 1 — Create the repository
1. Log in at **github.com** with username **abidenterprise**.
2. **+** (top-right) → **New repository**.
3. Name: **`abidenterprise.github.io`** ← exactly this. Visibility: **Public** (data is encrypted, so public is safe).
4. **Create repository**.

### Step 2 — Upload the files
1. On the repo page → **"uploading an existing file"**.
2. Upload **`index.html`**, the **`data`** folder, and the **`photos`** folder.
3. **Commit changes**.

### Step 3 — Open & create the admin account
1. Wait 1–2 minutes → open **https://abidenterprise.github.io**.
2. The **Initial Setup** screen appears → choose:
   - Admin username (e.g. `admin`)
   - A strong password (min 6 characters)
   - Your **GitHub token** (next step tells you how; you can skip and add later)
   - Leave "Load demo employees" unchecked for real use (it only adds 4 test records)
3. Press **Create Secure System** → the app encrypts everything and uploads it. 🎉

### Step 4 — Get a GitHub token (if you skipped it)
1. github.com → profile photo → **Settings** → **Developer settings** → **Personal access tokens** → **Fine-grained tokens** → **Generate new token**.
2. Name: `hr-erp` · Expiration: **1 year** · Resource owner: abidenterprise.
3. Repository access: **Only select repositories** → `abidenterprise.github.io`.
4. Permissions → Repository permissions → **Contents: Read and write**.
5. Generate → copy (`github_pat_…`).
6. In the ERP: **Settings → Replace GitHub token → Save Token**. The token is stored encrypted.

### Step 5 — Create user accounts for your staff
1. Menu → **User Accounts**.
2. Add each person: username + password + role (Editor or Viewer).
3. Share their username/password with them (e.g. by phone — not by public chat).
4. They open the same website, sign in, and work — **no token, no installation needed**.

---

## 📥 Import your employees from Excel (to the server)

1. Open your Excel file — keep one row per employee with a header row.
   Recognized columns: Emp Code, Name, Designation, Department, Section, Job Location, Date Of Joining, Date Of Birth, Mobile number, Father's Name, Mother's Name, NID, Blood Group, Gender, Address…
2. In the ERP: **Settings → Choose Excel (.xlsx) or CSV file** → select your file.
   - `.xlsx` supported directly (dates like 01-Oct-13, 1/10/2013 and Excel serial dates all understood).
   - Or save as **CSV UTF-8** from Excel first.
3. Result: **every row is added to the server encrypted** (the shipped data file starts empty). If you import again later, existing Emp Codes are updated, new codes are added.
4. Best practice: add the **Name (Bangla)** column in your Excel too — it imports as well.

### 📷 Photos — two ways
- **One by one:** open an employee → Edit → Choose Photo → Save. It's compressed, encrypted and saved as `photos/<emp-code>.enc` automatically.
- **In bulk:** you cannot upload encrypted photos by hand — use the app. (Tip: do it from a phone, it's fast.)

---

## 📱 Daily use

| Action | How |
|---|---|
| Sign in | Open site → username + password ("Keep me signed in" = stays logged in on that device) |
| Search / filter | Employees menu — by name, code, mobile, NID, department, location, status |
| New employee | Add Employee → fill English + Bangla → photo → Save |
| Resignation | Profile → **Resign** → last working day + reason (record kept as history) |
| Wrong resignation | Profile → **Reinstate** |
| Delete employee | Admin only — profile → 🗑 (usually resignation is better) |
| Print | Profile → 🖨 Print |
| English ⇄ বাংলা | Top-right button — whole UI switches |
| Change my password | User Accounts → Change My Password |
| Reset someone's password | Admin: User Accounts → 🔑 Reset PW |

## 🔄 Multi-device / multi-user

- Everyone uses the same website URL. Data syncs through your GitHub repo.
- Two people editing at the exact same moment → the last save shows "data changed elsewhere" → reload page and retry (rare).
- Each user can "Keep me signed in" on their own phone/PC. **Logout** ends the session on that device.

## 🆘 Recovery & safety

| Situation | What to do |
|---|---|
| User forgot password | Any **Admin** resets it: User Accounts → 🔑 Reset PW |
| The ONLY admin forgot password | Encrypted data cannot be opened again. On github.com delete `data/security.json` → open the site → create a new admin → **Settings → Restore JSON backup** with your last backup |
| Token expired / leaked | Create a new token (Step 4) → Settings → Save Token. To fully revoke: delete the old token at github.com → Developer settings |
| Backup | **Settings → Export JSON backup** (unlocked, for admins/editors). Keep at least one backup per month in a safe place (email/USB) |
| Suspicious staff account | Admin: User Accounts → 🗑 Remove user. Change the token if you think it was copied |

> ⚠️ **Important:** If you previously uploaded the OLD non-encrypted version, its files remain in GitHub history. Best: start a **fresh repository** with this secure version (or delete & recreate the repo).

---

## 🛠 Troubleshooting

| Problem | Fix |
|---|---|
| Site 404 | Wait 2 min; repo must be named exactly `abidenterprise.github.io`, Public |
| "Wrong username or password" | Check spelling; admin can reset your password |
| "Read-only (no token)" badge | Admin must save a token in Settings |
| Sync failed / HTTP 401 | Token expired → new token → Save Token |
| HTTP 422 | Someone saved at the same time → reload page, retry |
| .xlsx won't import | Use a modern browser (Chrome/Edge), or save as CSV UTF-8 |
| Photos don't show | They decrypt from the server — check internet; photo files must end in `.enc` in the photos folder |

---

*Secure version · Single-file app · AES-GCM encryption · PBKDF2 password keys · No server cost — runs on GitHub Pages.*
