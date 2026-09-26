# Chamele-Tea Security & Architecture Recommendations

## ✅ Implemented Improvements

### 1. **Stronger Password**
- Changed from simple PIN "1234" to `Chameleon_Tea123`
- More secure but still hardcoded (see recommendations below)

### 2. **Data Persistence with localStorage**
- Orders, feedbacks, and tickets now persist across page refreshes
- Prevents data loss from browser crashes or accidental closure
- Data survives until browser cache is cleared

## 🔴 Critical Issues & Recommendations

### 1. **Authentication & Authorization**

#### Current Issues:
- Password hardcoded in client-side JavaScript (visible in source code)
- No session timeout or auto-logout
- No password hashing
- No multi-user support

#### **RECOMMENDED SOLUTIONS:**

**Option A: Backend Authentication (Best Practice)**
```javascript
// Implement proper backend login API
// Backend: Node.js/Express + JWT tokens
app.post('/api/owner/login', async (req, res) => {
  const { password } = req.body;
  const hashedPassword = await bcrypt.hash(password, 10);
  
  if (await bcrypt.compare(password, storedHashedPassword)) {
    const token = jwt.sign({ role: 'owner' }, SECRET_KEY, { expiresIn: '8h' });
    res.json({ token, expiresIn: 28800 });
  }
});
```

**Option B: Environment Variables (Moderate)**
```javascript
// Store password in .env file (not committed to git)
const OWNER_PASSWORD = process.env.REACT_APP_OWNER_PASSWORD;
```

**Option C: Session Management (Quick Fix)**
```javascript
// Add auto-logout after 30 minutes of inactivity
useEffect(() => {
  let timeout;
  const resetTimeout = () => {
    clearTimeout(timeout);
    timeout = setTimeout(() => {
      setIsOwnerAuthenticated(false);
      alert('Session expired. Please login again.');
    }, 30 * 60 * 1000); // 30 minutes
  };
  
  if (isOwnerAuthenticated) {
    resetTimeout();
    window.addEventListener('mousemove', resetTimeout);
    window.addEventListener('keypress', resetTimeout);
  }
  
  return () => {
    clearTimeout(timeout);
    window.removeEventListener('mousemove', resetTimeout);
    window.removeEventListener('keypress', resetTimeout);
  };
}, [isOwnerAuthenticated]);
```

**Option D: Rate Limiting (Add Now)**
```javascript
// Prevent brute force attacks
const [loginAttempts, setLoginAttempts] = useState(0);
const [lockoutTime, setLockoutTime] = useState(null);

const handleOwnerLogin = (e) => {
  e.preventDefault();
  
  // Check if locked out
  if (lockoutTime && Date.now() < lockoutTime) {
    const minutesLeft = Math.ceil((lockoutTime - Date.now()) / 60000);
    setPinError(`Too many failed attempts. Try again in ${minutesLeft} minutes.`);
    return;
  }
  
  if (ownerPinInput === 'Chameleon_Tea123') {
    setIsOwnerAuthenticated(true);
    setLoginAttempts(0);
    setLockoutTime(null);
    setPinError('');
  } else {
    const newAttempts = loginAttempts + 1;
    setLoginAttempts(newAttempts);
    
    if (newAttempts >= 5) {
      setLockoutTime(Date.now() + 15 * 60 * 1000); // 15 minute lockout
      setPinError('Too many failed attempts. Locked for 15 minutes.');
    } else {
      setPinError(`Incorrect password. ${5 - newAttempts} attempts remaining.`);
    }
  }
  setOwnerPinInput('');
};
```

---

### 2. **Data Security & Privacy**

#### Current Issues:
- PII (emails, phone numbers) stored in plain text
- No encryption
- Data visible in browser localStorage
- No GDPR compliance

#### **RECOMMENDED SOLUTIONS:**

**Option A: Backend Database (Best Practice)**
```
Tech Stack:
- PostgreSQL or MongoDB for data storage
- Node.js/Express or Python/Flask backend
- API endpoints with authentication
- Data encrypted at rest and in transit (HTTPS)
- Regular automated backups
```

**Option B: Client-Side Encryption (Quick Fix)**
```javascript
// Install crypto-js: npm install crypto-js
import CryptoJS from 'crypto-js';

const ENCRYPTION_KEY = 'your-secret-key-here'; // Store securely

// Encrypt before saving to localStorage
const saveEncrypted = (key, data) => {
  const encrypted = CryptoJS.AES.encrypt(
    JSON.stringify(data), 
    ENCRYPTION_KEY
  ).toString();
  localStorage.setItem(key, encrypted);
};

// Decrypt when loading
const loadEncrypted = (key) => {
  const encrypted = localStorage.getItem(key);
  if (!encrypted) return null;
  
  const decrypted = CryptoJS.AES.decrypt(encrypted, ENCRYPTION_KEY);
  return JSON.parse(decrypted.toString(CryptoJS.enc.Utf8));
};
```

