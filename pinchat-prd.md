# PinChat - Product Requirements Document

## Vision
AI-powered prompt generator that converts Pinterest references into production-ready image transformation prompts for tools like Higgsfield.

## Problem
Designers manually craft prompts for image-to-image tools, resulting in inconsistent quality and time waste. Current prompts lack professional detail for Pinterest-style outputs.

## Solution
PinChat analyzes Pinterest pins via vision AI, extracts comprehensive style data, and generates optimized prompts that capture exact aesthetic intent.

---

## Phase 1 MVP Scope

### Core Features
1. **Pin URL Input** - Paste any Pinterest pin link
2. **Image Upload** - Direct image upload as alternative input
3. **Vision Analysis** - Gemini 3.0 extracts: colors, composition, fonts, mood, subject, medium
4. **Prompt Generation** - Outputs ready-to-use prompts for Higgsfield/Midjourney/etc
5. **Prompt History** - Save generated prompts per user session

### Out of Scope (Future)
- Pinterest OAuth integration
- Gallery sync with user boards
- Shared pin collections
- Advanced filtering by style attributes

---

## Technical Architecture

### Stack
- **Framework**: Next.js 14+ (App Router)
- **Styling**: TailwindCSS
- **Backend**: Next.js API Routes + Supabase
- **Vision AI**: Google Gemini 3.0 Flash
- **Deployment**: Vercel
- **Optional**: Pinterest API v5 for metadata enrichment

### Data Schema

```sql
-- users table (Supabase Auth handles this)

-- prompts table
create table prompts (
  id uuid primary key default uuid_generate_v4(),
  user_id uuid references auth.users(id),
  pin_url text,
  image_url text,
  vision_data jsonb, -- full Gemini response
  generated_prompt text,
  style_tags text[],
  color_palette jsonb,
  created_at timestamptz default now()
);

-- user_preferences table
create table user_preferences (
  user_id uuid primary key references auth.users(id),
  preferred_colors text[],
  preferred_moods text[],
  preferred_layouts text[],
  custom_instructions text
);
```

### Vision Data Structure (JSON)
```json
{
  "colors": {
    "dominant": ["#F4E4D7", "#2C3E50"],
    "accent": ["#E74C3C"],
    "palette_name": "warm_minimal"
  },
  "composition": {
    "layout": "centered_text_on_image",
    "grid_type": "asymmetric",
    "focal_point": "center-top"
  },
  "typography": {
    "primary_font": "sans-serif-bold",
    "style": "modern_geometric",
    "hierarchy": "title_dominant"
  },
  "mood": ["elegant", "minimal", "professional"],
  "subject": ["product_photography", "lifestyle"],
  "medium": "digital_photography",
  "brand_tone": "premium_aspirational"
}
```

---

## User Flow

1. **Landing** → User sees prompt generator interface
2. **Input** → Paste pin URL OR upload image
3. **Processing** → Vision AI analyzes (2-3 sec)
4. **Output** → Display structured prompt + metadata visualization
5. **Save** → One-click copy, optional save to history
6. **Iterate** → Adjust parameters, regenerate

---

## Success Metrics
- Prompt generation time < 5 seconds
- 80%+ user satisfaction with prompt quality
- 50%+ users save prompts for reuse
- Vision accuracy > 85% for color/composition detection

---

## Non-Functional Requirements
- Mobile responsive design
- Real-time status updates during processing
- Error handling for invalid URLs/images
- Rate limiting on API routes
- Session persistence (cookies + localStorage)
- Optional account creation for saved history

---

## Next.js API Routes

### `/api/vision/analyze` (POST)
```typescript
// app/api/vision/analyze/route.ts
export async function POST(request: Request) {
  const { image, pinUrl } = await request.json();
  
  // Call Gemini Vision API
  const visionData = await analyzeWithGemini(image);
  
  // Optionally scrape Pinterest metadata
  if (pinUrl) {
    const metadata = await scrapePinMetadata(pinUrl);
    visionData.metadata = metadata;
  }
  
  return Response.json(visionData);
}
```

### `/api/prompts` (GET/POST)
```typescript
// Get user's prompt history
export async function GET(request: Request) {
  const { searchParams } = new URL(request.url);
  const userId = searchParams.get('userId');
  
  const { data } = await supabase
    .from('prompts')
    .select('*')
    .eq('user_id', userId)
    .order('created_at', { ascending: false });
    
  return Response.json(data);
}

// Save new prompt
export async function POST(request: Request) {
  const promptData = await request.json();
  const { data } = await supabase.from('prompts').insert(promptData);
  return Response.json(data);
}
```

---

## Gemini Vision Prompt Template
```
Analyze this Pinterest design image in extreme detail:

1. COLOR ANALYSIS
- Extract exact hex codes of dominant colors (3-5)
- Identify color harmony type (complementary/analogous/triadic)
- Note color psychology and mood

2. COMPOSITION
- Layout structure (grid type, alignment, spacing)
- Visual hierarchy and focal points
- Negative space usage

3. TYPOGRAPHY
- Font families and weights (if text present)
- Type scale and spacing
- Text treatment effects

4. STYLE ATTRIBUTES
- Design medium (photography/illustration/3D/collage)
- Artistic style (minimal/maximalist/vintage/modern)
- Texture and finish (matte/glossy/grainy)

5. SUBJECT & CONTENT
- Primary subject matter
- Secondary elements
- Overall theme/concept

6. BRAND TONE
- Professional/casual spectrum
- Luxury/accessible positioning
- Emotional appeal

Return as structured JSON matching schema.
```

---

## Prompt Output Format

**Style**: [mood keywords]
**Colors**: [hex values with names]
**Composition**: [layout description]
**Subject**: [main focus]
**Medium**: [photographic/illustrated style]

**Full Prompt**:
"Transform this image into [style descriptor] aesthetic with [color treatment], featuring [composition layout]. Emphasis on [key elements]. [Mood/tone guidance]. [Technical specifications for tool: aspect ratio, quality, stylization level]."

**Tool Settings**: 
- Aspect ratio: [inferred from input]
- Style strength: [recommended 0-100]
- Coherence: [recommended setting]

---

## Development Priorities

### P0 (Must Have)
- Image upload + URL input
- Gemini vision integration via API route
- Prompt generation engine
- Copy to clipboard
- Basic responsive UI

### P1 (Should Have)
- Prompt history with localStorage + Supabase
- Style tag visualization
- Color palette preview
- Regenerate with tweaks

### P2 (Nice to Have)
- Supabase auth + persistent history
- Pinterest URL metadata scraping
- User preference learning
- Batch processing