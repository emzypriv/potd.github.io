# 🧟 PAWN OF THE DEAD — Firebase Setup Guide

## Firebase Collections & Documents

This guide will walk you through creating the collections and documents needed for your Pawn Shop system.

---

## 📋 Collection 1: `staff`

**Purpose:** Store staff login credentials and roles

### Create Collection
1. Go to Firebase Console → Firestore Database
2. Click **"+ Start collection"**
3. Collection name: `staff`
4. Click **"Next"**

### Add Staff Documents

#### Document 1: Manager Account
- **Document ID:** `manager1`
- **Fields:**

| Field | Type | Value |
|-------|------|-------|
| `displayName` | string | `"Store Manager"` |
| `pin` | string | `"1234"` |
| `role` | string | `"manager"` |

#### Document 2: Staff Account
- **Document ID:** `staff1`
- **Fields:**

| Field | Type | Value |
|-------|------|-------|
| `displayName` | string | `"Staff Member"` |
| `pin` | string | `"5678"` |
| `role` | string | `"staff"` |

---

## 📦 Collection 2: `items`

**Purpose:** Store inventory items

### Create Collection
1. Click **"+ Start collection"**
2. Collection name: `items`
3. Click **"Next"**

### Sample Items to Add

Click **"+ Add document"** for each item. Let Firestore auto-generate the Document ID, then add these fields:

#### Item 1: Rusty Machete
```
name: "Rusty Machete"
description: "Vintage undead slayer, well-used but reliable"
category: "weapons"
price: 49.99
condition: "good"
visible: true
createdAt: (timestamp - set to now)
```

#### Item 2: Antique Pocket Watch
```
name: "Antique Pocket Watch"
description: "Gold-plated, still keeps perfect time. Pre-outbreak."
category: "antiques"
price: 125.50
condition: "excellent"
visible: true
createdAt: (timestamp - set to now)
```

#### Item 3: Shotgun (Worn)
```
name: "Pump Shotgun"
description: "12-gauge, functional condition. Ammo NOT included."
category: "weapons"
price: 199.99
condition: "fair"
visible: true
createdAt: (timestamp - set to now)
```

#### Item 4: Electronics Bundle
```
name: "Solar Power Bank"
description: "10,000mAh, charges in sunlight. Essential for survival."
category: "electronics"
price: 79.99
condition: "mint"
visible: true
createdAt: (timestamp - set to now)
```

#### Item 5: Furniture - Workbench
```
name: "Heavy Wooden Workbench"
description: "Solid oak, perfect for repairs and crafting"
category: "furniture"
price: 225.00
condition: "good"
visible: true
createdAt: (timestamp - set to now)
```

#### Item 6: Cursed Item (Hidden)
```
name: "Mysterious Amulet"
description: "Origin unknown. Previous owner: deceased."
category: "cursed"
price: 999.99
condition: "mint"
visible: false
createdAt: (timestamp - set to now)
```

---

## 🔐 Firestore Security Rules

After creating your collections, update your Firestore security rules:

1. Go to Firestore Database → **Rules** tab
2. Replace existing rules with this:

```firestore
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    
    // Staff collection - read for login, no public writes
    match /staff/{userId} {
      allow read: if true;  // Needed for login verification
      allow write: if false; // Only add staff manually in console
    }
    
    // Items collection - public read, staff write
    match /items/{itemId} {
      allow read: if true;  // Public can browse
      allow write: if false; // Only staff can manage (via dashboard)
    }
  }
}
```

3. Click **"Publish"**

---

## 🔑 Field Types Guide

When adding documents in Firebase, use these types:

| Type | Example | Used For |
|------|---------|----------|
| **string** | `"Rusty Machete"` | Names, descriptions, roles |
| **number** | `49.99` | Prices |
| **boolean** | `true` or `false` | Visibility toggle |
| **timestamp** | Click "Server timestamp" | Auto date/time |

---

## ✅ Verification Checklist

After setup, verify:

- [ ] `staff` collection created with manager and staff documents
- [ ] `items` collection created with at least 3 sample items
- [ ] All items have: `name`, `description`, `category`, `price`, `condition`, `visible`, `createdAt`
- [ ] Security rules published
- [ ] Can view staff collection in console
- [ ] Can view items collection in console

---

## 🧪 Test Your Setup

1. Go to `https://yourname.github.io/pawn-of-the-dead/`
2. Login with:
   - **Username:** `manager1`
   - **Password:** `1234`
3. You should see the dashboard
4. Go to **Inventory** tab - should see your items
5. Go to **Add Item** - add a test item

---

## 📝 Adding More Items

To add items from the staff dashboard:

1. Login to staff dashboard
2. Click **"Add Item"** tab
3. Fill in:
   - Item Name
   - Description
   - Category (select from dropdown)
   - Price
   - Condition
   - Toggle "Visible to Public"
4. Click **"Register Item"**
5. Item appears in inventory immediately

---

## 🚨 Common Issues

### "Unknown operative" error on login
- Check that Document ID matches username exactly (lowercase)
- Verify PIN is stored as **string**, not number

### Items not showing on public page
- Make sure `visible: true` in item document
- Check that `items` collection exists
- Verify security rules allow public read

### Can't add items from dashboard
- Security rules may be blocking writes (expected - this is for security)
- Add items manually from Firebase console for now

---

## 💡 Collection Structure Reference

```
Firestore
├── staff/
│   ├── manager1
│   │   ├── displayName: "Store Manager"
│   │   ├── pin: "1234"
│   │   └── role: "manager"
│   └── staff1
│       ├── displayName: "Staff Member"
│       ├── pin: "5678"
│       └── role: "staff"
│
└── items/
    ├── (auto-id)
    │   ├── name: "Rusty Machete"
    │   ├── description: "..."
    │   ├── category: "weapons"
    │   ├── price: 49.99
    │   ├── condition: "good"
    │   ├── visible: true
    │   └── createdAt: (timestamp)
    └── ... more items
```

---

*PAWN OF THE DEAD — We buy the dead, we sell the damned.* ☠️
