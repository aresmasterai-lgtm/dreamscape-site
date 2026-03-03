# Dreamscape - AI Art Social Platform

Welcome to Dreamscape! A platform where artists create stunning AI artwork and sell merchandise globally through Printful.

## 🚀 Features

- **AI Art Generation** - Create artwork with Dream AI assistant
- **Global Marketplace** - 300+ Printful products integrated
- **Artist Profiles** - Build your creative portfolio
- **Community Channels** - Connect with artists worldwide
- **Waitlist & Contact Forms** - Built-in with Netlify Forms

## 🛠 Tech Stack

- Pure HTML/CSS/JavaScript (no build tools required)
- Netlify Forms for waitlist & contact
- Google Analytics ready
- Mobile responsive design
- SEO optimized

## 📝 Setup Instructions

### Deploy to Netlify

1. Push this repo to GitHub
2. Connect your GitHub account to Netlify
3. Select this repository
4. Deploy! (Netlify auto-detects settings)

### Configure Google Analytics

1. Get your GA4 Measurement ID from [Google Analytics](https://analytics.google.com)
2. Replace `GA_MEASUREMENT_ID` in `index.html` (line 16) with your actual ID
3. Example: `G-XXXXXXXXXX`

### Enable Netlify Forms

Forms are already configured! Just make sure:
- Form has `data-netlify="true"` attribute ✅
- Form has `name` attribute ✅

Netlify will automatically handle submissions after deployment.

### Custom Domain

1. Go to Netlify → Site Settings → Domain Management
2. Add your custom domain
3. Update DNS records at your registrar

## 📧 Form Notifications

By default, form submissions go to your Netlify account email. To change:

1. Go to Netlify → Forms
2. Click on form name
3. Settings → Form notifications
4. Add email or webhook

## 🎨 Customization

All styling is in the `<style>` section of `index.html`. Color scheme defined in `:root` CSS variables:

```css
--accent: #7C5CFC;   /* Primary brand color */
--teal: #00D4AA;     /* Secondary accent */
--gold: #F5C842;     /* Highlights */
```

## 📊 Analytics Events

The site tracks these events:
- Waitlist signups
- Contact form submissions
- Button clicks (can be expanded)

## 🔒 Security

- HTTPS enabled by default (Netlify)
- Form spam protection (Netlify built-in)
- No sensitive data stored client-side

## 📱 Browser Support

- Chrome/Edge (latest)
- Firefox (latest)
- Safari (latest)
- Mobile browsers (iOS/Android)

## 📄 License

All rights reserved © 2024 Dreamscape

---

Built with ✦ for artists, by artists.
