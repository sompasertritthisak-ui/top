# VisaFlow Pro — Complete Deployment Guide

## Project Overview
A fully automated visa application platform with frontend forms, real-time status tracking, admin dashboard, and Google Apps Script backend.

**Files:**
- `visa-section1-landing.html` — Marketing landing page
- `visa-section2-form.html` — Multi-step application form
- `visa-section3-status.html` — Application tracker
- `visa-section4-admin.html` — Admin dashboard
- `visa-section5-apps-script.js` — Google Apps Script backend
- `.env.example` — Environment configuration template

---

## PHASE 1: Setup Google Infrastructure

### Step 1: Create Google Sheet
1. Go to [sheets.google.com](https://sheets.google.com)
2. Create new spreadsheet: "VisaFlow Pro — Applications"
3. Rename first sheet to "Applications"
4. Copy the Sheet ID from URL: `https://docs.google.com/spreadsheets/d/{SHEET_ID}/edit`

### Step 2: Create Google Drive Folder
1. Go to [drive.google.com](https://drive.google.com)
2. Create folder: "VisaFlow Pro — Applicant Files"
3. Right-click → Share → Change to "Anyone with link can view"
4. Copy Folder ID from URL: `https://drive.google.com/drive/folders/{FOLDER_ID}`

### Step 3: Deploy Google Apps Script
1. Go to [script.google.com](https://script.google.com)
2. Create new project: "VisaFlow Pro Backend"
3. Copy entire content from `visa-section5-apps-script.js`
4. Update CONFIG object with:
   - `SHEET_ID`: from Step 1
   - `DRIVE_FOLDER_ID`: from Step 2
   - `ADMIN_EMAIL`: your admin email
   - `SUPPORT_EMAIL`: support email
   - `SITE_URL`: your site domain (e.g., `https://yoursite.com`)
5. Click **Deploy** → **New deployment** → Type: **Web app**
   - Execute as: Your email
   - Who has access: **Anyone**
6. Copy the **Deployment URL** (looks like: `https://script.google.com/macros/s/YOUR_ID/exec`)

### Step 4: Update Forms with Apps Script URL
In `visa-section2-form.html`, find the `submitApplication()` function and update:

```javascript
const APPS_SCRIPT_URL = 'https://script.google.com/macros/s/YOUR_DEPLOYMENT_ID/exec';
```

---

## PHASE 2: Deploy Frontend

### Option A: Deploy to GitHub Pages
1. Push all HTML files to your repository
2. Go to **Repository Settings** → **Pages**
3. Set source to `main` branch
4. Site will be available at `https://yourusername.github.io/top`

### Option B: Deploy to Custom Domain
1. Upload HTML files to your web server (Apache, Nginx, etc.)
2. Update internal links in all files to use correct URLs:
   ```html
   <!-- Change from -->
   <a href="visa-section2-form.html">
   
   <!-- To -->
   <a href="https://yourdomain.com/visa-section2-form.html">
   ```
3. Update `CONFIG.SITE_URL` in Apps Script accordingly

### Option C: Deploy to Vercel/Netlify
```bash
npm install -g vercel
vercel --prod
```

---

## PHASE 3: Configuration & Testing

### Create Environment File (.env)
```ini
SHEET_ID=YOUR_GOOGLE_SHEET_ID
DRIVE_FOLDER_ID=YOUR_GOOGLE_DRIVE_FOLDER_ID
ADMIN_EMAIL=admin@yourcompany.com
SUPPORT_EMAIL=support@yourcompany.com
COMPANY_NAME=VisaFlow Pro
SITE_URL=https://yourdomain.com
APPS_SCRIPT_URL=https://script.google.com/macros/s/YOUR_ID/exec
WHATSAPP_NUMBER=1234567890
```

### Test the Workflow
1. Go to landing page (`visa-section1-landing.html`)
2. Click "Start Application" → Select visa type → Continue
3. Fill in form details → Submit
4. Check Google Sheet for new row
5. Check admin dashboard for pending applications
6. Update status to "Approved" → Check inbox for email

### Verify Automation
- ✅ Application submitted to Google Sheet
- ✅ Drive folder created with subfolders
- ✅ Confirmation email sent to applicant
- ✅ Admin notification sent
- ✅ Application ID generated
- ✅ Status tracker works

---

## PHASE 4: Customization

### Customize Emails
Edit the HTML templates in `sendApplicantConfirmation()` and `sendStatusUpdateEmail()`:
```javascript
const subject = `Your Custom Subject — ${appId}`;
const body = `Your custom HTML email body...`;
```

### Add More Visa Types
In `visa-section2-form.html`, add to the visa-type radio cards:
```html
<label class="radio-card">
  <input type="radio" name="visaType" value="newtype">
  <div class="radio-card-label"><span class="rc-icon">🎯</span><span class="rc-text">New Type</span></div>
</label>
```

### Add Countries
Update the destination select in Step 1:
```html
<option value="countrycode">🏳️ Country Name</option>
```

### Custom Branding
Find `:root` CSS variables and update colors:
```css
:root {
  --gold: #YOUR_COLOR;
  --navy: #YOUR_COLOR;
  /* etc */
}
```

---

## PHASE 5: Production Checklist

- [ ] Google Sheet set up with headers
- [ ] Drive folder created with sharing enabled
- [ ] Apps Script deployed with correct CONFIG
- [ ] Frontend uploaded to web server
- [ ] All internal links updated
- [ ] HTTPS enabled (if custom domain)
- [ ] Tested full workflow (submit → email → admin)
- [ ] Whatsapp number added
- [ ] Admin email verified
- [ ] Daily reminder triggers set up
- [ ] Error logging configured
- [ ] Backups scheduled

---

## PHASE 6: Monitor & Maintain

### Daily Tasks
- Check admin dashboard for new applications
- Respond to missing document requests
- Update application statuses
- Monitor email delivery

### Weekly Tasks
- Review analytics in admin dashboard
- Check error logs in Google Sheet
- Update testimonials if needed
- Analyze conversion rates

### Monthly Tasks
- Backup Google Sheet data
- Review and optimize email templates
- Check visa processing times by country
- Update country requirements if changed

---

## Troubleshooting

### Issue: Form not submitting
**Solution:** Check browser console for errors, verify Apps Script URL is correct

### Issue: Emails not sending
**Solution:** Ensure Gmail API enabled in Apps Script project settings

### Issue: Files not uploading
**Solution:** Verify Drive folder ID is correct and accessible

### Issue: Application ID not showing
**Solution:** Check that Google Sheet has proper headers in first row

---

## API Endpoints

All endpoints use `POST` to the Apps Script URL with JSON payload:

### Create Application
```json
{
  "action": "submit_application",
  "visaType": "tourist",
  "destination": "us",
  "fullName": "John Doe",
  ...
}
```

### Update Status
```json
{
  "action": "update_status",
  "appId": "VISA-2026-00487",
  "newStatus": "Approved",
  "staffNote": "All documents verified"
}
```

### Get Application
```json
{
  "action": "get_status",
  "appId": "VISA-2026-00487"
}
```

---

## Support & Resources

- **Documentation:** [VisaFlow Pro Docs](https://docs.example.com)
- **Help Center:** [support.example.com](https://support.example.com)
- **Email:** support@example.com
- **WhatsApp:** [Chat with us](https://wa.me/1234567890)

---

**Last Updated:** May 24, 2026  
**Version:** 1.0  
**Status:** Production Ready ✅
