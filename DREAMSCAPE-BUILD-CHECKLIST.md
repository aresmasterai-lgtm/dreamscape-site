# 🚀 Dreamscape Build Checklist
**Complete roadmap from landing page to functional platform**

---

## 📊 Current Status (Completed)

✅ Static landing page deployed  
✅ Domain: trydreamscape.com configured  
✅ Netlify hosting active  
✅ Forms working (waitlist + contact)  
✅ GitHub repo: aresmasterai-lgtm/dreamscape-site  
✅ Supabase account created (assumed)  

---

## 🎯 PHASE 1: MVP Foundation (Week 1-2)
**Goal: Basic user accounts + AI art generation + Printful mockup preview**

### 1.1 Backend Setup

- [ ] **Supabase Database Schema**
  ```sql
  -- Users table (Supabase Auth handles this automatically)
  
  -- Artworks table
  CREATE TABLE artworks (
    id UUID PRIMARY KEY DEFAULT uuid_generate_v4(),
    user_id UUID REFERENCES auth.users(id) ON DELETE CASCADE,
    prompt TEXT NOT NULL,
    style VARCHAR(50),
    image_url TEXT,
    created_at TIMESTAMP DEFAULT NOW()
  );
  
  -- Products table
  CREATE TABLE products (
    id UUID PRIMARY KEY DEFAULT uuid_generate_v4(),
    artwork_id UUID REFERENCES artworks(id) ON DELETE CASCADE,
    user_id UUID REFERENCES auth.users(id) ON DELETE CASCADE,
    printful_product_id INT,
    title VARCHAR(255),
    price DECIMAL(10,2),
    status VARCHAR(50) DEFAULT 'draft',
    created_at TIMESTAMP DEFAULT NOW()
  );
  
  -- Add indexes
  CREATE INDEX idx_artworks_user ON artworks(user_id);
  CREATE INDEX idx_products_user ON products(user_id);
  CREATE INDEX idx_products_artwork ON products(artwork_id);
  ```

- [ ] **Set up Supabase Row Level Security (RLS)**
  ```sql
  -- Enable RLS
  ALTER TABLE artworks ENABLE ROW LEVEL SECURITY;
  ALTER TABLE products ENABLE ROW LEVEL SECURITY;
  
  -- Users can only see their own artworks
  CREATE POLICY "Users view own artworks" ON artworks
    FOR SELECT USING (auth.uid() = user_id);
  
  -- Users can insert their own artworks
  CREATE POLICY "Users insert own artworks" ON artworks
    FOR INSERT WITH CHECK (auth.uid() = user_id);
  
  -- Repeat for products table
  CREATE POLICY "Users view own products" ON products
    FOR SELECT USING (auth.uid() = user_id);
  
  CREATE POLICY "Users insert own products" ON products
    FOR INSERT WITH CHECK (auth.uid() = user_id);
  ```

- [ ] **Get API Keys & Store in Supabase Edge Function Secrets**
  - Gemini API key (or Stability AI)
  - Printful API key
  - Store via: `supabase secrets set GEMINI_API_KEY=your_key`

### 1.2 Authentication

- [ ] **Install Supabase Client in your project**
  ```bash
  npm install @supabase/supabase-js
  ```

- [ ] **Create auth pages**
  - [ ] Sign up page (`/signup.html`)
  - [ ] Login page (`/login.html`)
  - [ ] Password reset flow
  - [ ] User profile page (`/profile.html`)

- [ ] **Add Supabase Auth integration**
  ```javascript
  // In your HTML or separate JS file
  import { createClient } from '@supabase/supabase-js'
  
  const supabase = createClient(
    'YOUR_SUPABASE_URL',
    'YOUR_SUPABASE_ANON_KEY'
  )
  
  // Sign up
  async function signUp(email, password) {
    const { data, error } = await supabase.auth.signUp({
      email: email,
      password: password,
    })
  }
  
  // Sign in
  async function signIn(email, password) {
    const { data, error } = await supabase.auth.signInWithPassword({
      email: email,
      password: password,
    })
  }
  ```

