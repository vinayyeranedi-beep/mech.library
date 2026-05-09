# ⚙️ Royal Mechanical Engineering Library Management System

<div align="center">

![Royal Library Banner](https://img.shields.io/badge/Royal%20Mechanical%20Library-Management%20System-C9A84C?style=for-the-badge&logo=bookstack&logoColor=white)

![HTML5](https://img.shields.io/badge/HTML5-E34F26?style=for-the-badge&logo=html5&logoColor=white)
![CSS3](https://img.shields.io/badge/CSS3-1572B6?style=for-the-badge&logo=css3&logoColor=white)
![JavaScript](https://img.shields.io/badge/JavaScript-F7DF1E?style=for-the-badge&logo=javascript&logoColor=black)
![Firebase](https://img.shields.io/badge/Firebase-FFCA28?style=for-the-badge&logo=firebase&logoColor=black)
![Three.js](https://img.shields.io/badge/Three.js-000000?style=for-the-badge&logo=three.js&logoColor=white)
![GitHub Pages](https://img.shields.io/badge/GitHub%20Pages-222222?style=for-the-badge&logo=github&logoColor=white)

> *A royal-themed, fully functional Library Management System built for the Mechanical Engineering Department — featuring 3D animations, Firebase Authentication, Realtime Database, and a complete library workflow.*

[🔴 Live Demo](#) • [📥 Download](#installation) • [🔥 Firebase Setup](#firebase-setup) • [📖 Features](#features)

</div>

---

## 📸 Preview

| Login Page | Dashboard |
|---|---|
| 3D Animated Books & Gears | Royal Gold & Navy Theme |
| Flip Card Login / Register | Full Library Management |

---

## ✨ Features

### 🔐 Authentication
- ✅ Email & Password Login / Register
- ✅ Google Sign-In (OAuth)
- ✅ GitHub Sign-In (OAuth)
- ✅ Role-based Access — **Admin**, **Librarian**, **Faculty**, **Staff**, **Student**
- ✅ Secret Key protection for Admin & Librarian registration
- ✅ Auto-login if session exists
- ✅ Secure Logout

### 📚 Library Management
- ✅ **Book Catalog** — Add, Edit, Delete, Search, Filter by category & status
- ✅ **Issue & Return** — Full transaction management with due dates
- ✅ **Members** — Register and manage students, faculty, staff
- ✅ **Fine Calculator** — Auto-calculate overdue fines with damage penalty
- ✅ **Reservations** — Book hold/waiting list with priority
- ✅ **Reports** — 8 report types including circulation, overdue, inventory
- ✅ **Settings** — Loan policy, fine rates, library hours, notification toggles

### 🎨 Design & UI
- ✅ **3D Three.js Animation** — Floating books, spinning gears, blueprint wireframes, piston animation
- ✅ **Royal Theme** — Deep navy + crimson + gold palette
- ✅ **Mouse Parallax** — 3D scene reacts to cursor movement
- ✅ **Custom Cursor** — Gold ring cursor
- ✅ **Flip Card** — 3D flip between Login and Register
- ✅ **Animated Particles** — Floating gold particles
- ✅ **Responsive** — Works on all screen sizes
- ✅ **Royal Typography** — Cinzel, Playfair Display, Cormorant Garamond

---

## 🚀 Installation

### Option 1 — GitHub Pages (Recommended)
```bash
1. Fork or download this repository
2. Go to Settings → Pages → Source → main branch
3. Your site is live at: https://yourusername.github.io/repo-name/
```

### Option 2 — Local
```bash
# Just open index.html in any browser — no server needed
open index.html
```

### Option 3 — VS Code Live Server
```bash
1. Install "Live Server" extension in VS Code
2. Right-click index.html → Open with Live Server
```

---

## 🔥 Firebase Setup

### Step 1 — Create Firebase Project
1. Go to [Firebase Console](https://console.firebase.google.com/)
2. Click **Add Project** → Enter project name → Create
3. Go to **Project Settings → Your Apps → Add Web App**
4. Copy your `firebaseConfig` values

### Step 2 — Add Config to index.html
Open `index.html` in VS Code, press `Ctrl+Shift+F`, search for `YOUR_API_KEY` and replace all 7 values:

```javascript
const firebaseConfig = {
  apiKey:            "YOUR_API_KEY",           // 🔑 Replace this
  authDomain:        "YOUR_AUTH_DOMAIN",       // 🔑 Replace this
  databaseURL:       "YOUR_DATABASE_URL",      // 🔑 Replace this
  projectId:         "YOUR_PROJECT_ID",        // 🔑 Replace this
  storageBucket:     "YOUR_STORAGE_BUCKET",    // 🔑 Replace this
  messagingSenderId: "YOUR_MESSAGING_SENDER_ID", // 🔑 Replace this
  appId:             "YOUR_APP_ID"             // 🔑 Replace this
};
```

### Step 3 — Enable Authentication
In Firebase Console → **Authentication → Sign-in methods**, enable:
- ✅ Email/Password
- ✅ Google
- ✅ GitHub

### Step 4 — GitHub OAuth Setup (for GitHub login)
1. Go to [GitHub Developer Settings → OAuth Apps](https://github.com/settings/developers)
2. Click **New OAuth App**
3. Set **Authorization callback URL** to your Firebase authDomain:
   ```
   https://YOUR_PROJECT_ID.firebaseapp.com/__/auth/handler
   ```
4. Copy **Client ID** and **Client Secret** into Firebase → GitHub provider settings

### Step 5 — Realtime Database Rules
Go to Firebase Console → **Realtime Database → Rules** and paste:

```json
{
  "rules": {
    "users": {
      "$uid": {
        ".read": "$uid === auth.uid",
        ".write": "$uid === auth.uid"
      }
    },
    "books": {
      ".read": "auth != null",
      ".write": "auth != null && root.child('users').child(auth.uid).child('role').val() === 'admin'"
    },
    "issued": {
      ".read": "auth != null",
      ".write": "auth != null && root.child('users').child(auth.uid).child('role').val() === 'admin'"
    },
    "fines": {
      ".read": "auth != null",
      ".write": "auth != null && root.child('users').child(auth.uid).child('role').val() === 'admin'"
    },
    "reservations": {
      "$uid": {
        ".read": "$uid === auth.uid || root.child('users').child(auth.uid).child('role').val() === 'admin'",
        ".write": "$uid === auth.uid || root.child('users').child(auth.uid).child('role').val() === 'admin'"
      }
    }
  }
}
```

---

## 🔑 Secret Keys for Admin & Librarian

When registering as **Admin** or **Librarian**, a secret key is required.

To change the keys, open `index.html` in VS Code, search for `MECH_ADMIN_2024`:

```javascript
const ADMIN_KEY     = 'MECH_ADMIN_2024';   // 👑 Change this to your Admin key
const LIBRARIAN_KEY = 'MECH_LIB_2024';     // 📚 Change this to your Librarian key
```

> ⚠️ **Important:** Keep these keys private. Share only with authorized personnel.

---

## 👥 User Roles

| Role | Secret Key Required | DB Role Value | Access Level |
|---|---|---|---|
| 👑 Admin | ✅ Yes | `admin` | Full access |
| 📚 Librarian | ✅ Yes | `librarian` | Library management |
| 👨‍🏫 Faculty | ❌ No | `faculty` | Extended borrowing |
| 🧑‍💼 Staff | ❌ No | `staff` | Standard access |
| 🎓 Student (B.Tech/M.Tech/PhD) | ❌ No | `member` | Standard access |

---

## 📁 File Structure

```
royal-mech-library/
│
├── index.html          ← Main file (Login + Dashboard combined)
├── README.md           ← This file
│
├── css/
│   └── styles.css      ← Master stylesheet (CSS variables, all components)
│
└── js/
    ├── login3d.js      ← Three.js 3D animation for login page
    └── login.js        ← Login / Register / Auth logic
```

> **Note:** The `index.html` is fully self-contained — all CSS and JS is embedded.
> The `css/` and `js/` files are provided separately for developers who want modular structure.

---

## 🛠️ Tech Stack

| Technology | Purpose |
|---|---|
| HTML5 / CSS3 | Structure & Royal styling |
| Vanilla JavaScript | App logic & interactivity |
| Three.js r128 | 3D book & gear animations |
| Firebase Auth | Email, Google, GitHub login |
| Firebase Realtime DB | User data & library records |
| Google Fonts | Cinzel, Playfair Display, Cormorant Garamond |
| GitHub Pages | Free hosting |

---

## 📖 How to Use

### As a Student / Member
1. Open the site → Click **Register here**
2. Fill in your details → Select **Student** role → Register
3. Login with your email & password
4. Browse the catalog, check availability, view your issued books

### As Admin / Librarian
1. Click **Register here** → Select **Admin** or **Librarian**
2. Enter the secret key provided by your institution
3. After login, you have full access to issue books, manage members, collect fines, generate reports

### As Google / GitHub User
1. Click the **Google** or **GitHub** button on the login page
2. Authenticate with your account
3. You are automatically registered and logged in as a Member

---

## 🤝 Contributing

1. Fork the repository
2. Create your feature branch: `git checkout -b feature/AmazingFeature`
3. Commit your changes: `git commit -m 'Add AmazingFeature'`
4. Push to the branch: `git push origin feature/AmazingFeature`
5. Open a Pull Request

---

## 📜 License

This project is licensed under the **MIT License** — free to use, modify and distribute.

---

## 👨‍💻 Developer

Built with ❤️ for the **Mechanical Engineering Department**

> *"Knowledge is the finest gear in the machine of progress."*

---

<div align="center">

⚙️ **Royal Mechanical Engineering Library** ⚙️

*Est. MMXXIV · Mechanical Department · Royal Knowledge Repository*

</div>
