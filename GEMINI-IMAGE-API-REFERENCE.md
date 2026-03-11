# 🎨 Gemini 3 Pro Image API Reference (Nano Banana Pro)
**Complete API documentation for image generation in Dreamscape**

---

## 📍 Quick Reference

**Model:** `gemini-3-pro-image-preview`  
**Base URL:** `https://generativelanguage.googleapis.com/v1beta/models/gemini-3-pro-image-preview:generateContent`  
**Method:** `POST`  
**Auth:** Bearer token in header  
**Docs:** https://ai.google.dev/docs  

---

## 🔑 Get API Key

1. Visit: https://ai.google.dev/
2. Click **"Get API key in Google AI Studio"**
3. Create new API key
4. Store securely (never commit to GitHub!)

**Set as environment variable:**
```bash
export GEMINI_API_KEY="your_api_key_here"
```

**In Supabase Edge Functions:**
```bash
supabase secrets set GEMINI_API_KEY=your_api_key_here
```

---

## 📝 Request Format

### Headers
```json
{
  "Content-Type": "application/json",
  "Authorization": "Bearer YOUR_GEMINI_API_KEY"
}
```

### Simple Image Generation
```json
{
  "contents": [{
    "parts": [{
      "text": "A majestic sunset over snow-capped mountains, vibrant orange and purple sky, photorealistic"
    }]
  }],
  "generationConfig": {
    "responseModalities": ["TEXT", "IMAGE"],
    "imageConfig": {
      "imageSize": "1K"
    }
  }
}
```

### Image Editing (with input images)
```json
{
  "contents": [{
    "parts": [
      {
        "inlineData": {
          "mimeType": "image/png",
          "data": "base64_encoded_image_data"
        }
      },
      {
        "text": "Add a dramatic sunset in the background"
      }
    ]
  }],
  "generationConfig": {
    "responseModalities": ["TEXT", "IMAGE"],
    "imageConfig": {
      "imageSize": "2K"
    }
  }
}
```

### Multi-Image Composition (up to 14 images)
```json
{
  "contents": [{
    "parts": [
      {
        "inlineData": {
          "mimeType": "image/png",
          "data": "base64_image_1"
        }
      },
      {
        "inlineData": {
          "mimeType": "image/png",
          "data": "base64_image_2"
        }
      },
      {
        "text": "Combine these images into a single artistic composition"
      }
    ]
  }],
  "generationConfig": {
    "responseModalities": ["TEXT", "IMAGE"],
    "imageConfig": {
      "imageSize": "4K"
    }
  }
}
```

---

## 📦 Response Format

### Success Response
```json
{
  "candidates": [
    {
      "content": {
        "parts": [
          {
            "text": "I've generated a sunset over mountains with vibrant colors."
          },
          {
            "inlineData": {
              "mimeType": "image/png",
              "data": "iVBORw0KGgoAAAANSUhEUgAA... (base64 encoded image)"
            }
          }
        ],
        "role": "model"
      },
      "finishReason": "STOP",
      "safetyRatings": [
        {
          "category": "HARM_CATEGORY_HATE_SPEECH",
          "probability": "NEGLIGIBLE"
        },
        {
          "category": "HARM_CATEGORY_DANGEROUS_CONTENT",
          "probability": "NEGLIGIBLE"
        },
        {
          "category": "HARM_CATEGORY_SEXUALLY_EXPLICIT",
          "probability": "NEGLIGIBLE"
        },
        {
          "category": "HARM_CATEGORY_HARASSMENT",
          "probability": "NEGLIGIBLE"
        }
      ]
    }
  ],
  "usageMetadata": {
    "promptTokenCount": 15,
    "candidatesTokenCount": 1200,
    "totalTokenCount": 1215
  }
}
```

### Error Response
```json
{
  "error": {
    "code": 400,
    "message": "API key not valid. Please pass a valid API key.",
    "status": "INVALID_ARGUMENT",
    "details": [
      {
        "@type": "type.googleapis.com/google.rpc.ErrorInfo",
        "reason": "API_KEY_INVALID",
        "domain": "googleapis.com",
        "metadata": {
          "service": "generativelanguage.googleapis.com"
        }
      }
    ]
  }
}
```

---

## 🔧 Sample Requests

### cURL (Command Line)

**Simple generation:**
```bash
curl -X POST \
  'https://generativelanguage.googleapis.com/v1beta/models/gemini-3-pro-image-preview:generateContent' \
  -H 'Content-Type: application/json' \
  -H 'Authorization: Bearer YOUR_GEMINI_API_KEY' \
  -d '{
    "contents": [{
      "parts": [{
        "text": "A futuristic cyberpunk city at night, neon lights, rain, cinematic"
      }]
    }],
    "generationConfig": {
      "responseModalities": ["TEXT", "IMAGE"],
      "imageConfig": {
        "imageSize": "1K"
      }
    }
  }'
```

