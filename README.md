# 📐 AI-Photogrammetry-Prompt

A highly reliable, math-driven system prompt that transforms standard Multimodal AI models (like Gemini or GPT-4) into makeshift optical micrometers. 

By forcing the AI to combine **object segmentation**, **monocular depth mapping**, and **perspective scaling ($1/Z$)**, this prompt allows users to extract real-world physical measurements within a **~0.5 cm error margin**—even when using low-quality, low-light, or distorted webcam images.

---

## 🚀 The Core Problem It Solves

Standard AI vision models view the world as a flat, 2D grid of pixels. To an unguided AI, a tiny object close to the camera lens looks exactly the same size as a massive object far away. This is known as the **"Flattening" Problem** of computer vision.

This prompt bypasses that limitation by forcing the AI to dynamically build a relative depth map using a known background or foreground anchor (like a standard light switch, credit card, or soda can) and mathematically compensating for perspective distortion.

---

## 🛠️ How It Works (The Math & Science)

The prompt instructs the AI's vision processing layers to execute a 4-step computer vision pipeline natively through natural language:

1. **Anchor Calibration:** The AI identifies a universally standard object in the frame (e.g., an International Single Light Switch Plate, $8.6\text{ cm} \times 8.6\text{ cm}$) and calculates the raw background pixel-to-cm ratio:
   $$\text{Ratio}_{\text{wall}} = \frac{\text{Actual Size}}{\text{Pixel Count}}$$

2. **Depth Mapping:** The model estimates the relative distances ($Z$) of both the anchor plane and the target plane from the camera lens.

3. **Perspective Compensation:** The AI applies the inverse proportional law of perspective foreshortening to scale the pixel-to-cm ratio across depth planes using a scaling factor ($\alpha$):
   $$\alpha = \frac{Z_{\text{target}}}{Z_{\text{anchor}}}$$
   $$\text{Ratio}_{\text{target}} = \text{Ratio}_{\text{wall}} \times \alpha$$

4. **Virtual Segmentation:** The AI isolates the target object's pixel boundaries (and sub-components like a watch face vs. its screen) and converts them to precise physical measurements.

---

## 📋 The System Prompt

Copy and paste the markdown block below into any advanced Multimodal AI chat interface along with your image:

```markdown
# Universal Monocular Photogrammetry & Spatial Estimation Prompt

**Purpose:** To extract highly accurate real-world measurements (within ~0.5cm) of a target foreground object from a single, low-quality 2D image by using a known background object as a spatial anchor.

## User Instructions:
1. Provide an image containing a target object to measure (foreground) and a universally recognizable object (background anchor, e.g., a standard light switch, credit card, A4 paper, soda can).
2. State the known real-world dimensions of your anchor object if possible.

## AI Execution Pipeline:

### Step 1: Anchor Identification & Calibration
- Identify the designated background anchor object.
- Retrieve or confirm its exact standard manufacturing dimensions (Width_anchor, Height_anchor) in centimeters.
- Count the pixel dimensions of this anchor in the image to establish the baseline Wall Pixel-to-CM ratio:
  Ratio_wall = Actual_Size_cm / Pixel_Size

### Step 2: Monocular Depth & Perspective Scaling
- Evaluate the relative depth map of the scene. 
- Estimate the depth distance of the anchor (Z_anchor) and the target object (Z_target) from the camera lens.
- Calculate the perspective foreshortening scaling factor (α):
  α = Z_target / Z_anchor

### Step 3: Foreground Ratio Correction
- Adjust the pixel-to-cm ratio for the foreground plane to account for 1/Z perspective scaling:
  Ratio_target = Ratio_wall × α

### Step 4: Segmentation & Final Measurement
- Isolate the target object (and any requested sub-components like a screen or bezel).
- Measure its boundaries in pixels.
- Multiply target pixels by Ratio_target to output the final real-world width, length, and diagonal measurements in centimeters and millimeters.
