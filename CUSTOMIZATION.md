# 🎨 Customization Guide

**Step-by-Step Guide to Make This Website Yours**

This guide will walk you through customizing every aspect of this template for your business. No coding experience required for basic customizations!

---

## 📋 Table of Contents

1. [Before You Start](#before-you-start)
2. [Basic Information](#1-basic-information)
3. [Brand Colors](#2-brand-colors)
4. [Logo & Mascot](#3-logo--mascot)
5. [Menu Items](#4-menu-items)
6. [Owner Password](#5-owner-password)
7. [Initial Data](#6-initial-data)
8. [Advanced Customization](#7-advanced-customization)
9. [Testing](#8-testing)

---

## Before You Start

### What You'll Need:
- ✅ Text editor (Notepad++, VSCode, or even Notepad)
- ✅ Your business information (name, descriptions, etc.)
- ✅ Product photos (or use Unsplash URLs)
- ✅ Your color preferences (optional)
- ✅ 30-60 minutes of time

### Tips:
- 💡 Make a backup copy of `index.html` before editing
- 💡 Change one thing at a time and test
- 💡 Use Ctrl+F (Find) to locate text in the file
- 💡 Keep the file structure intact

---

## 1. Basic Information

### 1.1 Business Name

**Find and Replace:**
```javascript
// Search for: "Chamele-Tea"
// Replace with: "Your Business Name"

// Locations in file (use Ctrl+F):
Line ~50: <title>Chamele-Tea | Color Your Boba Experience</title>
Line ~250: h1 className="text-xl...>Chamele-Tea</h1>
Line ~320: businessName: 'Chamele-Tea'
```

**Example:**
```html
<!-- Before -->
<title>Chamele-Tea | Color Your Boba Experience</title>

<!-- After -->
<title>Maria's Coffee House | Fresh Brews Daily</title>
```

### 1.2 Tagline / Slogan

**Find:**
```javascript
// Line ~251
<p className="text-[10px]...>CUSTOMIZABLE BOBA LAB</p>
```

**Replace with your tagline:**
```javascript
<p className="text-[10px]...>FRESH BREWED COFFEE & PASTRIES</p>
```

### 1.3 Business Description

**Find (Line ~728):**
```javascript
description: 'Small batch teas, authentic brown sugar pearls...'
```

**Replace with:**
```javascript
description: 'Family-owned cafe serving organic coffee and homemade pastries since 2020.'
```

---

## 2. Brand Colors

### 2.1 Main Color Palette

**Find** the `tailwind.config` section (around Line ~29):

```javascript
tailwind.config = {
  theme: {
    extend: {
      colors: {
        chameleon: {
          emerald: '#10B981',   // Main green
          teal: '#14B8A6',       // Accent teal
          purple: '#8B5CF6',     // Accent purple
          pink: '#EC4899',       // Accent pink
          dark: '#0F172A',       // Dark navy
        }
      }
    }
  }
}
```

**Replace with your brand colors:**

```javascript
colors: {
  mybrand: {              // Change "chameleon" to "mybrand"
    primary: '#YOUR_COLOR',    // Your main color
    secondary: '#YOUR_COLOR',  // Your secondary color
    accent: '#YOUR_COLOR',     // Accent color
    dark: '#YOUR_COLOR',       // Dark color
  }
}
```

**Color Picker Tools:**
- [Coolors.co](https://coolors.co/) - Generate color palettes
- [Adobe Color](https://color.adobe.com/) - Color wheel
- HTML Color Codes: `#RRGGBB` format

### 2.2 Update Color References

After changing color names, update these classes throughout the file:

**Find:** `bg-chameleon-emerald` or `text-chameleon-teal`
**Replace:** `bg-mybrand-primary` or `text-mybrand-secondary`

**Common locations:**
- Buttons (search for `bg-teal-` or `bg-emerald-`)
- Headers (search for `bg-gradient-to-r`)
- Status badges
- Navigation

**Tip:** Use Find & Replace carefully. Test after each major change!

---

## 3. Logo & Mascot

### Option A: Use Your Logo Image

**Find** the `ChameleonMascot` component (Line ~208):

**Replace the entire SVG with an image:**

```javascript
const YourLogo = ({ className = "w-10 h-10" }) => (
  <img 
    src="https://your-website.com/logo.png" 
    alt="Your Business Logo" 
    className={className}
  />
);
```

**Then find and replace all instances:**
- `<ChameleonMascot` → `<YourLogo`

### Option B: Keep the Mascot, Change Colors

**Find** the mascot SVG and update the `fill` colors:

```javascript
// Change these hex colors to match your brand
fill="#10B981"  →  fill="#YOUR_COLOR"
fill="#FACC15"  →  fill="#YOUR_COLOR"
```

### Option C: Remove the Mascot

Simply remove or comment out the `<ChameleonMascot />` components.

---

## 4. Menu Items

### 4.1 Menu Structure

**Find** `const INITIAL_MENU` (around Line ~85):

```javascript
const INITIAL_MENU = [
  {
    id: 'm1',
    name: 'Chameleon Signature Brown Sugar',
    category: 'Milk Teas',
    price: 6.25,
    rating: '4.9',
    popular: true,
    image: 'https://images.unsplash.com/photo-...',
    description: 'Rich black tea, creamy oat milk...',
    badge: 'Best Seller'
  },
  // More items...
];
```

### 4.2 Add Your Products

**Template for each item:**

```javascript
{
  id: 'm1',                    // Unique ID (m1, m2, m3, etc.)
  name: 'YOUR PRODUCT NAME',   // Product name
  category: 'YOUR CATEGORY',   // Must match categories list
  price: 0.00,                 // Base price as number
  rating: '4.5',               // Star rating (string)
  popular: true,               // true/false for homepage
  image: 'IMAGE_URL',          // Photo URL
  description: 'DESCRIPTION',  // Product description
  badge: 'BADGE_TEXT'          // Optional: Best Seller, New, etc.
}
```

### 4.3 Update Categories

**Find** `const categories` (Line ~300):

```javascript
const categories = ['All', 'Milk Teas', 'Fruit Teas', 'Slush & Ice'];
```

**Replace with your categories:**

```javascript
const categories = ['All', 'Hot Drinks', 'Cold Drinks', 'Pastries', 'Sandwiches'];
```

**Make sure menu item categories match exactly!**

### 4.4 Finding Product Images

**Free Image Sources:**
- [Unsplash](https://unsplash.com/) - Free high-quality photos
- [Pexels](https://pexels.com/) - Free stock photos
- Your own photos uploaded to [Imgur](https://imgur.com/)

**How to get Unsplash URL:**
1. Go to Unsplash.com
2. Search for your product (e.g., "coffee")
3. Click on image
4. Copy URL with `?auto=format&fit=crop&w=600&q=80`

**Example:**
```
https://images.unsplash.com/photo-1559056199-641a0ac8b55e?auto=format&fit=crop&w=600&q=80
```

### 4.5 Customization Options

**Sweetness Options** (Line ~148):
```javascript
const SWEETNESS_OPTIONS = ['0%', '30%', '50%', '70%', '100%'];
```

**Ice Options** (Line ~149):
```javascript
const ICE_OPTIONS = ['No Ice', 'Less Ice', 'Regular Ice', 'Extra Ice'];
```

**Toppings/Add-ons** (Line ~150):
```javascript
const TOPPING_OPTIONS = [
  { name: 'Brown Sugar Pearls', price: 0.75 },
  { name: 'Whipped Cream', price: 0.50 },
  { name: 'Extra Shot Espresso', price: 1.00 },
  // Add your own!
];
```

**Customize for your business:**
- Coffee shop: Add milk options, syrups, extra shots
- Bakery: Add glazing options, filling choices
- Sandwich shop: Add toppings, bread choices
- Or remove entirely if not needed

---

## 5. Owner Password

### ⚠️ IMPORTANT: Change This Before Deploying!

**Current Password:** `Chameleon_Tea123`

**Find** (Line ~413):
```javascript
if (ownerPinInput === 'Chameleon_Tea123') {
```

**Replace with your password:**
```javascript
if (ownerPinInput === 'YourSecurePassword123!') {
```

**Password Recommendations:**
- ✅ At least 12 characters
- ✅ Mix of uppercase and lowercase
- ✅ Include numbers and symbols
- ✅ Not related to business name
- ✅ Don't share publicly

**Examples of strong passwords:**
- `Coffee@House2024Secure`
- `BestBakery#2024Pass`
- `MyBiz$Strong9Pass`

### Update Login Screen Text

**Find** (Line ~1608):
```html
<p className="text-xs...>
  Enter password to manage daily operations and customer data.
</p>
```

Keep or customize the login screen text.

---

## 6. Initial Data

### 6.1 Sample Orders (Optional)

**Find** `const INITIAL_ORDERS` (Line ~178):

You can:
- **Option A:** Keep for demo purposes
- **Option B:** Delete all sample orders to start fresh:

```javascript
const INITIAL_ORDERS = [];  // Empty array = no demo orders
```

### 6.2 Sample Reviews (Optional)

**Find** `const INITIAL_FEEDBACK` (Line ~217):

Same options:
```javascript
const INITIAL_FEEDBACK = [];  // Start with no reviews
```

**Or customize the sample reviews:**
```javascript
{
  id: 'fb-1',
  name: 'John D.',
  rating: 5,
  date: 'Today',
  category: 'Taste & Quality',
  comment: 'Best coffee in town! The latte art is amazing.',
  drink: 'Caramel Latte',
  ownerResponse: null
}
```

---

## 7. Advanced Customization

### 7.1 Taxes

**Find** (Line ~1080):
```javascript
const taxAmount = cartSubtotal * 0.095; // 9.5% local tax
```

**Update to your local tax rate:**
```javascript
const taxAmount = cartSubtotal * 0.08; // 8% tax
```

### 7.2 Size Pricing

**Find** "Select Size" section (Line ~1450):

```javascript
{['Regular', 'Large (+ $1.00)'].map(sz => {
```

**Change the price difference:**
```javascript
{['Regular', 'Large (+ $2.00)'].map(sz => {
  const isLarge = sz.includes('Large');
  const szValue = isLarge ? 'Large' : 'Regular';
  // Update price calculation below too!
```

**And update the calculation** (Line ~1011):
```javascript
if (options.size === 'Large') price += 2.00; // Changed from 1.00
```

### 7.3 Form Fields

**Ticket/Feedback Forms:**
You can add or remove fields in:
- Ticket submission form (Line ~855)
- Feedback form (Line ~910)
- Checkout form (Line ~1930)

**Example - Add "Address" field:**

```javascript
<div>
  <label className="block text-[11px] font-semibold text-slate-600 mb-1">
    Delivery Address
  </label>
  <input
    type="text"
    name="address"
    placeholder="123 Main St"
    className="w-full text-xs px-3 py-2 rounded-xl border..."
  />
</div>
```

### 7.4 Business Hours / Contact Info

Add a contact section in the home page (find Line ~700, insert after):

```javascript
<div className="bg-white rounded-2xl p-4 shadow-sm border border-slate-200">
  <h3 className="font-bold text-slate-900 text-sm mb-2">📍 Visit Us</h3>
  <p className="text-xs text-slate-600 leading-relaxed">
    <strong>Address:</strong> 123 Main Street, City, State 12345<br/>
    <strong>Hours:</strong> Mon-Fri 7am-8pm, Sat-Sun 8am-9pm<br/>
    <strong>Phone:</strong> (555) 123-4567<br/>
    <strong>Email:</strong> hello@yourbusiness.com
  </p>
</div>
```

---

## 8. Testing

### 8.1 Test Checklist

After customization, test EVERYTHING:

**Customer Flow:**
- [ ] Homepage loads correctly
- [ ] Menu displays all your products
- [ ] Categories filter properly
- [ ] Can customize a drink/product
- [ ] Add to cart works
- [ ] Checkout form accepts input
- [ ] Order success shows confetti
- [ ] Submit feedback form
- [ ] Submit support ticket
- [ ] All images load

**Owner Dashboard:**
- [ ] Can login with new password
- [ ] Failed login attempts trigger lockout
- [ ] See submitted orders
- [ ] Change order status
- [ ] Reply to feedback
- [ ] View support tickets
- [ ] Mark tickets resolved
- [ ] Export to sheets (may fail without API, that's ok)
- [ ] Backup data downloads
- [ ] Restore backup works
- [ ] Reset orders works
- [ ] Session timeout after 30 min

### 8.2 Mobile Testing

Test on actual devices or browser dev tools:
- [ ] iPhone (Safari)
- [ ] Android (Chrome)
- [ ] Tablet (iPad)
- [ ] Different screen sizes

### 8.3 Browser Testing

Test on:
- [ ] Chrome
- [ ] Firefox
- [ ] Safari
- [ ] Edge

---

## 🎯 Customization Examples

### Example 1: Coffee Shop

```javascript
// Business name
"Maria's Coffee House"

// Tagline
"ORGANIC BEANS • HANDCRAFTED BREWS"

// Colors
{
  coffee: {
    brown: '#6F4E37',    // Coffee brown
    cream: '#F5DEB3',    // Cream
    gold: '#D4AF37',     // Gold accent
    dark: '#2C1810'      // Dark brown
  }
}

// Menu categories
['All', 'Hot Coffee', 'Iced Coffee', 'Espresso', 'Pastries']

// Sample product
{
  id: 'm1',
  name: 'Signature Caramel Latte',
  category: 'Hot Coffee',
  price: 5.50,
  description: 'House-made caramel syrup with double shot espresso and steamed milk.',
  badge: 'Best Seller'
}

// Customizations
SWEETNESS_OPTIONS = ['No Sugar', 'Light', 'Regular', 'Extra']
ICE_OPTIONS = [] // Not needed for hot coffee
TOPPING_OPTIONS = [
  { name: 'Extra Shot', price: 1.50 },
  { name: 'Oat Milk', price: 0.75 },
  { name: 'Whipped Cream', price: 0.50 }
]
```

### Example 2: Bakery

```javascript
// Business name
"Sweet Dreams Bakery"

// Categories
['All', 'Cupcakes', 'Cakes', 'Cookies', 'Pastries']

// Sample product
{
  id: 'm1',
  name: 'Red Velvet Cupcake',
  category: 'Cupcakes',
  price: 4.50,
  description: 'Moist red velvet cake topped with cream cheese frosting.',
  badge: 'Customer Favorite'
}

// Customizations (optional for bakery)
SWEETNESS_OPTIONS = [] // Remove
ICE_OPTIONS = []       // Remove
TOPPING_OPTIONS = [
  { name: 'Extra Frosting', price: 0.50 },
  { name: 'Sprinkles', price: 0.25 },
  { name: 'Candle', price: 0.50 }
]
```

---

## 🆘 Troubleshooting

### "My changes don't show up"
- Hard refresh: Ctrl + Shift + R (Windows) or Cmd + Shift + R (Mac)
- Clear browser cache
- Check browser console for errors (F12)

### "I broke something"
- Restore from your backup copy
- Use Ctrl + Z to undo recent changes
- Check for missing commas, quotes, or brackets

### "Images don't load"
- Check image URL is correct
- Ensure URL starts with `https://`
- Try a different image source
- Check browser console (F12) for errors

### "Colors look weird"
- Verify hex codes are correct (`#RRGGBB`)
- Make sure you updated all references
- Check contrast for readability

### "Can't login to owner dashboard"
- Double-check password exactly matches code
- Case-sensitive!
- Check for extra spaces
- Clear localStorage and try again

---

## 📚 Additional Resources

- **HTML/CSS Basics:** [W3Schools](https://www.w3schools.com/)
- **JavaScript Basics:** [MDN Web Docs](https://developer.mozilla.org/en-US/docs/Web/JavaScript)
- **Tailwind CSS:** [Official Docs](https://tailwindcss.com/docs)
- **React Basics:** [React Docs](https://react.dev/)
- **Color Tools:** [Coolors](https://coolors.co/), [Adobe Color](https://color.adobe.com/)
- **Free Images:** [Unsplash](https://unsplash.com/), [Pexels](https://pexels.com/)

---

## 💡 Pro Tips

1. **Start Small** - Change one section at a time
2. **Test Often** - Check after each major change
3. **Keep Backups** - Save copies before big changes
4. **Use Comments** - Add notes in code for future reference
5. **Ask for Help** - Check GitHub Issues or documentation

---

## ✅ Ready to Deploy?

Once you've customized and tested everything:

1. [ ] All business information updated
2. [ ] Colors match your brand
3. [ ] Menu items are correct
4. [ ] Password changed
5. [ ] Tested on mobile & desktop
6. [ ] All features working
7. [ ] Images loading properly

**Next Step:** See [README.md](./README.md#deployment) for deployment instructions!

---

**Need more help?** Open an issue on GitHub or contact support!

*Happy customizing! 🎉*