### 1.3 AI Art Generation

- [ ] **Create Supabase Edge Function for AI generation**
  ```bash
  supabase functions new generate-art
  ```

- [ ] **Implement Gemini API call in Edge Function**
  ```typescript
  // supabase/functions/generate-art/index.ts
  import { serve } from "https://deno.land/std@0.168.0/http/server.ts"
  
  serve(async (req) => {
    const { prompt, style } = await req.json()
    
    // Call Gemini API
    const response = await fetch('https://generativelanguage.googleapis.com/v1beta/models/gemini-pro-vision:generateContent', {
      method: 'POST',
      headers: {
        'Content-Type': 'application/json',
        'Authorization': `Bearer ${Deno.env.get('GEMINI_API_KEY')}`
      },
      body: JSON.stringify({
        prompt: `${style}: ${prompt}`,
        // ... other parameters
      })
    })
    
    const data = await response.json()
    
    // Store in Supabase Storage
    // Return image URL
    
    return new Response(JSON.stringify({ imageUrl: data.url }))
  })
  ```

- [ ] **Deploy Edge Function**
  ```bash
  supabase functions deploy generate-art
  ```

- [ ] **Create frontend UI for art generation**
  - [ ] Style selector
  - [ ] Prompt textarea
  - [ ] Generate button
  - [ ] Loading state
  - [ ] Image preview

### 1.4 Printful Integration

- [ ] **Get Printful API key** from https://www.printful.com/dashboard/api

- [ ] **Create Edge Function for Printful**
  ```bash
  supabase functions new printful-mockup
  ```

- [ ] **Implement Printful product catalog fetch**
  ```typescript
  // Fetch available products
  const response = await fetch('https://api.printful.com/products', {
    headers: {
      'Authorization': `Bearer ${Deno.env.get('PRINTFUL_API_KEY')}`
    }
  })
  ```

- [ ] **Create mockup generation endpoint**
  ```typescript
  // Generate mockup with user's artwork
  const mockup = await fetch('https://api.printful.com/mockup-generator/create-task/123', {
    method: 'POST',
    headers: {
      'Authorization': `Bearer ${Deno.env.get('PRINTFUL_API_KEY')}`,
      'Content-Type': 'application/json'
    },
    body: JSON.stringify({
      variant_ids: [4011],
      format: 'jpg',
      files: [{
        placement: 'default',
        image_url: userArtworkUrl
      }]
    })
  })
  ```

- [ ] **Create product selection UI**
  - [ ] Show Printful product catalog
  - [ ] Multi-select products
  - [ ] Preview mockups with user's art

### 1.5 Artist Dashboard

- [ ] **Create dashboard page** (`/dashboard.html`)
  - [ ] List user's generated artworks
  - [ ] List user's products
  - [ ] Stats (views, sales)
  - [ ] Create new product button

- [ ] **Implement artwork gallery**
  - [ ] Grid layout
  - [ ] Click to edit/delete
  - [ ] Use artwork for product creation

### 1.6 Testing

- [ ] **Test complete flow:**
  1. Sign up new account
  2. Generate AI art
  3. Create product with art
  4. Preview mockup
  5. View in dashboard

---

## 🎨 PHASE 2: Core Features (Week 3-4)
**Goal: Public marketplace + artist profiles + channels**

### 2.1 Public Marketplace

- [ ] **Create marketplace page** (`/marketplace.html`)
  - [ ] Filter by category
  - [ ] Search functionality
  - [ ] Sort by: newest, price, popular
  - [ ] Pagination

- [ ] **Product detail page** (`/product/[id].html`)
  - [ ] Product images/mockups
  - [ ] Price & variants
  - [ ] Artist info
  - [ ] Add to cart button

- [ ] **Update database schema**
  ```sql
  -- Make certain products public
  ALTER TABLE products ADD COLUMN is_public BOOLEAN DEFAULT FALSE;
  
  -- Public read policy
  CREATE POLICY "Anyone can view public products" ON products
    FOR SELECT USING (is_public = true);
  ```

### 2.2 Artist Profiles

