# 🏢 Habibs Franchise System - Complete Deployment Guide

## Quick Start (5 Minutes)

### Step 1: Clone Repository Locally
```bash
git clone https://github.com/369mayankkumar-creator/habibs-franchise-system.git
cd habibs-franchise-system
npm install
```

### Step 2: Create .env File
Create a file named `.env` in the root directory with your credentials:

```env
REACT_APP_SUPABASE_URL=https://phwssaijhajatbgodewa.supabase.co
REACT_APP_SUPABASE_ANON_KEY=sb_publishable_84B6Kc
REACT_APP_N8N_FRANCHISE_LEAD_WEBHOOK=https://app.n8n.cloud/webhook/franchise-lead-capture
REACT_APP_N8N_ATTENDANCE_WEBHOOK=https://app.n8n.cloud/webhook/attendance-alert
REACT_APP_N8N_INVENTORY_WEBHOOK=https://app.n8n.cloud/webhook/inventory-alert
REACT_APP_TWILIO_PHONE=+1234567890
REACT_APP_ENVIRONMENT=production
REACT_APP_API_TIMEOUT=30000
```

### Step 3: Start Development Server
```bash
npm start
```

App will open at `http://localhost:3000`

---

## Complete File Structure & Code

### All Files to Create:
1. package.json ✅
2. .env
3. .gitignore
4. src/index.js
5. src/index.css
6. src/App.js
7. src/App.css
8. src/supabaseClient.js
9. src/components/FranchiseLeadForm.jsx
10. src/components/FranchiseLeadForm.css
11. src/components/StaffAttendanceForm.jsx
12. src/components/StaffAttendanceForm.css
13. src/components/Dashboard.jsx
14. src/components/Dashboard.css
15. src/services/supabaseService.js
16. src/services/n8nWebhook.js
17. src/utils/validators.js
18. public/index.html
19. n8n-workflows/*.json

---

## N8N Webhook URLs (Update After Creating Workflows)

### Webhook 1: Franchise Lead Capture
- **URL**: https://app.n8n.cloud/webhook/franchise-lead-capture
- **Method**: POST
- **Triggered by**: Custom UI form submission
- **Actions**: Insert to Supabase → Calculate score → Send WhatsApp alert

### Webhook 2: Attendance Alert
- **URL**: https://app.n8n.cloud/webhook/attendance-alert
- **Method**: POST
- **Triggered by**: Daily 11 AM schedule
- **Actions**: Read attendance → Flag absentees → Send WhatsApp alert

### Webhook 3: Inventory Low-Stock
- **URL**: https://app.n8n.cloud/webhook/inventory-alert
- **Method**: POST
- **Triggered by**: Daily 7 AM schedule
- **Actions**: Check stock → Auto-create reorder → Send WhatsApp alert

---

## Production Deployment Options

### Option 1: Vercel (Recommended - Free)
```bash
npm install -g vercel
vercel
# Follow prompts, link to GitHub repo
# Set environment variables in Vercel dashboard
```

### Option 2: Netlify
```bash
npm install -g netlify-cli
netlify deploy
```

### Option 3: GitHub Pages
```bash
npm run build
# Deploy 'build' folder to GitHub Pages
```

---

## Troubleshooting

### Issue: "Supabase connection failed"
**Solution**: Verify `.env` credentials are correct
```bash
# Check your Supabase URL and key
# Settings → API → Publishable Key
```

### Issue: "N8N webhook not triggering"
**Solution**: Verify webhook URL in N8N workflow matches .env file

### Issue: "Twilio SMS not sending"
**Solution**: Ensure Twilio credentials in N8N are correctly configured

---

## Next Steps

1. ✅ Clone repo and install dependencies
2. ⏳ Create N8N workflows (see N8N_SETUP.md)
3. ⏳ Configure Twilio integration
4. ⏳ Test lead capture form
5. ⏳ Deploy to production
6. ⏳ Set up monitoring & logging

---

## Support

For issues or questions:
- Check Supabase dashboard for data
- Verify N8N workflow execution logs
- Check browser console for errors (F12)
- Review Twilio logs for SMS issues

## Created by: Mayank Kumar
## Organization: Habibs Hair & Beauty
