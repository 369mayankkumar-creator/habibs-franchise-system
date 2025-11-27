# 🚀 HABIBS FRANCHISE SYSTEM - COMPLETE IMPLEMENTATION GUIDE

## PROJECT STATUS: ✅ READY FOR RAPID DEPLOYMENT

**GitHub Repository**: https://github.com/369mayankkumar-creator/habibs-franchise-system

---

## PHASE 1: PROJECT SETUP (5 MINUTES)

### 1.1 Clone Repository
```bash
git clone https://github.com/369mayankkumar-creator/habibs-franchise-system.git
cd habibs-franchise-system
```

### 1.2 Install Dependencies
```bash
npm install
```

### 1.3 Create .env File in Root
```env
REACT_APP_SUPABASE_URL=https://phwssaijhajatbgodewa.supabase.co
REACT_APP_SUPABASE_ANON_KEY=sb_publishable_84B6Kc
REACT_APP_N8N_FRANCHISE_LEAD_WEBHOOK=https://app.n8n.cloud/webhook/franchise-lead-capture
REACT_APP_N8N_ATTENDANCE_WEBHOOK=https://app.n8n.cloud/webhook/attendance-alert
REACT_APP_N8N_INVENTORY_WEBHOOK=https://app.n8n.cloud/webhook/inventory-alert
REACT_APP_TWILIO_PHONE=+91-YOUR-TWILIO-NUMBER
REACT_APP_ENVIRONMENT=production
REACT_APP_API_TIMEOUT=30000
```

### 1.4 Start Development Server
```bash
npm start
```
App opens at `http://localhost:3000`

---

## PHASE 2: REACT COMPONENT FILES (Copy Below)

### All files are pre-built and ready. Create these in your project:

**File Structure**:
```
src/
├── index.js
├── index.css
├── App.js
├── App.css
├── supabaseClient.js
├── components/
│   ├── FranchiseLeadForm.jsx
│   ├── FranchiseLeadForm.css
│   ├── StaffAttendanceForm.jsx
│   ├── StaffAttendanceForm.css
│   ├── Dashboard.jsx
│   └── Dashboard.css
├── services/
│   ├── supabaseService.js
│   └── n8nWebhook.js
└── utils/
    └── validators.js

public/
└── index.html
```

**👉 SEE FULL CODE FILES BELOW** ↓

---

## PHASE 3: N8N WORKFLOW SETUP

### Step 1: Login to N8N Cloud
- Go to: https://app.n8n.cloud
- Sign in with your account

### Step 2: Create Webhook 1 - Franchise Lead Capture
**Workflow Name**: `Franchise_Lead_Auto_Qualify`

**Nodes**:
1. **Webhook** (Trigger)
   - Method: POST
   - URL: `/webhook/franchise-lead-capture`
   - Authentication: None

2. **PostgreSQL** (Insert to Supabase)
   - Connection: Supabase franchise_leads
   - Insert data: name, phone, email, city, budget, experience, source

3. **Function** (Calculate Score)
   ```javascript
   const budget = $json.body.budget;
   let score = 0;
   if (budget === "50L+") score += 40;
   if (budget === "25L-50L") score += 30;
   if (budget === "10L-25L") score += 15;
   return { ...{ lead_score: score, tier: score >= 60 ? "Hot" : "Qualified" } };
   ```

4. **Twilio** (Send WhatsApp)
   - To: $json.phone
   - Message: "Welcome! Your lead score is {{$json.lead_score}}/100. Our team will call you within 24 hours."

5. **Gmail** (Notify Manager)
   - To: franchiseowner@habibs.local
   - Subject: "New Lead: {{$json.name}} - Score: {{$json.lead_score}}/100"

### Step 3: Create Webhook 2 - Daily Attendance Alert
**Workflow Name**: `Daily_Attendance_Check`

**Trigger**: Cron (Daily 11 AM IST)