- [ ] **Create artist profile page** (`/artist/[username].html`)
  - [ ] Bio & profile picture
  - [ ] Portfolio of artworks
  - [ ] Products for sale
  - [ ] Follow button
  - [ ] Stats (followers, sales)

- [ ] **Profiles database schema**
  ```sql
  CREATE TABLE profiles (
    id UUID PRIMARY KEY REFERENCES auth.users(id) ON DELETE CASCADE,
    username VARCHAR(50) UNIQUE,
    display_name VARCHAR(100),
    bio TEXT,
    avatar_url TEXT,
    style_tags TEXT[],
    created_at TIMESTAMP DEFAULT NOW()
  );
  
  CREATE TABLE followers (
    follower_id UUID REFERENCES auth.users(id) ON DELETE CASCADE,
    following_id UUID REFERENCES auth.users(id) ON DELETE CASCADE,
    created_at TIMESTAMP DEFAULT NOW(),
    PRIMARY KEY (follower_id, following_id)
  );
  ```

- [ ] **Profile edit page**
  - [ ] Upload avatar
  - [ ] Edit bio
  - [ ] Set username
  - [ ] Choose art styles

### 2.3 Community Channels

- [ ] **Channels database schema**
  ```sql
  CREATE TABLE channels (
    id UUID PRIMARY KEY DEFAULT uuid_generate_v4(),
    name VARCHAR(100) UNIQUE NOT NULL,
    description TEXT,
    category VARCHAR(50),
    member_count INT DEFAULT 0,
    created_at TIMESTAMP DEFAULT NOW()
  );
  
  CREATE TABLE channel_posts (
    id UUID PRIMARY KEY DEFAULT uuid_generate_v4(),
    channel_id UUID REFERENCES channels(id) ON DELETE CASCADE,
    user_id UUID REFERENCES auth.users(id) ON DELETE CASCADE,
    content TEXT NOT NULL,
    image_url TEXT,
    likes_count INT DEFAULT 0,
    created_at TIMESTAMP DEFAULT NOW()
  );
  
  CREATE TABLE channel_members (
    channel_id UUID REFERENCES channels(id) ON DELETE CASCADE,
    user_id UUID REFERENCES auth.users(id) ON DELETE CASCADE,
    joined_at TIMESTAMP DEFAULT NOW(),
    PRIMARY KEY (channel_id, user_id)
  );
  ```

- [ ] **Create channels page** (`/channels.html`)
  - [ ] List all channels by category
  - [ ] Join/leave channel button
  - [ ] Live member count

- [ ] **Channel detail page** (`/channel/[name].html`)
  - [ ] Post feed
  - [ ] Create post button
  - [ ] Like/comment functionality
  - [ ] Online members sidebar

### 2.4 Shopping Cart & Checkout

- [ ] **Cart database schema**
  ```sql
  CREATE TABLE cart_items (
    id UUID PRIMARY KEY DEFAULT uuid_generate_v4(),
    user_id UUID REFERENCES auth.users(id) ON DELETE CASCADE,
    product_id UUID REFERENCES products(id) ON DELETE CASCADE,
    quantity INT DEFAULT 1,
    variant TEXT,
    added_at TIMESTAMP DEFAULT NOW()
  );
  ```

- [ ] **Create cart UI**
  - [ ] Add to cart functionality
  - [ ] View cart page
  - [ ] Update quantities
  - [ ] Remove items

- [ ] **Integrate Stripe for payments**
  ```bash
  npm install @stripe/stripe-js
  ```

- [ ] **Create checkout flow**
  - [ ] Shipping address form
  - [ ] Payment method (Stripe)
  - [ ] Order review
  - [ ] Submit order

- [ ] **Create Printful order on successful payment**
  ```typescript
  // In Edge Function
  const order = await fetch('https://api.printful.com/orders', {
    method: 'POST',
    headers: {
      'Authorization': `Bearer ${Deno.env.get('PRINTFUL_API_KEY')}`,
      'Content-Type': 'application/json'
    },
    body: JSON.stringify({
      recipient: {...shippingAddress},
      items: cartItems,
      retail_costs: {
        currency: 'USD',
        total: totalAmount
      }
    })
  })
  ```

