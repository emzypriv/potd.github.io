# 🧟 PAWN OF THE DEAD — Undead Merchandise Dashboard

A zombie-themed pawn shop inventory system with public storefront and staff management dashboard. Built for GitHub Pages + Firebase, optimized for **LB-Phone** mobile UI.

---

## 🚀 QUICK START

### 1. Firebase Setup

1. Go to [https://console.firebase.google.com](https://console.firebase.google.com)
2. Click **"Create a project"** → name it `pawn-of-the-dead`
3. Disable Google Analytics → **Create project**
4. In left sidebar → **Firestore Database** → **Create database**
   - Choose **"Start in test mode"**
   - Pick a region close to you
5. In left sidebar → **Project Settings** (gear icon) → **Your apps** → **Add app** → Web (`</>`)
6. Register app name as `pawn-of-the-dead-web`
7. **Copy the `firebaseConfig` object**

### 2. Update Firebase Config

Open these files and replace `YOUR_API_KEY`, `YOUR_APP_ID`, etc. with your Firebase credentials:

- `index.html` (staff login)
- `pages/public.html` (public storefront)
- `pages/staff-dashboard.html` (staff management)

### 3. Create Firestore Collections

#### **`staff` Collection**

Create user documents with this structure:

| Field | Type | Example |
|-------|------|----------|
| `pin` | string | `"1234"` |
| `role` | string | `"manager"` or `"staff"` |
| `displayName` | string | `"Zombie Bob"` |

**Example document ID:** `zombiebob`

#### **`items` Collection**

Each item document:

| Field | Type | Example |
|-------|------|----------|
| `name` | string | `"Rusty Machete"` |
| `description` | string | `"Vintage undead slayer"` |
| `category` | string | `"weapons"`, `"jewelry"`, `"electronics"`, `"furniture"`, `"antiques"`, `"cursed"` |
| `price` | number | `49.99` |
| `condition` | string | `"mint"`, `"excellent"`, `"good"`, `"fair"`, `"poor"` |
| `visible` | boolean | `true` |
| `createdAt` | timestamp | Auto-populated |

### 4. Deploy to GitHub Pages

1. Create repo: `yourname/pawn-of-the-dead` (or similar)
2. Make it **Public** (or Private if you prefer)
3. Upload all files maintaining structure:
   ```
   /index.html
   /css/style.css
   /pages/public.html
   /pages/staff-dashboard.html
   /README.md
   ```
4. Go to repo **Settings** → **Pages**
5. Under **Source**, select: `main` branch → `/ (root)` → **Save**
6. Your sites will be live at:
   - **Public Storefront:** `https://emzypriv.github.io/pawn-of-the-dead/pages/public.html`
   - **Staff Dashboard:** `https://emzypriv.github.io/pawn-of-the-dead/`

---

## 📁 FILE STRUCTURE

```
pawn-of-the-dead/
├── index.html                    ← Staff login page
├── css/
│   └── style.css                 ← All styles (phone-optimized)
├── pages/
│   ├── public.html               ← Public storefront
│   └── staff-dashboard.html      ← Staff management dashboard
└── README.md
```

---

## 🎮 FEATURES

### **Public Storefront** (`pages/public.html`)
- Browse all visible items
- Filter by category
- View item details (condition, price, description)
- Phone-optimized UI (360px width)

### **Staff Dashboard** (`pages/staff-dashboard.html`)
- **Inventory Tab:** View all items in stock
- **Add Item Tab:** Register new merchandise
  - Name, description, category
  - Price and condition
  - Visibility toggle
- **Edit Tab:** Modify or delete existing items
- Secure login system

---

## 🎨 CUSTOMIZATION

### Colors (in `css/style.css`)

```css
:root {
  --primary-bg: #0a0a0a;        /* Black background */
  --primary-text: #8fbc2f;      /* Undead green */
  --accent-red: #8b0000;        /* Blood red */
  --accent-orange: #ff4500;     /* Zombie orange */
}
```

### Item Categories

Edit in `pages/staff-dashboard.html` (the `<select id="itemCategory">` dropdown):

```html
<option value="weapons">Weapons</option>
<option value="jewelry">Jewelry</option>
<option value="electronics">Electronics</option>
<!-- Add more categories here -->
```

### Conditions

Current options: `mint`, `excellent`, `good`, `fair`, `poor`

Edit the `<select id="itemCondition">` dropdown if needed.

---

## 🔐 SECURITY NOTES

1. **Staff Collection:** Add Firestore rules to prevent public writes:

```firestore
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    match /staff/{userId} {
      allow read: if true;      // needed for login
      allow write: if false;    // only you add staff manually
    }
    match /items/{itemId} {
      allow read: if true;      // public can view
      allow write: if false;    // staff edit via dashboard
    }
  }
}
```

2. **PIN Storage:** Currently stored as plaintext. For production, consider:
   - Using Firebase Authentication instead
   - Hashing PINs server-side
   - Implementing session expiration

3. **Sensitive Data:** Don't hardcode sensitive credentials in client-side code.

---

## 📱 PHONE UI (LB-Phone)

- **Width:** 360px (standard mobile)
- **Height:** 800px
- **Aspect Ratio:** 9:20 (portrait)
- **Border:** Red with glow effect
- **Scrollable:** Single vertical scroll
- **Responsive:** Adapts to smaller screens

---

## 🛠 TROUBLESHOOTING

**Items not showing?**
- Check Firestore collection name (`items`)
- Ensure `visible: true` in item documents
- Check browser console for errors

**Login not working?**
- Verify Firebase config is correct
- Ensure `staff` collection exists
- Check PIN is stored as string, not number

**Styling looks broken?**
- Clear browser cache (Ctrl+Shift+Delete)
- Ensure `css/style.css` is linked correctly
- Check for CSS font load errors (Barlow Condensed)

---

## 📞 SUPPORT

For Firebase help, see: [Firebase Documentation](https://firebase.google.com/docs/guides)

*PAWN OF THE DEAD — We buy the dead, we sell the damned.* ☠️