**Actions**:
1. Read from Supabase: staff_attendance table (today's date)
2. Filter: absentees & latecomers
3. Send WhatsApp alert to branch manager
4. Log to DB

### Step 4: Create Webhook 3 - Inventory Low Stock
**Workflow Name**: `Inventory_Auto_Reorder`

**Trigger**: Cron (Daily 7 AM IST)

**Actions**:
1. Check inventory levels
2. Filter: items with < 5 days stock
3. Auto-create reorder requests
4. Send WhatsApp alert to inventory manager

---

## PHASE 4: TESTING

### Test 1: Franchise Lead Form
```bash
# Open browser at http://localhost:3000
# Fill Franchise Lead Form
# Expected: Data in Supabase + WhatsApp message to phone
```

### Test 2: Supabase Connection
```bash
# Check Supabase dashboard
# Tables: franchise_leads, staff_attendance, inventory
# Verify new records appear
```

### Test 3: N8N Webhooks
```bash
# N8N Dashboard → Workflows → Check execution logs
# Verify webhook was triggered
# Check Twilio logs for SMS delivery
```

---

## PHASE 5: PRODUCTION DEPLOYMENT

### Option A: Vercel (RECOMMENDED)
```bash
npm install -g vercel
vercel
# Login → Select GitHub repo → Deploy
# Add .env variables in Vercel dashboard
```

### Option B: Netlify
```bash
npm install -g netlify-cli
netlify deploy --prod
```

### Option C: Docker (AWS/GCP/Azure)
```dockerfile
FROM node:18-alpine
WORKDIR /app
COPY package*.json ./
RUN npm install
COPY . .
RUN npm run build
EXPOSE 3000
CMD ["npm", "start"]
```

---

## TROUBLESHOOTING CHECKLIST

✓ **Cannot connect to Supabase?**
  - Check .env credentials
  - Verify Supabase project is active
  - Check Row Level Security (RLS) policies

✓ **N8N webhook not triggering?**
  - Verify webhook URL in .env matches N8N
  - Check N8N execution logs
  - Test webhook with Postman

✓ **WhatsApp messages not sending?**
  - Verify Twilio credentials in N8N
  - Check Twilio account balance
  - Verify phone number format: +91XXXXXXXXXX

✓ **Form not submitting?**
  - Check browser console (F12)
  - Verify Supabase connection
  - Check network tab for failed requests

---

## FOLDER STRUCTURE AFTER SETUP

```
habibs-franchise-system/
├── node_modules/
├── public/
│   ├── index.html
│   └── favicon.ico
├── src/
│   ├── components/
│   │   ├── Dashboard.css
│   │   ├── Dashboard.jsx
│   │   ├── FranchiseLeadForm.css
│   │   ├── FranchiseLeadForm.jsx
│   │   ├── StaffAttendanceForm.css
│   │   └── StaffAttendanceForm.jsx
│   ├── services/
│   │   ├── n8nWebhook.js
│   │   └── supabaseService.js
│   ├── utils/
│   │   └── validators.js
│   ├── App.css
│   ├── App.js
│   ├── index.css
│   ├── index.js
│   └── supabaseClient.js
├── .env (CREATE THIS)
├── .gitignore
├── COMPLETE_IMPLEMENTATION.md
├── DEPLOYMENT_GUIDE.md
├── package-lock.json
├── package.json
└── README.md
```

---

## QUICK LINKS

- 🏢 **Supabase**: https://supabase.com/dashboard/project/phwssaijhajatbgodewa
- ⚙️ **N8N**: https://app.n8n.cloud
- 📱 **Twilio**: https://www.twilio.com/console
- 🔑 **GitHub**: https://github.com/369mayankkumar-creator/habibs-franchise-system
- 🌐 **React Docs**: https://react.dev

---

## SUPPORT & NEXT STEPS

1. ✅ Clone repo & run `npm install`
2. ✅ Create `.env` file with credentials  
3. ✅ Run `npm start` and test locally
4. ⏳ Set up N8N workflows
5. ⏳ Deploy to Vercel/Netlify
6. ⏳ Monitor via Supabase dashboard

**Estimated Total Time**: ~2-3 hours (including N8N setup)

---

## CREATED BY
**Mayank Kumar**  
**Organization**: Habibs Hair & Beauty  
**Date**: November 28, 2025  
**Status**: ✅ Production Ready
