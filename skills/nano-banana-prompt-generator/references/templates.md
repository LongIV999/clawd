# Use Case Templates

## Portrait (Face Reference)

**User input pattern:** "Create a photo of me [doing X] in [location]"

**References expected:**
- Image 1: User's face photo → `face_id`

```json
{
  "meta": {
    "aspect_ratio": "4:5",
    "quality": "ultra_photorealistic"
  },
  "references": [
    {
      "id": "user_face",
      "usage_type": "face_id",
      "apply_to": "main subject",
      "strength": 0.95
    }
  ],
  "identity_preservation": {
    "enabled": true,
    "strict_lock": true,
    "accuracy": "100%",
    "preserve": [
      "exact facial structure",
      "skin texture and tone",
      "eye shape",
      "natural features"
    ],
    "constraints": {
      "no_face_change": true,
      "no_beautify": true
    }
  },
  "subject": {
    "pose": "[USER SPECIFIED]",
    "expression": "[USER SPECIFIED]",
    "wardrobe": {
      "top": "[description]",
      "bottom": "[description]",
      "accessories": []
    }
  },
  "environment": {
    "location": "[USER SPECIFIED]",
    "time": "[golden_hour/night/etc]",
    "atmosphere": "[mood]"
  },
  "lighting": {
    "type": "[natural/studio/dramatic]",
    "direction": "[front/side/rim]"
  },
  "camera": {
    "lens": "85mm",
    "aperture": "f/1.8",
    "shot_type": "[close_up/medium/full_body]"
  },
  "negative_prompt": [
    "identity drift", "face change", "beautify filter",
    "plastic skin", "extra fingers", "blurry face"
  ]
}
```

---

## Miniature 3D (Product + Optional Face)

**User input pattern:** "Tiny workers building [product]" or "Me supervising workers on [product]"

**References expected:**
- Image 1: Product photo → `product_reference`
- Image 2 (optional): User's face → `face_id`

```json
{
  "meta": {
    "aspect_ratio": "3:4",
    "quality": "ultra_photorealistic"
  },
  "references": [
    {
      "id": "product",
      "usage_type": "product_reference",
      "apply_to": "giant monument",
      "preserve": [
        "exact design",
        "colors",
        "logo",
        "surface texture"
      ]
    },
    {
      "id": "supervisor_face",
      "usage_type": "face_id",
      "apply_to": "supervisor character",
      "strength": 0.95,
      "note": "optional - include only if user provides face ref"
    }
  ],
  "scene": {
    "concept": "product as massive structure/monument",
    "scale": "product towering like building, workers ant-sized"
  },
  "product_as_monument": {
    "position": "center, vertical or horizontal",
    "details": [
      "surface texture magnified",
      "brand elements clearly visible"
    ]
  },
  "miniature_figures": {
    "workers": {
      "count": "dozens",
      "attire": "[safety gear/white suits/uniforms]",
      "equipment": [
        "scaffolding",
        "ladders",
        "tiny tools",
        "cranes",
        "forklifts"
      ],
      "actions": [
        "polishing surface",
        "painting logo",
        "inspecting quality",
        "carrying materials"
      ]
    },
    "supervisor": {
      "include": "[true if face_id provided]",
      "position": "foreground, overseeing work",
      "scale": "larger than workers, still small vs product"
    }
  },
  "environment": {
    "surface": "[studio floor/table/themed surface]",
    "background": "[void black/gradient/themed]"
  },
  "camera": {
    "technique": "tilt-shift macro",
    "view": "wide angle",
    "depth_of_field": "shallow, product sharp"
  },
  "lighting": {
    "type": "cinematic warm",
    "highlights": "emphasize product surface"
  }
}
```

---

## Cinematic Poster (Face Reference + Title)

**User input pattern:** "[Movie genre] poster of me as [character]"

**References expected:**
- Image 1: User's face → `face_id`

```json
{
  "meta": {
    "aspect_ratio": "2:3",
    "quality": "ultra_photorealistic"
  },
  "references": [
    {
      "id": "actor",
      "usage_type": "face_id",
      "apply_to": "protagonist",
      "strength": 0.95,
      "note": "treat as real actor in scene"
    }
  ],
  "identity_preservation": {
    "enabled": true,
    "strict_lock": true,
    "accuracy": "100%"
  },
  "subject": {
    "role": "[character archetype]",
    "pose": "[heroic/dramatic/vulnerable]",
    "expression": "[determined/intense/emotional]",
    "wardrobe": {
      "description": "[genre-appropriate costume]",
      "condition": "[pristine/worn/battle-damaged]"
    }
  },
  "environment": {
    "setting": "[cinematic location]",
    "atmosphere": "[rain/fog/dust/fire]",
    "background": "[dramatic sky/destruction/cityscape]"
  },
  "lighting": {
    "style": "cinematic dramatic",
    "setup": [
      "rim light from behind",
      "soft key on face",
      "[colored accents if applicable]"
    ]
  },
  "camera": {
    "style": "movie poster / unit still",
    "lens": "[24mm heroic / 85mm portrait]",
    "angle": "[low angle heroic / eye level intimate]"
  },
  "color_grading": {
    "palette": "[teal-orange/desaturated/warm]",
    "tone": "[gritty/stylized/naturalistic]"
  },
  "text_rendering": {
    "title": {
      "content": "[EXACT TITLE]",
      "placement": "bottom-center",
      "style": "[elegant serif/bold sans/stylized]"
    }
  },
  "strict_rules": [
    "Face must match reference exactly",
    "Only render specified title text",
    "No taglines/credits unless requested",
    "Photoreal cinematic look"
  ],
  "negative_prompt": [
    "identity drift", "cartoon", "CGI look",
    "extra text", "watermark", "deformed hands"
  ]
}
```