### 2.5 Orders & Fulfillment

- [ ] **Orders database schema**
  ```sql
  CREATE TABLE orders (
    id UUID PRIMARY KEY DEFAULT uuid_generate_v4(),
    user_id UUID REFERENCES auth.users(id) ON DELETE CASCADE,
    printful_order_id VARCHAR(255),
    total_amount DECIMAL(10,2),
    status VARCHAR(50),
    tracking_number VARCHAR(255),
    created_at TIMESTAMP DEFAULT NOW()
  );
  ```

- [ ] **Order history page** (`/orders.html`)
  - [ ] List past orders
  - [ ] Track shipment
  - [ ] Order details

- [ ] **Artist sales dashboard**
  - [ ] Revenue tracking
  - [ ] Order notifications
  - [ ] Payout information

---

## 🚀 PHASE 3: Growth & Scale (Week 5-8)
**Goal: Advanced features + monetization + analytics**

### 3.1 Search & Discovery

- [ ] **Implement Algolia or Meilisearch**
  - [ ] Index products
  - [ ] Index artists
  - [ ] Index channels
  - [ ] Search bar with autocomplete

- [ ] **Recommendation engine**
  - [ ] Similar products
  - [ ] Similar artists
  - [ ] Personalized feed

### 3.2 Social Features

- [ ] **Comments & Replies**
  ```sql
  CREATE TABLE comments (
    id UUID PRIMARY KEY DEFAULT uuid_generate_v4(),
    post_id UUID REFERENCES channel_posts(id) ON DELETE CASCADE,
    user_id UUID REFERENCES auth.users(id) ON DELETE CASCADE,
    content TEXT NOT NULL,
    created_at TIMESTAMP DEFAULT NOW()
  );
  ```

- [ ] **Notifications system**
  ```sql
  CREATE TABLE notifications (
    id UUID PRIMARY KEY DEFAULT uuid_generate_v4(),
    user_id UUID REFERENCES auth.users(id) ON DELETE CASCADE,
    type VARCHAR(50),
    content TEXT,
    read BOOLEAN DEFAULT FALSE,
    created_at TIMESTAMP DEFAULT NOW()
  );
  ```

- [ ] **Direct messaging**
  - [ ] Artist-to-artist DMs
  - [ ] Customer-to-artist inquiries

### 3.3 Advanced AI Features

- [ ] **Style transfer**
  - [ ] Upload reference image
  - [ ] Apply style to new artwork

- [ ] **Batch generation**
  - [ ] Generate multiple variations
  - [ ] Compare outputs

- [ ] **AI prompt library**
  - [ ] Save successful prompts
  - [ ] Share prompts with community
  - [ ] Remix others' prompts

### 3.4 Analytics & Insights

- [ ] **Google Analytics 4 integration**
  - [ ] Track page views
  - [ ] Track conversions
  - [ ] User flow analysis

- [ ] **Artist analytics dashboard**
  - [ ] Views per product
  - [ ] Conversion rates
  - [ ] Revenue charts
  - [ ] Traffic sources

### 3.5 Marketing & Growth

- [ ] **Email campaigns** (Mailchimp or SendGrid)
  - [ ] Welcome email
  - [ ] New product alerts
  - [ ] Weekly digest

- [ ] **Referral program**
  ```sql
  CREATE TABLE referrals (
    id UUID PRIMARY KEY DEFAULT uuid_generate_v4(),
    referrer_id UUID REFERENCES auth.users(id),
    referred_id UUID REFERENCES auth.users(id),
    reward_amount DECIMAL(10,2),
    created_at TIMESTAMP DEFAULT NOW()
  );
  ```

- [ ] **Affiliate system**
  - [ ] Generate affiliate links
  - [ ] Track commissions

### 3.6 Mobile App (Optional)

- [ ] **React Native app**
  - [ ] Reuse Supabase backend
  - [ ] Push notifications
  - [ ] Camera integration for art generation

---

## 🔧 Technical Debt & Optimization

