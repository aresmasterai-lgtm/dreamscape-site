# 🤖 Working with Claude on Dreamscape

Quick reference for building Dreamscape with Claude AI when away from home.

---

## 📍 Where You Are Now

✅ **Completed:**
- Static landing page live at trydreamscape.com
- GitHub repo: aresmasterai-lgtm/dreamscape-site
- Netlify deployment configured
- Supabase account created
- Forms working (waitlist + contact)

🎯 **Next Step:** Phase 1 - MVP Foundation (see DREAMSCAPE-BUILD-CHECKLIST.md)

---

## 🗣️ How to Talk to Claude

### ✅ Good Prompts

**Be specific:**
> "Help me write the SQL schema for the artworks table in Supabase. It should store user_id, prompt, style, image_url, and timestamps."

**Show context:**
> "I'm building Dreamscape, an AI art marketplace. I'm in Phase 1.2 (Authentication). I need help creating a login page that integrates with Supabase Auth."

**Share errors:**
> "I'm getting this error when calling the Supabase Edge Function: [paste full error]. Here's my code: [paste code]"

### ❌ Vague Prompts

**Too broad:**
> "Build me the backend"

**No context:**
> "Why isn't my code working?"

**Missing details:**
> "Add authentication"

---

## 🛠️ Common Commands

### Git (Push updates)
```bash
cd dreamscape-site
git add .
git commit -m "Description of changes"
git push
```

### Supabase
```bash
# Login
supabase login

# Create new function
supabase functions new function-name

# Deploy function
supabase functions deploy function-name

# Set secrets
supabase secrets set API_KEY=your_key
```

### Install dependencies
```bash
npm install @supabase/supabase-js
npm install @stripe/stripe-js
```

---

## 📂 Project Structure

```
dreamscape-site/
├── index.html              # Landing page (current)
├── thank-you.html          # Form success page
├── signup.html             # Create this (Phase 1.2)
├── login.html              # Create this (Phase 1.2)
├── dashboard.html          # Create this (Phase 1.5)
├── marketplace.html        # Create this (Phase 2.1)
├── profile.html            # Create this (Phase 1.2)
├── channels.html           # Create this (Phase 2.3)
├── supabase/
│   └── functions/
│       ├── generate-art/   # AI generation (Phase 1.3)
│       └── printful-mockup/# Printful integration (Phase 1.4)
└── DREAMSCAPE-BUILD-CHECKLIST.md  # Your roadmap
```

---

## 🔑 API Keys & Secrets

**You'll need:**

1. **Supabase**
   - Project URL: Found in Supabase dashboard → Settings → API
   - Anon Key: Public key (safe to use in frontend)
   - Service Role Key: Secret (use in Edge Functions only!)

2. **Printful**
   - API Key: Get from https://www.printful.com/dashboard/api
   - Store in Supabase secrets: `supabase secrets set PRINTFUL_API_KEY=your_key`

3. **Gemini API** (for AI art)
   - Get from: https://ai.google.dev/
   - Store in Supabase secrets: `supabase secrets set GEMINI_API_KEY=your_key`

4. **Stripe** (for payments - Phase 2)
   - Publishable Key: Public (frontend)
   - Secret Key: Private (Edge Functions)

**⚠️ NEVER commit API keys to GitHub!**

---

## 🧪 Testing Workflow

### 1. Test Locally
```bash
# Serve site locally
npx serve .

# Test Supabase functions locally
supabase functions serve
```

### 2. Test on Netlify
- Push to GitHub
- Netlify auto-deploys (~1 min)
- Test on staging URL

### 3. Iterate
- Find bugs → fix → push → test again

---

## 🆘 Common Issues & Solutions

### "Supabase function not working"
1. Check logs: `supabase functions logs function-name`
2. Make sure secrets are set: `supabase secrets list`
3. Verify CORS headers in function response

### "Can't authenticate users"
1. Check Supabase URL and keys are correct
2. Enable Email provider in Supabase dashboard
3. Check browser console for errors

### "Printful API returns 401"
1. Verify API key is correct
2. Check if API key has proper permissions
3. Make sure you're using Bearer token auth

### "Images not loading"
1. Check Supabase Storage policies (make public)
2. Verify image URL is correct
3. Check CORS settings

---

## 📋 Phase-by-Phase with Claude

### Starting Phase 1 (MVP)

**Say to Claude:**
> "I'm building Dreamscape (AI art marketplace). I just completed the landing page. I'm starting Phase 1.1 (Backend Setup) from my checklist. Can you help me create the Supabase database schema for artworks and products?"

Then share the schema from DREAMSCAPE-BUILD-CHECKLIST.md

### During Phase 1

**For each task, ask:**
> "I'm on Phase 1.X (describe section). Help me implement [specific task from checklist]."

**Example:**
> "I'm on Phase 1.3 (AI Art Generation). Help me create the Supabase Edge Function that calls the Gemini API to generate images from a user prompt."

### Testing Phase 1 Complete

**Ask Claude:**
> "I've completed all Phase 1 tasks. Can you help me test the complete flow: signup → generate art → create product → view in dashboard?"

---

## 🎯 Working on Specific Features

### Authentication
> "I need help adding Supabase Auth to my site. I want users to sign up with email/password and stay logged in across pages."

### AI Generation
> "Help me create an Edge Function that takes a text prompt, calls the Gemini API, and returns an image URL."

### Printful Integration
> "I need to fetch the Printful product catalog and display it as selectable cards. Then generate mockups using a user's artwork."

### Database Queries
> "Show me how to query Supabase to get all products created by a specific user, sorted by newest first."

---

## 💡 Tips for Success

1. **One feature at a time** - Don't try to build everything at once

2. **Test incrementally** - Make sure each piece works before moving on

3. **Save working code** - Commit to GitHub after each working feature

4. **Ask for explanations** - If you don't understand code, ask Claude to explain

5. **Security mindset** - Always ask "Is this secure?" when handling user data

6. **Mobile-first** - Test on phone as you build

---

## 📞 Support Contacts

**When stuck:**
- Supabase Discord: https://discord.supabase.com/
- Printful Support: help@printful.com
- Netlify Support: https://answers.netlify.com/

**GitHub Repo:**
https://github.com/aresmasterai-lgtm/dreamscape-site

**Live Site:**
https://trydreamscape.com

---

## 🚀 Quick Start Commands

**To continue building:**
```bash
cd dreamscape-site
git pull  # Get latest changes
npm install  # Install any new dependencies
```

**To deploy changes:**
```bash
git add .
git commit -m "What you changed"
git push  # Auto-deploys to Netlify
```

**To create new page:**
1. Copy existing HTML file
2. Modify content
3. Update navigation links
4. Test locally
5. Push to GitHub

---

**You've got this! Build Dreamscape one feature at a time. 🌟**