---

## Product Photography (Product Reference)

**User input pattern:** "Product shot of [this item]"

**References expected:**
- Image 1: Product photo → `product_reference`

```json
{
  "meta": {
    "aspect_ratio": "1:1",
    "quality": "ultra_photorealistic"
  },
  "references": [
    {
      "id": "product",
      "usage_type": "product_reference",
      "preserve": [
        "exact shape",
        "colors",
        "materials",
        "logo",
        "label text",
        "surface details"
      ],
      "fidelity": "exact"
    }
  ],
  "product_preservation": {
    "enabled": true,
    "accuracy": "100%"
  },
  "composition": {
    "position": "center",
    "angle": "[front/45-degree/hero angle]",
    "framing": "[isolated/with props/lifestyle]"
  },
  "environment": {
    "setting": "professional studio",
    "surface": "[white seamless/marble/wood/gradient]",
    "props": "[none/minimal supporting elements]"
  },
  "lighting": {
    "type": "studio_softbox",
    "setup": "three-point with soft diffusion",
    "quality": "commercial advertising"
  },
  "camera": {
    "model": "Hasselblad X2D",
    "lens": "50mm",
    "aperture": "f/4.0",
    "focus": "entire product sharp"
  },
  "style": {
    "render": "e-commerce / commercial",
    "finish": "high-end advertising quality"
  }
}
```

---

## Brand Concept Store (Brand Name Only)

**User input pattern:** "Miniature [brand] store"

**References expected:**
- None required (brand name sufficient)
- Optional: Style reference for aesthetic

```json
{
  "meta": {
    "aspect_ratio": "2:3",
    "quality": "3d_render_octane"
  },
  "references": [],
  "brand": {
    "name": "[BRAND NAME]",
    "colors": "[brand colors]",
    "icon_product": "[signature product shape for exterior]"
  },
  "subject": {
    "type": "3D chibi-style miniature concept store",
    "exterior": "shaped like brand's iconic product",
    "structure": [
      "two floors",
      "large glass windows",
      "visible interior"
    ],
    "interior": [
      "brand-colored decor",
      "warm lighting",
      "staff in uniforms"
    ]
  },
  "environment": {
    "setting": "miniature urban street",
    "elements": [
      "tiny figures walking",
      "benches",
      "street lamps",
      "potted plants"
    ]
  },
  "style": {
    "render": "Cinema 4D",
    "aesthetic": "blind-box toy",
    "lighting": "soft afternoon glow"
  }
}
```

---

## Couple/Multi-Person (Multiple Face References)

**User input pattern:** "Photo of me and [person] together"

**References expected:**
- Image 1: Person A face → `face_id`
- Image 2: Person B face → `face_id`

```json
{
  "meta": {
    "aspect_ratio": "3:4",
    "quality": "ultra_photorealistic"
  },
  "references": [
    {
      "id": "person_A",
      "usage_type": "face_id",
      "apply_to": "character_left",
      "strength": 0.95
    },
    {
      "id": "person_B",
      "usage_type": "face_id",
      "apply_to": "character_right",
      "strength": 0.95
    }
  ],
  "identity_preservation": {
    "person_A": {"strict_lock": true, "accuracy": "100%"},
    "person_B": {"strict_lock": true, "accuracy": "100%"}
  },
  "subjects": [
    {
      "id": "character_left",
      "position": "left/foreground",
      "pose": "[description]",
      "wardrobe": {}
    },
    {
      "id": "character_right",
      "position": "right/behind",
      "pose": "[description]",
      "wardrobe": {}
    }
  ],
  "interaction": "[embracing/standing together/looking at each other]",
  "environment": {},
  "lighting": {},
  "camera": {},
  "strict_rules": [
    "Exactly 2 people only",
    "Both faces must match their respective references",
    "No identity drift or face swapping"
  ]
}
```

---

## Infographic (No References Usually)

**User input pattern:** "Timeline/chart/dashboard about [topic]"

**References expected:**
- Usually none
- Optional: Style reference for aesthetic

```json
{
  "meta": {
    "aspect_ratio": "9:16",
    "quality": "vector_illustration"
  },
  "references": [],
  "type": "[timeline/dashboard/flowchart/comparison]",
  "content": {
    "title": "[EXACT TITLE TEXT]",
    "data_points": "[list of items to visualize]"
  },
  "layout": {
    "structure": "[vertical timeline/grid/flow]",
    "hierarchy": "clear visual priority"
  },
  "visual_style": {
    "background": "[white/dark/gradient]",
    "colors": "[palette]",
    "typography": "[modern sans-serif/etc]"
  },
  "text_rendering": {
    "enabled": true,
    "elements": [
      {"content": "[text 1]", "placement": "[position]"},
      {"content": "[text 2]", "placement": "[position]"}
    ],
    "strict_spelling": true
  }
}
```
