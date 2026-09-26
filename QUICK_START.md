# 🚀 Quick Start Guide

**Get Your Business Online in 1 Hour!**

This is the fastest path from download to deployed website. Follow these steps in order.

---

## ⏱️ 60-Minute Launch Plan

### Minutes 0-10: Download & Setup
1. ✅ Download all files to a folder
2. ✅ Open `index.html` in your browser to see it working
3. ✅ Open `index.html` in a text editor (Notepad, VSCode, etc.)

### Minutes 10-25: Essential Customization
1. ✅ **Change business name** (Search for "Chamele-Tea", replace all)
2. ✅ **Change owner password** (Search for "Chameleon_Tea123", replace)
3. ✅ Save and test - refresh browser to see changes

### Minutes 25-45: Menu Setup
1. ✅ **Update menu items** (Find `INITIAL_MENU` around line 85)
2. ✅ Change product names, prices, descriptions
3. ✅ Update image URLs (use Unsplash or your own)
4. ✅ Save and test - check menu displays correctly

### Minutes 45-55: Deploy
1. ✅ Go to [github.com](https://github.com) and sign up/login
2. ✅ Create new repository (name: your-business-name)
3. ✅ Upload `index.html` file
4. ✅ Settings → Pages → Enable GitHub Pages
5. ✅ Get your URL: `username.github.io/your-business-name`

### Minutes 55-60: Launch!
1. ✅ Visit your live site
2. ✅ Test placing an order
3. ✅ Login to owner dashboard (with your new password)
4. ✅ Share your URL!

---

## 🎯 Minimum Viable Customization

### Must Change (Critical):
- [ ] Business name (everywhere)
- [ ] Owner password
- [ ] Menu items and prices

### Should Change (Important):
- [ ] Product images
- [ ] Product descriptions
- [ ] Categories
- [ ] Tagline/slogan

### Nice to Change (Optional):
- [ ] Colors
- [ ] Logo
- [ ] Tax rate
- [ ] Initial demo data

---

## 📝 Copy-Paste Customization

### 1. Find This:
```javascript
'Chamele-Tea'
```
### Replace With:
```javascript
'Your Business Name'
```
**Locations:** Lines ~50, ~250, ~320

---

### 2. Find This:
```javascript
if (ownerPinInput === 'Chameleon_Tea123') {
```
### Replace With:
```javascript
if (ownerPinInput === 'YourPassword123!') {
```
**Location:** Line ~413

---

### 3. Find This:
```javascript
const INITIAL_MENU = [
  {
    id: 'm1',
    name: 'Chameleon Signature Brown Sugar',
    category: 'Milk Teas',
    price: 6.25,
```
### Replace With Your Items:
```javascript
const INITIAL_MENU = [
  {
    id: 'm1',
    name: 'Your Product Name',
    category: 'Your Category',
    price: 5.99,
```
**Location:** Line ~85

---

## ✅ Testing Checklist

### Before Deploying:
- [ ] Site loads without errors (check F12 console)
- [ ] All menu items display correctly
- [ ] Can add items to cart
- [ ] Can complete checkout
- [ ] Can login with new password
- [ ] Wrong password triggers lockout after 5 attempts
- [ ] All your business info is correct

### After Deploying:
- [ ] Live site loads
- [ ] Test on mobile phone
- [ ] Test placing an order
- [ ] Test owner dashboard
- [ ] Share URL with friend to test

---

## 🆘 Quick Troubleshooting

### "I broke something!"
**Solution:** Restore from backup or re-download

### "Changes don't show"
**Solution:** Hard refresh (Ctrl + Shift + R)

### "Can't login to owner dashboard"
**Solution:** Check password matches exactly (case-sensitive!)

### "Images don't load"
**Solution:** Verify image URLs start with `https://`

### "Site won't deploy"
**Solution:** Make sure filename is exactly `index.html` (lowercase)

---

## 📞 Need More Help?

- 📖 **Detailed Guide:** See [CUSTOMIZATION.md](./CUSTOMIZATION.md)
- 🚀 **Deployment Help:** See [DEPLOYMENT.md](./DEPLOYMENT.md)
- 🔒 **Security Info:** See [SECURITY_RECOMMENDATIONS.md](./SECURITY_RECOMMENDATIONS.md)
- 💼 **Business Info:** See [BUSINESS_PACKAGE.md](./BUSINESS_PACKAGE.md)

---

## 🎉 You're Done!

Your business is now online! 

**Next steps:**
1. Share your URL on social media
2. Add to Google My Business
3. Create QR code for in-store
4. Start taking orders!

**Questions?** Check the documentation or reach out for support.

---

**Built with ❤️ for busy business owners who want results fast.**

*Total time: ~1 hour • Total cost: $0-15/year • Total impact: Priceless*
