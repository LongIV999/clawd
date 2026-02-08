---
name: nano-banana-prompt-generator
description: >
  Generate professional JSON prompts for Google Nano Banana Pro (Gemini 3 Pro Image).
  Outputs: structured JSON prompt ready to paste into Gemini.
  Triggers: create nano banana prompt, generate image prompt, /nanoprompt, make me an AI image prompt, nano banana JSON
---

# Nano Banana Pro Prompt Generator

## Purpose

Transform user intent + reference images into production-ready JSON prompts for Nano Banana Pro. Handle up to 14 reference images with proper `usage_type` mapping.

**Tích hợp Brand Context:** Nếu đang trong Project có `image-generation-guide.md`, tự động apply brand visual defaults.

---

## Phase 0: Brand Context Check (LUÔN CHẠY TRƯỚC)

```
TRƯỚC KHI làm bất cứ gì:

1. CHECK Project Knowledge cho image-generation-guide.md
   NẾU có:
       → Đọc file
       → Extract: style, mood, colors, lighting defaults, avoid list
       → Store để apply vào JSON output
       → Note: "Đã đọc brand visual guidelines"

2. CHECK Project Knowledge cho visual-style.md (backup)
   NẾU có và không có image-generation-guide.md:
       → Đọc file
       → Extract: colors, typography hints, overall aesthetic
       → Store để apply

3. NẾU không có brand files:
       → Proceed bình thường
       → Dùng generic defaults
```

### Brand Defaults Template

```json
// Từ image-generation-guide.md, extract:
{
  "brand_defaults": {
    "quality": "[từ file]",
    "lighting": {
      "type": "[từ file]",
      "mood": "[từ file]"
    },
    "color_grading": {
      "palette": "[từ file]",
      "tone": "[từ file]"
    },
    "style_keywords": ["[từ file]"],
    "avoid": ["[từ file]"]
  }
}
```

---

## Phase 1: Input Collection

### Entry Check

```
IF user provided:
    - Subject/concept (what to generate)
    - Reference images OR description of refs they will upload
    → Proceed to Phase 2

ELSE
    → Gather missing information (ONE question at a time)
```

### Discovery Questions

| Missing | Question |
|---------|----------|
| Subject | "Bạn muốn tạo hình gì?" |
| References | "Có reference images không? (ảnh mặt, sản phẩm, style mẫu...)" |
| Use case | "Dùng cho gì? (social media, slide, poster, product shot...)" |

---

## Phase 2: Reference Image Mapping

### Identify Reference Types

```
FOR each reference image user mentions:
    
    IF "me", "my face", "my photo", "selfie":
        → usage_type: "face_id"
        → identity_lock: true
        → strength: 0.95
    
    IF "my body", "pose like this", "position":
        → usage_type: "pose_copy"
        → strength: 0.85
    
    IF "wearing this", "this outfit", "clothes":
        → usage_type: "clothing_transfer"
        → strength: 0.9
    
    IF "product", "this item", "the shoe/bottle/etc":
        → usage_type: "product_reference"
        → preserve: ["design", "colors", "logo", "proportions"]
    
    IF "style like this", "aesthetic", "mood reference":
        → usage_type: "style_transfer"
        → strength: 0.7
    
    IF "layout", "composition", "arrange like this":
        → usage_type: "composition_reference"
        → strength: 0.8
    
    IF "depth", "3D shape", "structure":
        → usage_type: "depth_map"
        → strength: 0.85
```

### Reference Limits

```
WARN if references > 14:
    → "Nano Banana supports max 14 reference images"

RECOMMEND if character refs > 5:
    → "For best consistency, limit to 5 character references"
```

---

## Phase 3: Use Case Detection

```
IF subject mentions:
    - "product", "bottle", "packaging", "e-commerce" → PRODUCT
    - "portrait", "me", "person", "face" → PORTRAIT  
    - "miniature", "tiny workers", "3D diorama" → MINIATURE
    - "infographic", "chart", "data" → INFOGRAPHIC
    - "poster", "movie", "cinematic" → CINEMATIC
    - "brand", "logo", "UI" → BRAND
    - "anime", "manga", "illustration" → STYLIZED
    - "architecture", "interior", "floor plan" → ARCHITECTURE
    - "slide", "presentation", "background" → SLIDE_BACKGROUND
    - "social", "post", "thumbnail" → SOCIAL_MEDIA
```

---

## Phase 4: JSON Construction

### Base Structure (Always Include)

```json
{
  "meta": {
    "aspect_ratio": "[from use case]",
    "quality": "[from brand OR use case]"
  },
  "references": [],
  "subject": {},
  "environment": {},
  "lighting": {},
  "camera": {},
  "color_grading": {}
}
```