**Option C: Data Minimization**
```javascript
// Only collect necessary data
// Mask sensitive information in UI
const maskPhone = (phone) => {
  return phone.replace(/(\d{3})\d{3}(\d{4})/, '$1-***-$2');
};

const maskEmail = (email) => {
  const [name, domain] = email.split('@');
  return `${name.substring(0, 2)}***@${domain}`;
};
```

**Option D: Data Retention Policy**
```javascript
// Auto-delete old data
useEffect(() => {
  const cleanupOldData = () => {
    const thirtyDaysAgo = Date.now() - (30 * 24 * 60 * 60 * 1000);
    
    // Remove orders older than 30 days
    setOrders(orders.filter(order => {
      const orderDate = new Date(order.time).getTime();
      return orderDate > thirtyDaysAgo;
    }));
    
    // Similar for feedbacks and tickets
  };
  
  // Run cleanup weekly
  const interval = setInterval(cleanupOldData, 7 * 24 * 60 * 60 * 1000);
  return () => clearInterval(interval);
}, []);
```

---

### 3. **Google Sheets Export**

#### Current Issues:
- No Google OAuth authentication
- API calls will fail from browser (CORS)
- Can't set proper sheet permissions

#### **RECOMMENDED SOLUTIONS:**

**Option A: Backend Proxy (Best Practice)**
```
Architecture:
1. Frontend sends data to your backend API
2. Backend authenticates with Google using Service Account
3. Backend creates sheet and sets proper permissions
4. Backend returns sheet URL to frontend

Benefits:
- Secure API credentials
- Control sheet permissions (private by default)
- No CORS issues
- Audit logging
```

**Option B: Google Apps Script (No Backend)**
```javascript
// Deploy this as a Google Apps Script Web App
function doPost(e) {
  const data = JSON.parse(e.postData.contents);
  const ss = SpreadsheetApp.create('Chamele-Tea Report - ' + new Date().toLocaleDateString());
  
  // Create sheets and populate data
  const ordersSheet = ss.getSheets()[0];
  ordersSheet.setName('Orders');
  // ... populate data
  
  // Set permissions
  ss.addEditor('owner@chamele-tea.com');
  
  return ContentService.createTextOutput(JSON.stringify({
    url: ss.getUrl()
  }));
}

// Frontend calls this:
fetch('YOUR_APPS_SCRIPT_URL', {
  method: 'POST',
  body: JSON.stringify({ orders, feedbacks, tickets })
});
```

**Option C: Use Excel Export Instead (No API)**
```javascript
// Install SheetJS: npm install xlsx
import * as XLSX from 'xlsx';

const exportToExcel = () => {
  const workbook = XLSX.utils.book_new();
  
  // Orders sheet
  const ordersData = orders.map(o => ({
    'Order ID': o.id,
    'Customer': o.customerName,
    'Phone': o.phone,
    'Total': o.total,
    // ... more fields
  }));
  const ordersSheet = XLSX.utils.json_to_sheet(ordersData);
  XLSX.utils.book_append_sheet(workbook, ordersSheet, 'Orders');
  
  // Similar for other sheets
  
  // Download
  XLSX.writeFile(workbook, `chamele-tea-report-${new Date().toISOString().split('T')[0]}.xlsx`);
};
```

---

### 4. **Backup & Recovery**

#### **RECOMMENDED SOLUTIONS:**

**Option A: Automatic Backup (Implement Now)**
```javascript
// Export backup button in owner dashboard
const createBackup = () => {
  const backup = {
    version: '1.0',
    timestamp: new Date().toISOString(),
    data: {
      orders,
      feedbacks,
      tickets
    }
  };
  
  const blob = new Blob([JSON.stringify(backup, null, 2)], { type: 'application/json' });
  const url = URL.createObjectURL(blob);
  const link = document.createElement('a');
  link.href = url;
  link.download = `chamele-tea-backup-${Date.now()}.json`;
  link.click();
};

// Restore from backup
const restoreBackup = (file) => {
  const reader = new FileReader();
  reader.onload = (e) => {
    const backup = JSON.parse(e.target.result);
    if (confirm(`Restore backup from ${backup.timestamp}? This will overwrite current data.`)) {
      setOrders(backup.data.orders);
      setFeedbacks(backup.data.feedbacks);
      setTickets(backup.data.tickets);
    }
  };
  reader.readAsText(file);
};
```

