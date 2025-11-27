# Habibs Franchise System - Zapier Alternative: Supabase Edge Functions Setup

## STATUS: ✅ WORKING & TESTED

**Edge Function Deployed**: `process-franchise-lead`  
**Function URL**: `https://phwssaijhajatbgodewa.supabase.co/functions/v1/process-franchise-lead`  
**Last Updated**: November 28, 2025

---

## What Was Changed From Original Plan

**Original Plan**: Use Zapier (but Webhooks feature is Premium-only on free tier)  
**New Solution**: Supabase Edge Functions (100% free, serverless, deployed globally)

### Why This is Better

✅ Completely FREE (no credit card required)  
✅ Serverless (no infrastructure management)  
✅ Integrated with Supabase (same project)  
✅ Can send WhatsApp via Twilio + Email via SendGrid  
✅ Global deployment (fast response times)  
✅ Built-in logging and monitoring

---

## System Architecture

```
React Form (localhost:3000)
         ↓ (POST JSON)
Supabase Edge Function
         ↓
   ├→ Save to Supabase Database
   ├→ Calculate Lead Score
   ├→ Send WhatsApp (Twilio) [Optional]
   └→ Send Email (SendGrid) [Optional]
         ↓
  JSON Response with Score
```

---

## Implementation Steps

### Step 1: Edge Function is Already Deployed ✅

The Edge Function `process-franchise-lead` is deployed and TESTED.

**Test Results**:
```json
{
  "success": true,
  "leadName": "Mayank Kumar",
  "score": 80,
  "whatsappSent": false,  // Will work once Twilio credentials are added
  "emailSent": false      // Will work once SendGrid credentials are added
}
```

### Step 2: Update React App (LOCAL CODE CHANGE)

In your `src/App.js` or form component, update the POST endpoint:

**FIND THIS LINE**:
```javascript
const response = await fetch(process.env.REACT_APP_SUPABASE_URL + '/rest/v1/franchise_leads', {
```

**CHANGE TO THIS**:
```javascript
const response = await fetch('https://phwssaijhajatbgodewa.supabase.co/functions/v1/process-franchise-lead', {
```

### Step 3: Update .env File

Add the Edge Function webhook URL:
```
REACT_APP_EDGE_FUNCTION_WEBHOOK=https://phwssaijhajatbgodewa.supabase.co/functions/v1/process-franchise-lead
```

### Step 4: Test End-to-End

1. Start React app: `npm start`
2. Navigate to `http://localhost:3000`
3. Fill in the Franchise Lead form
4. Submit the form
5. Check the browser console for response
6. Verify data appears in Supabase database

---

## Adding Twilio WhatsApp Integration

### Setup Twilio Credentials

1. Go to Supabase → Edge Functions → Secrets
2. Add the following secrets:

```
TWILIO_ACCOUNT_SID = your_twilio_account_sid
TWILIO_AUTH_TOKEN = your_twilio_auth_token
TWILIO_PHONE = +1234567890  // Your Twilio WhatsApp-enabled number
FRANCHISE_PHONE = +91XXXXXXXXXX  // Franchise owner's phone
```

3. The function will automatically send WhatsApp messages to the franchise owner

---

## Adding SendGrid Email Integration

### Setup SendGrid Credentials

1. Get API key from SendGrid (https://sendgrid.com)
2. Add to Supabase Edge Function Secrets:

```
SENDGRID_API_KEY = your_sendgrid_api_key
FRANCHISE_EMAIL = franchise@habibs.com
```

3. The function will send email notifications with lead details and score

---

## Monitoring & Debugging

### View Function Logs

1. Open Supabase Dashboard
2. Go to Edge Functions → process-franchise-lead → Logs
3. See real-time execution logs, errors, and performance metrics

### Test Function Directly

1. In Edge Functions → process-franchise-lead → Test
2. Send test requests with sample data
3. See response immediately

---

## Lead Scoring Logic

The function calculates lead quality score (0-100):

**Budget Scoring**:
- ₹50L+ → 50 points
- ₹25L-50L → 40 points
- ₹10L-25L → 30 points
- ₹5L-10L → 20 points
- <₹5L → 10 points

**Experience Scoring**:
- 5+ years → 30 points
- 2-5 years → 20 points
- 0-2 years → 10 points
- No experience → 5 points

**Final Score** = Budget Score + Experience Score

---

## Deployment to Production

### Option 1: Vercel (Recommended)

```bash
npm install -g vercel
vercel login
vercel deploy
```

### Option 2: Netlify

```bash
npm run build
netlify deploy --prod --dir=build
```

### Option 3: Self-Hosted

Use nginx/Apache to serve the build folder.

---

## Troubleshooting

**Q: Form submission returns 500 error**
- Check Edge Function logs in Supabase dashboard
- Verify .env variables are correct
- Ensure Edge Function secrets are set

**Q: WhatsApp messages not sending**
- Verify Twilio credentials in Edge Function secrets
- Check if Twilio account has WhatsApp sandbox enabled
- Check Edge Function logs for API errors

**Q: Emails not sending**
- Verify SendGrid API key is correct
- Check SendGrid account status and limits
- Verify `FRANCHISE_EMAIL` is set correctly

---

## Next Steps

1. ✅ Edge Function deployed and tested
2. ⏳ Update React App form to use Edge Function URL
3. ⏳ Test end-to-end with form submission
4. ⏳ Add Twilio credentials for WhatsApp
5. ⏳ Add SendGrid credentials for email
6. ⏳ Deploy to production

---

## Support

For issues or questions:
- Check Edge Function logs: Supabase → Edge Functions → process-franchise-lead → Logs
- Review this documentation
- Check console logs in browser Dev Tools

---

**Created**: November 28, 2025