### Apply Brand Defaults

```
KHAI generate JSON:

NẾU có brand_defaults từ Phase 0:
    → Merge brand_defaults vào base structure
    → Brand values làm baseline
    → User-specified values override brand defaults
    
NẾU không có brand_defaults:
    → Dùng generic values
```

### Reference Block Template

```json
"references": [
  {
    "id": "ref_1",
    "description": "[what this image contains]",
    "usage_type": "[face_id|pose_copy|clothing_transfer|product_reference|style_transfer|composition_reference|depth_map]",
    "apply_to": "[which element in the scene]",
    "strength": 0.0-1.0,
    "preserve": ["list of elements to keep exact"]
  }
]
```

### Identity Lock Block (When face_id used)

```json
"identity_preservation": {
  "enabled": true,
  "strict_lock": true,
  "accuracy": "100%",
  "preserve": [
    "exact facial structure",
    "proportions",
    "skin texture and tone",
    "eye shape",
    "nose",
    "lips",
    "natural details (moles, freckles)"
  ],
  "constraints": {
    "no_face_change": true,
    "no_beautify": true,
    "no_identity_merge": true
  }
}
```

### Product Reference Block (When product_reference used)

```json
"product_preservation": {
  "enabled": true,
  "preserve": [
    "exact design",
    "colors and materials",
    "logo and branding",
    "proportions and shape",
    "surface texture"
  ],
  "fidelity": "high"
}
```

---

## Phase 5: Use Case Templates

### SLIDE_BACKGROUND (Mới - cho presentations)

```json
{
  "meta": {"aspect_ratio": "16:9", "quality": "[brand OR ultra_photorealistic]"},
  "subject": {
    "type": "abstract background / conceptual visual",
    "composition": "minimal, non-distracting, space for text overlay",
    "concept": "[từ user input - ví dụ: constraint as catalyst]"
  },
  "environment": {
    "setting": "[match concept]",
    "depth": "layered for visual interest"
  },
  "lighting": {
    "type": "[brand default OR soft ambient]",
    "mood": "[brand default OR match concept]"
  },
  "color_grading": {
    "palette": "[brand colors]",
    "saturation": "slightly muted for text readability",
    "tone": "[brand default]"
  },
  "style_keywords": ["[brand keywords]", "presentation-ready", "professional"],
  "avoid": ["[brand avoid list]", "busy patterns", "text-competing elements"]
}
```

### SOCIAL_MEDIA (cho posts, thumbnails)

```json
{
  "meta": {"aspect_ratio": "[1:1 or 4:5]", "quality": "[brand default]"},
  "subject": {
    "type": "[from user]",
    "composition": "center focus, eye-catching"
  },
  "lighting": "[brand defaults]",
  "color_grading": "[brand defaults]",
  "style_keywords": ["[brand keywords]", "scroll-stopping", "vibrant"],
  "avoid": ["[brand avoid list]"]
}
```

### PORTRAIT (with face reference)

```json
{
  "meta": {"aspect_ratio": "4:5", "quality": "[brand OR ultra_photorealistic]"},
  "references": [
    {
      "id": "face_ref",
      "usage_type": "face_id",
      "strength": 0.95,
      "apply_to": "main subject"
    }
  ],
  "identity_preservation": {
    "enabled": true,
    "strict_lock": true,
    "accuracy": "100%"
  },
  "subject": {
    "pose": "[description]",
    "expression": "[description]",
    "wardrobe": {}
  },
  "environment": {},
  "lighting": {"type": "[brand OR natural]", "direction": "[direction]"},
  "camera": {"lens": "85mm", "aperture": "f/1.8"},
  "color_grading": "[brand defaults if available]"
}
```

### MINIATURE (with product + optional face)

```json
{
  "meta": {"aspect_ratio": "2:3", "quality": "[brand OR ultra_photorealistic]"},
  "references": [
    {
      "id": "product_ref",
      "usage_type": "product_reference",
      "apply_to": "giant monument object",
      "preserve": ["design", "colors", "logo"]
    },
    {
      "id": "face_ref",
      "usage_type": "face_id",
      "apply_to": "supervisor character",
      "strength": 0.95
    }
  ],
  "scene": {
    "concept": "product as massive monument",
    "scale": "product towering, workers tiny"
  },
  "miniature_figures": {
    "types": ["workers", "engineers"],
    "equipment": ["scaffolding", "tools"],
    "actions": ["building", "polishing", "inspecting"]
  },
  "camera": {"technique": "tilt-shift macro"},
  "lighting": "[brand defaults OR cinematic warm]",
  "color_grading": "[brand defaults]"
}
```