**Option B: Cloud Backup**
```javascript
// Use Firebase/Supabase for real-time backup
import { initializeApp } from 'firebase/app';
import { getFirestore, collection, addDoc } from 'firebase/firestore';

// Auto-save to cloud every 5 minutes
useEffect(() => {
  const backup = setInterval(async () => {
    await addDoc(collection(db, 'backups'), {
      timestamp: Date.now(),
      orders,
      feedbacks,
      tickets
    });
  }, 5 * 60 * 1000);
  
  return () => clearInterval(backup);
}, [orders, feedbacks, tickets]);
```

---

### 5. **Input Validation & Security**

#### **RECOMMENDED SOLUTIONS:**

**Add Input Sanitization**
```javascript
// Install DOMPurify: npm install dompurify
import DOMPurify from 'dompurify';

const sanitizeInput = (input) => {
  return DOMPurify.sanitize(input, { ALLOWED_TAGS: [] });
};

// Use in forms
const handleTicketSubmit = (e) => {
  e.preventDefault();
  const newTicket = {
    // ... other fields
    message: sanitizeInput(ticketForm.message),
    firstName: sanitizeInput(ticketForm.firstName),
    lastName: sanitizeInput(ticketForm.lastName),
  };
  // ...
};
```

**Email & Phone Validation**
```javascript
const validateEmail = (email) => {
  const re = /^[^\s@]+@[^\s@]+\.[^\s@]+$/;
  return re.test(email);
};

const validatePhone = (phone) => {
  const re = /^[\d\s\-\(\)]+$/;
  return re.test(phone) && phone.replace(/\D/g, '').length >= 10;
};

// Use in form submission
if (!validateEmail(ticketForm.email)) {
  alert('Please enter a valid email address');
  return;
}
```

---

## 📊 Priority Implementation Roadmap

### **Phase 1: Immediate (This Week)**
1. ✅ Add localStorage persistence
2. ✅ Implement stronger password
3. ⏳ Add login rate limiting (5 attempts, 15 min lockout)
4. ⏳ Add session timeout (30 minutes)
5. ⏳ Add data backup/restore buttons
6. ⏳ Add input validation and sanitization

### **Phase 2: Short-term (Next 2 Weeks)**
1. Switch to Excel export (no API needed)
2. Implement client-side encryption for localStorage
3. Add data masking for sensitive fields
4. Add confirmation dialogs for destructive actions
5. Implement auto-cleanup of old data

### **Phase 3: Medium-term (1-2 Months)**
1. Build backend API (Node.js/Express)
2. Set up PostgreSQL database
3. Implement proper authentication (JWT)
4. Set up HTTPS/SSL
5. Add audit logging

### **Phase 4: Long-term (3+ Months)**
1. Multi-location support
2. Employee accounts with roles/permissions
3. Real-time order notifications
4. Customer accounts & loyalty program
5. Payment processing integration
6. Mobile app (React Native)
7. Analytics dashboard
8. GDPR compliance features

---

## 🔒 Security Checklist

- ✅ Strong password implemented
- ✅ Data persistence with localStorage
- ⏳ Session timeout
- ⏳ Rate limiting
- ⏳ Input validation
- ⏳ Data encryption
- ⏳ Backup system
- ❌ Backend database
- ❌ HTTPS
- ❌ Audit logs
- ❌ GDPR compliance

---

## 💡 Quick Wins You Can Implement Today

1. **Add these security headers in your HTML:**
```html
<meta http-equiv="Content-Security-Policy" content="default-src 'self'; script-src 'self' 'unsafe-inline' https://cdn.tailwindcss.com https://unpkg.com https://apis.google.com https://cdn.jsdelivr.net; style-src 'self' 'unsafe-inline' https://fonts.googleapis.com; font-src https://fonts.gstatic.com;">
```

2. **Add a clear data button for privacy:**
```javascript
const clearAllData = () => {
  if (confirm('⚠️ This will permanently delete all orders, feedback, and tickets. Continue?')) {
    if (confirm('Are you absolutely sure? This cannot be undone.')) {
      localStorage.clear();
      window.location.reload();
    }
  }
};
```

3. **Add a privacy notice:**
```javascript
// Show on first visit
useEffect(() => {
  if (!localStorage.getItem('privacy_acknowledged')) {
    alert('This system stores customer data in your browser. Please ensure you are on a secure, private device.');
    localStorage.setItem('privacy_acknowledged', 'true');
  }
}, []);
```

---

## 📞 Contact for Implementation Help

Need help implementing any of these recommendations? Consider:
- Hiring a backend developer
- Using managed services (Firebase, Supabase)
- Security audit before launch
- Legal review for data privacy compliance