### JavaScript (Node.js / Deno)

**Using fetch:**
```javascript
async function generateImage(prompt, resolution = "1K") {
  const response = await fetch(
    'https://generativelanguage.googleapis.com/v1beta/models/gemini-3-pro-image-preview:generateContent',
    {
      method: 'POST',
      headers: {
        'Content-Type': 'application/json',
        'Authorization': `Bearer ${process.env.GEMINI_API_KEY}`
      },
      body: JSON.stringify({
        contents: [{
          parts: [{
            text: prompt
          }]
        }],
        generationConfig: {
          responseModalities: ["TEXT", "IMAGE"],
          imageConfig: {
            imageSize: resolution
          }
        }
      })
    }
  )
  
  const data = await response.json()
  
  // Extract image data
  const imagePart = data.candidates[0].content.parts.find(
    part => part.inlineData
  )
  
  if (imagePart) {
    const imageBase64 = imagePart.inlineData.data
    // Convert base64 to buffer
    const imageBuffer = Buffer.from(imageBase64, 'base64')
    return imageBuffer
  }
  
  throw new Error('No image generated')
}

// Usage
const imageBuffer = await generateImage(
  "A serene Japanese garden with cherry blossoms",
  "2K"
)
```

### Python (using Google SDK)

```python
from google import genai
from google.genai import types

# Initialize client
client = genai.Client(api_key="YOUR_GEMINI_API_KEY")

# Generate image
response = client.models.generate_content(
    model="gemini-3-pro-image-preview",
    contents="A bioluminescent coral reef city at twilight, ethereal glow",
    config=types.GenerateContentConfig(
        response_modalities=["TEXT", "IMAGE"],
        image_config=types.ImageConfig(
            image_size="1K"
        )
    )
)

# Extract and save image
for part in response.parts:
    if part.text:
        print(f"Model: {part.text}")
    elif part.inline_data:
        image_bytes = part.inline_data.data
        with open("output.png", "wb") as f:
            f.write(image_bytes)
        print("Image saved as output.png")
```

### TypeScript (Supabase Edge Function)

```typescript
// supabase/functions/generate-art/index.ts
import { serve } from "https://deno.land/std@0.168.0/http/server.ts"

serve(async (req) => {
  try {
    const { prompt, style, resolution = "1K" } = await req.json()
    
    // Build full prompt with style
    const fullPrompt = style ? `${style}: ${prompt}` : prompt
    
    // Call Gemini API
    const response = await fetch(
      'https://generativelanguage.googleapis.com/v1beta/models/gemini-3-pro-image-preview:generateContent',
      {
        method: 'POST',
        headers: {
          'Content-Type': 'application/json',
          'Authorization': `Bearer ${Deno.env.get('GEMINI_API_KEY')}`
        },
        body: JSON.stringify({
          contents: [{
            parts: [{
              text: fullPrompt
            }]
          }],
          generationConfig: {
            responseModalities: ["TEXT", "IMAGE"],
            imageConfig: {
              imageSize: resolution
            }
          }
        })
      }
    )
    
    const data = await response.json()
    
    // Check for errors
    if (data.error) {
      throw new Error(data.error.message)
    }
    
    // Extract image
    const imagePart = data.candidates[0].content.parts.find(
      (part: any) => part.inlineData
    )
    
    if (!imagePart) {
      throw new Error('No image generated')
    }
    
    const imageBase64 = imagePart.inlineData.data
    
    // TODO: Upload to Supabase Storage
    // const { data: uploadData, error: uploadError } = await supabase
    //   .storage
    //   .from('artworks')
    //   .upload(`${userId}/${timestamp}.png`, imageBuffer)
    
    return new Response(
      JSON.stringify({ 
        success: true,
        imageData: imageBase64,
        // imageUrl: uploadData.path
      }),
      { 
        headers: { 'Content-Type': 'application/json' },
        status: 200 
      }
    )
    
  } catch (error) {
    return new Response(
      JSON.stringify({ 
        success: false,
        error: error.message 
      }),
      { 
        headers: { 'Content-Type': 'application/json' },
        status: 500 
      }
    )
  }
})
```

---

## 🎛️ Configuration Options

### Image Sizes (imageSize)
- `"1K"` - ~1024px, fastest, ~$0.04/image (default)
- `"2K"` - ~2048px, balanced, ~$0.08/image
- `"4K"` - ~4096px, highest quality, ~$0.15/image

### Response Modalities
- `["TEXT"]` - Text only
- `["IMAGE"]` - Image only
- `["TEXT", "IMAGE"]` - Both (recommended)