### Performance

- [ ] **Implement CDN** for images (Cloudflare or Imgix)
- [ ] **Lazy loading** for product grids
- [ ] **Image optimization** (WebP format)
- [ ] **Database query optimization** (add indexes)
- [ ] **Caching** (Redis or Supabase Edge Cache)

### Security

- [ ] **Rate limiting** on API endpoints
- [ ] **Input validation** (prevent SQL injection)
- [ ] **Content moderation** (flagging inappropriate content)
- [ ] **CORS policies** configured correctly

### Monitoring

- [ ] **Sentry** for error tracking
- [ ] **Uptime monitoring** (UptimeRobot)
- [ ] **Performance monitoring** (Lighthouse CI)
- [ ] **Database backups** automated

---

## 📋 Deployment Checklist

### Before Launch

- [ ] All forms working
- [ ] User flows tested end-to-end
- [ ] Mobile responsive on all pages
- [ ] SSL certificate active (https)
- [ ] Privacy policy page
- [ ] Terms of service page
- [ ] Contact page

### Launch Day

- [ ] DNS propagated fully
- [ ] Email notifications working
- [ ] Payment gateway in production mode
- [ ] Printful API in production
- [ ] Social media accounts created
- [ ] Press release ready
- [ ] Discord/community launched

### Post-Launch

- [ ] Monitor errors (Sentry)
- [ ] Track analytics daily
- [ ] Respond to user feedback
- [ ] Bug fixes prioritized
- [ ] Content marketing started

---

## 🆘 Resources & Links

### APIs & Services

- **Supabase:** https://supabase.com/docs
- **Printful API:** https://developers.printful.com/docs/
- **Gemini API:** https://ai.google.dev/docs
- **Stripe Docs:** https://stripe.com/docs

### Tutorials

- **Supabase Auth:** https://supabase.com/docs/guides/auth
- **Edge Functions:** https://supabase.com/docs/guides/functions
- **Printful Integration Guide:** https://developers.printful.com/docs/#section/Integration-guides

### Community

- **Supabase Discord:** https://discord.supabase.com/
- **Printful Support:** https://www.printful.com/help
- **Dreamscape Discord:** (Your own community link)

---

## 💰 Estimated Costs

### Phase 1 (MVP)
- Supabase: Free tier
- Netlify: Free tier
- Gemini API: ~$5-20/month (depends on usage)
- Printful: $0 (pay per order)
- **Total: ~$5-20/month**

### Phase 2 (Core Features)
- Supabase Pro: $25/month (if needed)
- Netlify Pro: $19/month (optional)
- Stripe: 2.9% + $0.30 per transaction
- **Total: ~$50-100/month + transaction fees**

### Phase 3 (Growth)
- Algolia: $1/month (free tier likely sufficient)
- Email service: $10-50/month
- Analytics tools: $0-50/month
- **Total: ~$100-200/month**

---

## 🎯 Success Metrics

### Phase 1 Goals
- [ ] 100 waitlist signups converted to users
- [ ] 50 AI artworks generated
- [ ] 10 products created

### Phase 2 Goals
- [ ] 500 registered users
- [ ] 200 products in marketplace
- [ ] First 10 sales
- [ ] 5 active channels

### Phase 3 Goals
- [ ] 2,000+ users
- [ ] $5,000+ monthly GMV (Gross Merchandise Value)
- [ ] 50+ active channels
- [ ] 100+ daily active users

---

## 📝 Notes for Working with Claude

When working with Claude AI on this project:

1. **Share this checklist** - Give Claude the full context of what phase you're in

2. **Be specific** - Instead of "build the database", say "help me write the Supabase SQL for the artworks table in Phase 1.1"

3. **Show errors** - Copy/paste exact error messages when something breaks

4. **Test incrementally** - Don't build everything at once, test each feature

5. **Ask for explanations** - If code doesn't make sense, ask Claude to explain it

6. **Security first** - Always ask "is this secure?" when handling user data or API keys

---

**Good luck building Dreamscape! 🚀✨**

*Last updated: March 3, 2026*
