# Example Prompts with References

## Example 1: Me on Motorcycle

**User says:** "I have a photo of myself. Create me riding a Harley Davidson, cinematic style"

**Reference mapping:**
- Image 1 (user face) → `face_id`

**Generated prompt:**

```json
{
  "meta": {
    "aspect_ratio": "3:4",
    "quality": "ultra_photorealistic"
  },
  "references": [
    {
      "id": "user_face",
      "usage_type": "face_id",
      "apply_to": "rider",
      "strength": 0.95
    }
  ],
  "identity_preservation": {
    "enabled": true,
    "strict_lock": true,
    "accuracy": "100%",
    "preserve": ["facial structure", "skin tone", "features"],
    "constraints": {"no_beautify": true, "no_face_change": true}
  },
  "subject": {
    "action": "riding motorcycle",
    "pose": {
      "body": "leaning naturally into ride",
      "hands": "on handlebars",
      "head": "glancing confidently to side"
    },
    "expression": "confident, free-spirited",
    "wardrobe": {
      "top": "black leather jacket, white t-shirt underneath",
      "bottom": "dark jeans",
      "accessories": ["aviator sunglasses", "wristwatch"]
    }
  },
  "vehicle": {
    "type": "Harley Davidson",
    "model": "classic cruiser",
    "color": "black with chrome accents"
  },
  "environment": {
    "setting": "open highway",
    "time": "golden hour",
    "background": "blurred landscape, warm sky"
  },
  "lighting": {
    "type": "natural golden hour",
    "direction": "side-lit",
    "effect": "warm rim light on subject"
  },
  "camera": {
    "style": "cinematic",
    "lens": "35mm",
    "motion": "subtle motion blur on background",
    "grain": "vintage film grain"
  },
  "mood": "freedom, confidence, adventure"
}
```

---

## Example 2: Tiny Workers on My Sneaker

**User says:** "I have a photo of my Nike Air Jordan. Create tiny workers building it like a monument"

**Reference mapping:**
- Image 1 (shoe photo) → `product_reference`

**Generated prompt:**

```json
{
  "meta": {
    "aspect_ratio": "2:3",
    "quality": "ultra_photorealistic"
  },
  "references": [
    {
      "id": "sneaker",
      "usage_type": "product_reference",
      "apply_to": "giant monument",
      "preserve": [
        "exact shoe design",
        "colorway",
        "Nike swoosh",
        "Air Jordan logo",
        "leather texture",
        "lace pattern"
      ],
      "fidelity": "exact"
    }
  ],
  "product_preservation": {
    "enabled": true,
    "accuracy": "100%"
  },
  "scene": {
    "concept": "sneaker as massive architectural monument",
    "scale": "shoe towering like skyscraper, workers ant-sized"
  },
  "subject": {
    "main_element": "Air Jordan 1 standing vertically",
    "position": "center, dominating frame",
    "details": [
      "premium leather grain visible at macro level",
      "stitching details magnified"
    ]
  },
  "miniature_figures": {
    "workers": {
      "count": "dozens",
      "attire": "high-visibility safety gear, hard hats",
      "equipment": [
        "industrial scaffolding wrapping shoe",
        "miniature spray-paint rigs",
        "rope systems",
        "tiny cranes"
      ],
      "actions": [
        "painting the panels",
        "polishing the swoosh",
        "stitching leather seams",
        "quality inspection with magnifiers"
      ]
    }
  },
  "environment": {
    "background": "void-like black studio",
    "surface": "clean industrial floor",
    "atmosphere": "professional product shoot"
  },
  "lighting": {
    "type": "dramatic studio",
    "setup": [
      "rim lighting emphasizing shoe contours",
      "sharp spotlights on leather texture"
    ]
  },
  "camera": {
    "technique": "tilt-shift macro",
    "lens": "macro 100mm",
    "depth_of_field": "shallow, shoe sharp, some workers soft",
    "quality": "8k, ultra-detailed"
  }
}
```

---

## Example 3: Couple Christmas Photo

**User says:** "I have photos of me and my girlfriend. Create us in a Christmas living room scene"

**Reference mapping:**
- Image 1 (user face) → `face_id` → character A
- Image 2 (girlfriend face) → `face_id` → character B