### Safety Settings (optional)
```json
{
  "safetySettings": [
    {
      "category": "HARM_CATEGORY_HATE_SPEECH",
      "threshold": "BLOCK_MEDIUM_AND_ABOVE"
    },
    {
      "category": "HARM_CATEGORY_DANGEROUS_CONTENT",
      "threshold": "BLOCK_MEDIUM_AND_ABOVE"
    }
  ]
}
```

Thresholds:
- `BLOCK_NONE`
- `BLOCK_LOW_AND_ABOVE`
- `BLOCK_MEDIUM_AND_ABOVE` (default)
- `BLOCK_ONLY_HIGH`

---

## 💰 Pricing

**Per image (as of 2024):**
- 1K: ~$0.04
- 2K: ~$0.08
- 4K: ~$0.15

**Multi-image input:** Same base price + additional cost per input image

**Check latest:** https://ai.google.dev/pricing

**Estimate for Dreamscape:**
- 1000 images/month at 1K = ~$40/month
- 1000 images/month at 2K = ~$80/month

---

## ⚠️ Rate Limits

**Free tier:**
- 60 requests per minute
- 1500 requests per day

**Paid tier:**
- Higher limits (check your quota in AI Studio)

**Handle rate limits:**
```javascript
async function generateWithRetry(prompt, maxRetries = 3) {
  for (let i = 0; i < maxRetries; i++) {
    try {
      return await generateImage(prompt)
    } catch (error) {
      if (error.message.includes('429') && i < maxRetries - 1) {
        // Wait with exponential backoff
        await new Promise(resolve => setTimeout(resolve, 1000 * Math.pow(2, i)))
        continue
      }
      throw error
    }
  }
}
```

---

## 🎨 Prompt Engineering Tips

### Good Prompts
✅ "A serene Japanese garden at sunset, cherry blossoms, koi pond, soft lighting, photorealistic, 4K"  
✅ "Cyberpunk street scene, neon signs, rain-slicked pavement, night, cinematic composition"  
✅ "Abstract geometric pattern, vibrant colors, minimalist, vector art style"  

### Poor Prompts
❌ "Make it cool" (too vague)  
❌ "A picture" (no details)  
❌ "Something nice" (unclear)  

### Style Keywords
- **Photorealistic:** "photorealistic", "hyperrealistic", "4K photography"
- **Artistic:** "oil painting", "watercolor", "impressionist"
- **Digital:** "digital art", "3D render", "vector illustration"
- **Cinematic:** "cinematic lighting", "film grain", "dramatic shadows"

### Technical Terms
- **Lighting:** "golden hour", "rim lighting", "volumetric lighting"
- **Composition:** "rule of thirds", "wide angle", "close-up"
- **Quality:** "highly detailed", "sharp focus", "8K"

---

## 🔒 Security Best Practices

### ✅ DO
- Store API key in environment variables
- Use Supabase secrets for Edge Functions
- Validate user input before generating
- Implement rate limiting per user
- Log failed attempts

### ❌ DON'T
- Hardcode API keys in code
- Expose API key in frontend
- Allow unlimited generations per user
- Skip content moderation
- Store unencrypted API keys in database

---

## 🐛 Common Errors

### Error: API key not valid
**Cause:** Invalid or missing API key  
**Fix:** Check `GEMINI_API_KEY` is set correctly

### Error: 429 Too Many Requests
**Cause:** Rate limit exceeded  
**Fix:** Implement exponential backoff, upgrade quota

### Error: 400 Bad Request
**Cause:** Invalid request format  
**Fix:** Check JSON structure matches documentation

### Error: Content filtered
**Cause:** Prompt triggered safety filters  
**Fix:** Adjust prompt, check safety ratings in response

### Error: No image generated
**Cause:** Model returned text only  
**Fix:** Ensure `responseModalities` includes "IMAGE"

---

## 📚 Additional Resources

**Official Docs:**
- API Reference: https://ai.google.dev/api/rest
- Gemini Cookbook: https://github.com/google-gemini/cookbook
- Pricing: https://ai.google.dev/pricing

**Community:**
- Google AI Discord: https://discord.gg/googleai
- Stack Overflow: Tag [google-gemini]

**Alternatives:**
- Stability AI (Stable Diffusion): https://stability.ai/
- Midjourney API: https://docs.midjourney.com/
- DALL-E 3: https://platform.openai.com/docs/guides/images

---

## 🚀 Integration Checklist for Dreamscape

- [ ] Get Gemini API key from Google AI Studio
- [ ] Store API key in Supabase secrets
- [ ] Create Edge Function `generate-art`
- [ ] Implement image generation endpoint
- [ ] Add error handling & retries
- [ ] Set up Supabase Storage for images
- [ ] Create frontend UI for generation
- [ ] Add loading states
- [ ] Implement user rate limiting
- [ ] Test with various prompts
- [ ] Monitor costs & usage
- [ ] Add content moderation

---

**Last updated:** March 3, 2026  
**For Dreamscape MVP - Phase 1.3**
