---
title: "P5 — Plant Disease Detection"
weight: 50
bookFlatSection: true
---

# 🌱 P5 — Plant Disease Detection (Tool C)

## Objective

Build a mobile-friendly plant disease detection tool — first as an MVP using PlantNet API, then as a permanent on-device TensorFlow.js PWA that runs offline with a custom CNN trained on PlantVillage dataset, outputting ZBNF treatment suggestions.

## Prerequisites

- Node.js ≥ 18 or Python ≥ 3.10
- For Approach 2: TensorFlow/Keras installed, PlantVillage dataset downloaded

## Subtasks

### Phase 1: MVP — PlantNet API Integration

- [ ] Register at [my.plantnet.org](https://my.plantnet.org) for free API key (500 req/day)
- [ ] Build a simple HTML/JS page with camera input (`<input type="file" accept="image/*" capture>`)
- [ ] On photo capture, send to PlantNet identification API
- [ ] Parse response: plant species + disease identification
- [ ] Map identified disease → ZBNF treatment from a local JSON lookup table:
  ```json
  {
    "Rice Blast": "Neemastra foliar spray + extra Jeevamrutha at root zone",
    "Leaf Blight": "Brahmastra spray (neem + custard apple + datura leaf mix)",
    "Bacterial Wilt": "Beejamrutha root drench + increase mulch",
    "Powdery Mildew": "Sour buttermilk spray (1:10 dilution)"
  }
  ```
- [ ] Display result: disease name (Bangla + English) + treatment + confidence score
- [ ] Deploy to GitHub Pages / Netlify

### Phase 2: Dataset Preparation (for On-Device Model)

- [ ] Download PlantVillage dataset from Kaggle (54,000+ images, 38 classes)
- [ ] Filter to Bangladesh-relevant crops: Rice, Tomato, Potato, Corn, Pepper
- [ ] Organize into `train/` and `val/` directories (80/20 split)
- [ ] Apply data augmentation: rotation, flip, brightness, zoom
- [ ] Document class distribution and handle class imbalance

### Phase 3: CNN Model Training

- [ ] Train a lightweight CNN (MobileNetV2 or EfficientNet-Lite as base) using TensorFlow/Keras
- [ ] Transfer learning: freeze base layers, train classification head
- [ ] Target metrics: >90% accuracy on validation set for top-5 diseases
- [ ] Export model in two formats:
  - TensorFlow.js (`tfjs_graph_model`) for browser inference
  - TFLite for potential native mobile use
- [ ] Model size target: 5–15 MB (quantized)

### Phase 4: On-Device PWA

- [ ] Create React + Vite PWA (reuse `vite-plugin-pwa` setup from P3)
- [ ] Bundle TF.js model files with the PWA
- [ ] Implement camera capture → image preprocessing → TF.js inference pipeline
- [ ] Display: top-3 predictions with confidence + ZBNF treatment for top match
- [ ] All output in Bangla with English subtitles
- [ ] Service Worker caches model files for offline use
- [ ] Test: works in airplane mode after first load

### Phase 5: Treatment Knowledge Base

- [ ] Create `data/treatments.json` with comprehensive disease → treatment mapping
- [ ] Include for each disease: Bangla name, English name, symptoms, ZBNF treatment steps, prevention tips
- [ ] Add photo references for each disease (compressed WebP, <50KB each)
- [ ] Link to relevant ZBNF formulation recipes (cross-reference Tool F)

## Acceptance Criteria

- [ ] MVP (PlantNet): photo upload → disease + treatment in <5 seconds
- [ ] On-device: photo → inference → result in <3 seconds on mid-range Android
- [ ] Model accuracy >90% for top-5 BD crop diseases
- [ ] PWA works fully offline after initial load
- [ ] All results display in Bangla with treatment steps
- [ ] Model size ≤ 15 MB

## Estimated Effort

⏱️ **1 day** (MVP) + **3–5 days** (on-device) = **4–6 days total**

## Dependencies

| Dependency | Status |
|---|---|
| None (independent pipeline) | — |
| PlantVillage dataset download | Required for Phase 2 |
