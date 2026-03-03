# 🚀 Deploy Dreamscape to Netlify - Step by Step

## ✅ What's Ready
- Domain: trydreamscape.com
- Forms will email: kingsley@lucrativemerchants.com
- Google Analytics: Will add later

---

## Step 1: Upload to GitHub (5 minutes)

Open your terminal/command prompt and run these commands **one at a time**:

```bash
cd dreamscape-site
```

```bash
git init
```

```bash
git add .
```

```bash
git commit -m "Initial Dreamscape deployment"
```

Now go to **github.com** and:

1. Click the **"+"** icon (top right) → **"New repository"**
2. Repository name: `dreamscape-site`
3. Make it **Public**
4. **DO NOT** check "Initialize with README" (you already have one)
5. Click **"Create repository"**

GitHub will show you commands. **Ignore them** and run these instead (replace `YOUR-USERNAME` with your actual GitHub username):

```bash
git remote add origin https://github.com/YOUR-USERNAME/dreamscape-site.git
```

```bash
git branch -M main
```

```bash
git push -u origin main
```

**✅ Done!** Your code is on GitHub. Refresh the repository page to see it.

---

## Step 2: Deploy on Netlify (3 minutes)

1. Go to **app.netlify.com** (you should be logged in)

2. Click **"Add new site"** button (big green button)

3. Choose **"Import an existing project"**

4. Click **"Deploy with GitHub"**

5. Click **"Authorize Netlify"** (if prompted)

6. Find and click **"dreamscape-site"** in the list

7. Leave everything as default:
   - Branch: `main`
   - Build command: (leave empty)
   - Publish directory: `.` or leave empty

8. Click **"Deploy dreamscape-site"**

**⏱ Wait 30-60 seconds...**

You'll see: **"Site is live"** with a URL like `random-name-12345.netlify.app`

**🎉 Your site is LIVE!** Click the URL to see it.

---

## Step 3: Connect Your Domain (10 minutes)

### In Netlify:

1. Click **"Domain settings"** (or go to Site configuration → Domain management)

2. Click **"Add domain"**

3. Type: `trydreamscape.com`

4. Click **"Verify"**

5. Netlify will show DNS records. **Keep this page open!**

### In Your Domain Registrar (GoDaddy/Namecheap/etc):

1. Log into your domain provider

2. Find **DNS Settings** or **DNS Management**

3. **Delete** any existing A records for `@`

4. **Add these DNS records:**

   **Record 1:**
   ```
   Type: A
   Host: @ (or leave blank)
   Value: 75.2.60.5
   TTL: Automatic (or 3600)
   ```

   **Record 2:**
   ```
   Type: CNAME
   Host: www
   Value: [your-netlify-site].netlify.app
   TTL: Automatic (or 3600)
   ```

5. **Save changes**

6. Go back to Netlify and click **"Verify DNS configuration"**

**⏱ DNS takes 5 minutes - 2 hours (usually ~20 min)**

While waiting, your Netlify URL still works!

---

## Step 4: Set Up Form Notifications (2 minutes)

### In Netlify:

1. Go to **"Forms"** in the left sidebar

2. You'll see two forms: `waitlist` and `contact`

3. Click on **"waitlist"**

4. Click **"Settings & notifications"**

5. Scroll to **"Email notifications"**

6. Click **"Add notification"**

7. Select **"Email notification"**

8. Add email: `kingsley@lucrativemerchants.com`

9. Click **"Save"**

10. **Repeat for the "contact" form**

**✅ Now you'll get emails when someone submits a form!**

---

## Step 5: Test Everything (5 minutes)

1. Visit **trydreamscape.com** (or Netlify URL if DNS isn't ready)

2. **Test Waitlist Form:**
   - Enter your email
   - Click "Join Now"
   - Should see success message
   - Check your email (kingsley@lucrativemerchants.com)

3. **Test Contact Form:**
   - Fill out name, email, message
   - Click "Send Message"
   - Should see success message
   - Check your email again

4. **Test on Mobile:**
   - Open site on your phone
   - Make sure it looks good

---

## 🎉 You're Live!

Your site is now at: **https://trydreamscape.com**

### What Happens Now:

✅ **Any GitHub push auto-deploys** - Just commit & push, Netlify rebuilds automatically  
✅ **Forms go to your email** - You'll get notified of every submission  
✅ **HTTPS is automatic** - Secure by default  
✅ **Global CDN** - Fast everywhere in the world  

---

## 📊 Add Google Analytics Later

When you're ready:

1. Create GA4 property at analytics.google.com
2. Get Measurement ID (G-XXXXXXXXXX)
3. Add this code **before `</head>`** in index.html:

```html
<script async src="https://www.googletagmanager.com/gtag/js?id=G-XXXXXXXXXX"></script>
<script>
  window.dataLayer = window.dataLayer || [];
  function gtag(){dataLayer.push(arguments);}
  gtag('js', new Date());
  gtag('config', 'G-XXXXXXXXXX');
</script>
```

4. Push to GitHub → Auto-deploys

---

## 🆘 Troubleshooting

**"Command not found: git"**
- Download Git: https://git-scm.com/downloads
- Restart terminal after install

**"Remote origin already exists"**
```bash
git remote remove origin
git remote add origin https://github.com/YOUR-USERNAME/dreamscape-site.git
```

**DNS not working after 2 hours**
- Check DNS propagation: https://www.whatsmydns.net
- Make sure A record points to: 75.2.60.5
- Contact your domain registrar support

**Forms not sending emails**
- Check Netlify Forms dashboard
- Make sure email notifications are set up
- Check spam folder

---

## 🔥 Ready? Let's Go!

Start with **Step 1** and work through each step. Take your time!

Questions? Drop them in Discord and tag me.

**Let's get Dreamscape live! 🚀**
