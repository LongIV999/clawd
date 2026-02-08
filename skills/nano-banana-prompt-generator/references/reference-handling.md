# Reference Image Handling

## Overview

Nano Banana Pro accepts up to 14 reference images per prompt. Each reference must be mapped to a `usage_type` that tells the model HOW to use that image.

---

## Usage Types Detailed

### face_id
**Purpose:** Preserve exact facial identity  
**Use when:** User says "me", "my face", "photo of myself"

```json
{
  "id": "face_ref",
  "usage_type": "face_id",
  "apply_to": "[character name/role]",
  "strength": 0.95,
  "preserve": [
    "facial structure",
    "proportions", 
    "skin texture",
    "skin tone",
    "eye shape and color",
    "nose shape",
    "lip shape",
    "natural marks (moles, freckles, scars)"
  ]
}
```

**Strength guide:**
- 0.99: Exact match, minimal creative freedom
- 0.95: Very close, slight lighting adaptation
- 0.90: Recognizable, allows expression changes
- 0.85: Similar, allows more artistic interpretation

---

### pose_copy
**Purpose:** Copy body position and skeleton  
**Use when:** User provides a pose reference image

```json
{
  "id": "pose_ref",
  "usage_type": "pose_copy",
  "apply_to": "[character]",
  "strength": 0.85,
  "copy": [
    "body position",
    "limb arrangement", 
    "head angle",
    "hand positions"
  ]
}
```

---

### clothing_transfer
**Purpose:** Apply outfit from reference to subject  
**Use when:** User wants specific clothes on a character

```json
{
  "id": "outfit_ref",
  "usage_type": "clothing_transfer",
  "apply_to": "[character]",
  "strength": 0.9,
  "transfer": [
    "garment type",
    "colors",
    "patterns",
    "fit and drape"
  ]
}
```

---

### product_reference
**Purpose:** Keep product design exactly as shown  
**Use when:** User provides product/item photo

```json
{
  "id": "product_ref",
  "usage_type": "product_reference",
  "apply_to": "[product in scene]",
  "preserve": [
    "exact shape and proportions",
    "colors and materials",
    "logo and branding",
    "label text",
    "surface texture",
    "distinctive features"
  ],
  "fidelity": "exact"
}
```

---

### style_transfer
**Purpose:** Apply artistic style/mood from reference  
**Use when:** User says "style like this", "this aesthetic"

```json
{
  "id": "style_ref",
  "usage_type": "style_transfer",
  "strength": 0.7,
  "apply": [
    "color grading",
    "lighting mood",
    "artistic style",
    "texture/grain"
  ]
}
```

**Note:** Lower strength (0.5-0.7) allows blending with other styles

---

### composition_reference
**Purpose:** Match layout and arrangement  
**Use when:** User provides layout/wireframe/sketch

```json
{
  "id": "layout_ref",
  "usage_type": "composition_reference",
  "strength": 0.8,
  "match": [
    "element positions",
    "framing",
    "negative space",
    "visual hierarchy"
  ]
}
```

---

### depth_map
**Purpose:** Copy 3D spatial structure  
**Use when:** User wants same depth/perspective

```json
{
  "id": "depth_ref",
  "usage_type": "depth_map",
  "strength": 0.85,
  "preserve": [
    "spatial depth",
    "foreground/background relationship",
    "perspective"
  ]
}
```

---

### full_character_reference
**Purpose:** Entire character (face + body + clothes)  
**Use when:** User wants character consistency across images

```json
{
  "id": "character_ref",
  "usage_type": "full_character_reference",
  "strength": 0.9,
  "preserve": [
    "face identity",
    "body type",
    "outfit",
    "overall appearance"
  ]
}
```

---

## Multi-Reference Combinations

### Portrait with Pose Reference
```json
"references": [
  {"usage_type": "face_id", "apply_to": "subject", "strength": 0.95},
  {"usage_type": "pose_copy", "apply_to": "subject", "strength": 0.85}
]
```

### Product Scene with Style
```json
"references": [
  {"usage_type": "product_reference", "apply_to": "main product"},
  {"usage_type": "style_transfer", "apply_to": "overall image", "strength": 0.6}
]
```

### Character in New Environment
```json
"references": [
  {"usage_type": "face_id", "apply_to": "character", "strength": 0.95},
  {"usage_type": "clothing_transfer", "apply_to": "character", "strength": 0.9},
  {"usage_type": "style_transfer", "apply_to": "environment", "strength": 0.7}
]
```

### Multiple Characters
```json
"references": [
  {"id": "person_A", "usage_type": "face_id", "apply_to": "character_left", "strength": 0.95},
  {"id": "person_B", "usage_type": "face_id", "apply_to": "character_right", "strength": 0.95}
]
```

---

## Common Patterns

| Scenario | References Needed |
|----------|-------------------|
| "Me in a new scene" | face_id |
| "Me wearing this outfit" | face_id + clothing_transfer |
| "Me in this pose" | face_id + pose_copy |
| "Product as giant monument" | product_reference |
| "Product with my face" | product_reference + face_id |
| "Style like this image" | style_transfer |
| "Layout like this sketch" | composition_reference |
| "Couple photo" | face_id × 2 |
| "Character consistency" | full_character_reference |

---

## Limits & Best Practices

| Limit | Value | Note |
|-------|-------|------|
| Max references | 14 | Hard limit |
| Max face_id for consistency | 5 | Beyond this, quality drops |
| Recommended for portraits | 1-2 | face + optional pose |
| Recommended for products | 1 | product_reference only |

**Best Practices:**
1. Order references by importance (first = highest priority)
2. Don't mix conflicting style_transfers
3. Use lower strength for artistic freedom
4. Higher strength = more faithful but less creative