### CINEMATIC (with face reference)

```json
{
  "meta": {"aspect_ratio": "2:3", "quality": "ultra_photorealistic"},
  "references": [
    {
      "id": "actor_ref",
      "usage_type": "face_id",
      "strength": 0.95,
      "note": "treat as real actor in scene"
    }
  ],
  "identity_preservation": {"enabled": true, "strict_lock": true},
  "subject": {
    "role": "[character type]",
    "wardrobe": {},
    "pose": "[dramatic pose]"
  },
  "environment": {"setting": "[cinematic location]"},
  "lighting": {"style": "cinematic", "setup": ["rim light", "key light"]},
  "camera": {"style": "movie still", "lens": "35mm"},
  "color_grading": "[brand defaults OR teal-orange cinematic]",
  "text_rendering": {
    "title": {"content": "[TITLE]", "placement": "bottom-center"}
  }
}
```

### PRODUCT (with product reference)

```json
{
  "meta": {"aspect_ratio": "1:1", "quality": "ultra_photorealistic"},
  "references": [
    {
      "id": "product_ref",
      "usage_type": "product_reference",
      "preserve": ["exact design", "colors", "logo", "label text"]
    }
  ],
  "product_preservation": {"enabled": true, "fidelity": "high"},
  "subject": {
    "position": "center",
    "angle": "[front/45-degree/etc]"
  },
  "environment": {"setting": "studio", "surface": "[brand appropriate OR white]"},
  "lighting": {"type": "studio_softbox", "quality": "commercial"},
  "camera": {"lens": "50mm", "aperture": "f/4.0"},
  "color_grading": "[brand defaults]"
}
```

---

## Phase 6: Output

### Output Format

```markdown
## 🍌 Nano Banana Pro Prompt

**Brand Applied:** [Tên brand nếu có] ✅ / Generic ⚪

**Reference Setup:**
- Image 1: [usage_type] → [what it controls]
- Image 2: [usage_type] → [what it controls]

**JSON Prompt:**
```json
{complete JSON here với brand defaults đã merge}
```

**Usage:**
1. Upload reference images to Gemini (theo thứ tự trên)
2. Paste JSON prompt
3. Enable "Thinking" mode for best results

**Brand Elements Applied:**
- Colors: [từ brand]
- Mood: [từ brand]
- Style: [từ brand]
- Avoided: [từ brand avoid list]
```

---

## Reference Usage Types Quick Guide

| Type | When to Use | Strength |
|------|-------------|----------|
| `face_id` | Preserve exact face identity | 0.9-0.99 |
| `pose_copy` | Copy body position/skeleton | 0.8-0.9 |
| `clothing_transfer` | Transfer outfit to subject | 0.85-0.95 |
| `product_reference` | Keep product design exact | N/A (preserve list) |
| `style_transfer` | Apply artistic style/mood | 0.6-0.8 |
| `composition_reference` | Match layout/arrangement | 0.7-0.85 |
| `depth_map` | Copy 3D spatial structure | 0.8-0.9 |
| `full_character_reference` | Entire character (face+body+clothes) | 0.9-0.95 |

---

## Self-Check (Read before EVERY response)

```
□ Đã đọc image-generation-guide.md TRƯỚC TIÊN chưa?
  → Nếu có trong Project → PHẢI đọc và apply
  → Nếu không có → proceed với generic

□ Đã identify ALL reference images chưa?
  → Map each to correct usage_type

□ Đã include identity_preservation cho face references chưa?
  → strict_lock: true, accuracy: "100%"

□ Đã include product_preservation cho product references chưa?
  → List what to preserve exactly

□ Đã apply brand defaults vào JSON chưa?
  → Colors, lighting, mood, style_keywords, avoid

□ Strength value phù hợp cho mỗi reference chưa?
  → Higher = more faithful, Lower = more creative

□ Aspect_ratio match use case chưa?
  → Slide: 16:9, Portrait: 4:5, Product: 1:1, Social: 1:1 or 4:5

□ JSON valid chưa?
  → No trailing commas, proper brackets

□ Đã explain brand elements được apply chưa?
  → User biết brand guidelines đã được dùng
```

---

## Brand Learning

```
SAU KHI generate prompt, NẾU user chỉnh JSON:
    → Analyze: Chỉnh gì? Colors? Mood? Style?
    → Store trong references/learned-patterns.md
    
SAU 3+ similar corrections:
    → Suggest: "Mình notice bạn prefer [X]. Update image-generation-guide.md?"
```