**Generated prompt:**

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
      "apply_to": "man (background)",
      "strength": 0.95
    },
    {
      "id": "person_B",
      "usage_type": "face_id",
      "apply_to": "woman (foreground)",
      "strength": 0.95
    }
  ],
  "identity_preservation": {
    "person_A": {"strict_lock": true, "accuracy": "100%"},
    "person_B": {"strict_lock": true, "accuracy": "100%"}
  },
  "subjects": [
    {
      "id": "woman",
      "position": "foreground, slightly angled to camera",
      "expression": "warm smile",
      "wardrobe": {
        "dress": "strapless black textured dress",
        "accessories": ["small earrings"]
      },
      "props": "holding wine glass with red wine"
    },
    {
      "id": "man",
      "position": "behind her, leaning in from left",
      "expression": "happy smile at camera",
      "pose": "arm around her waist",
      "wardrobe": {
        "top": "cream short-sleeve button shirt",
        "bottom": "beige pants"
      }
    }
  ],
  "interaction": "intimate couple pose, his arm around her waist",
  "environment": {
    "location": "cozy living room at night",
    "christmas_tree": {
      "position": "behind couple",
      "decoration": "red ribbon garlands, gold ornaments, warm lights"
    },
    "props": "wrapped presents and gift bags in foreground on floor"
  },
  "lighting": {
    "type": "on-camera flash",
    "ambient": "warm glow from tree lights",
    "style": "authentic home photo"
  },
  "camera": {
    "style": "candid flash photography",
    "lens": "35mm equivalent",
    "angle": "slightly above eye level"
  },
  "strict_rules": [
    "Exactly 2 people only",
    "Both faces must match references exactly",
    "No identity drift between the two",
    "Photoreal candid look, not studio"
  ],
  "negative_prompt": [
    "extra people", "identity drift", "face swap artifacts",
    "beautify filter", "CGI look"
  ]
}
```

---

## Example 4: Me Supervising Workers on Product

**User says:** "I have a photo of myself and a photo of a Coca-Cola can. Create me as a giant supervisor watching tiny workers polish the can"

**Reference mapping:**
- Image 1 (user face) → `face_id` → supervisor
- Image 2 (Coke can) → `product_reference` → giant can

**Generated prompt:**

```json
{
  "meta": {
    "aspect_ratio": "3:4",
    "quality": "ultra_photorealistic"
  },
  "references": [
    {
      "id": "user_face",
      "usage_type": "face_id",
      "apply_to": "supervisor figure",
      "strength": 0.95
    },
    {
      "id": "coke_can",
      "usage_type": "product_reference",
      "apply_to": "giant can monument",
      "preserve": [
        "exact Coca-Cola design",
        "red color",
        "white logo",
        "can shape and proportions"
      ]
    }
  ],
  "identity_preservation": {
    "enabled": true,
    "strict_lock": true
  },
  "product_preservation": {
    "enabled": true,
    "fidelity": "exact"
  },
  "scene": {
    "concept": "supervisor overseeing miniature construction",
    "scale_hierarchy": [
      "Coca-Cola can: massive, building-sized",
      "Supervisor (user): medium, human-scale but smaller than can",
      "Workers: tiny, ant-sized"
    ]
  },
  "subjects": {
    "supervisor": {
      "identity": "from face reference",
      "position": "foreground right, overseeing",
      "pose": "standing confidently, arms crossed or pointing",
      "wardrobe": "business casual or foreman attire",
      "expression": "focused, authoritative"
    },
    "can_monument": {
      "position": "center-left, towering",
      "details": "condensation droplets magnified, logo crisp"
    }
  },
  "miniature_figures": {
    "workers": {
      "count": "many",
      "attire": "white safety suits",
      "equipment": ["scaffolding", "polishing tools", "spray rigs"],
      "actions": ["polishing surface", "cleaning droplets", "touching up logo"]
    }
  },
  "environment": {
    "surface": "industrial floor or themed surface",
    "background": "gradient or studio void"
  },
  "lighting": {
    "type": "cinematic product lighting",
    "highlights": "on can surface and supervisor"
  },
  "camera": {
    "technique": "tilt-shift",
    "perspective": "shows scale contrast clearly"
  }
}
```

---

## Example 5: Product Shot with Style Reference

**User says:** "I have my perfume bottle photo and a mood board image. Create a luxury product shot matching that aesthetic"

**Reference mapping:**
- Image 1 (perfume) → `product_reference`
- Image 2 (mood board) → `style_transfer`

**Generated prompt:**

```json
{
  "meta": {
    "aspect_ratio": "4:5",
    "quality": "ultra_photorealistic"
  },
  "references": [
    {
      "id": "perfume_bottle",
      "usage_type": "product_reference",
      "apply_to": "main product",
      "preserve": [
        "exact bottle shape",
        "glass material",
        "cap design",
        "label and branding"
      ],
      "fidelity": "exact"
    },
    {
      "id": "mood_reference",
      "usage_type": "style_transfer",
      "apply_to": "overall image aesthetic",
      "strength": 0.7,
      "transfer": [
        "color grading",
        "lighting mood",
        "surface textures",
        "atmosphere"
      ]
    }
  ],
  "product_preservation": {
    "enabled": true,
    "accuracy": "100%"
  },
  "composition": {
    "position": "center",
    "angle": "hero angle, slightly elevated",
    "framing": "product dominant, elegant negative space"
  },
  "environment": {
    "setting": "luxury studio",
    "surface": "reflective golden or marble",
    "props": "minimal, elegant accents if any"
  },
  "lighting": {
    "type": "soft beauty lighting",
    "quality": "high-end commercial",
    "reflections": "controlled, elegant on glass"
  },
  "camera": {
    "model": "Hasselblad",
    "lens": "85mm",
    "aperture": "f/2.8"
  },
  "style": {
    "aesthetic": "luxury fragrance advertising",
    "mood": "sophisticated, aspirational"
  }
}
```
