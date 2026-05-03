# Part 2 - B30: Generate an AI-created image, applying an imperceptible watermark on it and then perform an image-to-image regeneration or editing process to make sure the watermark is detectable---the watermark survives

### Task Completion
**1. Generate an AI Image**  
Using Grok AI for image generation  
<img width="1168" height="784" alt="3exqf" src="https://github.com/user-attachments/assets/fd687a91-c943-42fe-aaa4-fd1f4e38ff13" />

**2. Create Code for Embedding Watermark, Applying Transformations, and Verifying if watermark survives.**
```python

import cv2
import numpy as np
from PIL import Image
import os

# ---------- Configuration ----------
INPUT_IMG  = "/mnt/user-data/uploads/3exqf.jpg"
OUT_DIR    = "/home/claude/watermark"
BLOCK      = 8                          # 8x8 DCT blocks (JPEG-style)
ALPHA      = 25.0                       # embedding strength
REDUNDANCY = 3                          # embed each bit in N blocks, majority vote
# Watermark payload: a 64-bit signature
WATERMARK_BITS = np.array(
    [1,0,1,1,0,0,1,0, 1,1,0,0,1,0,1,1,
     0,1,1,0,1,0,0,1, 1,0,0,1,0,1,1,0,
     1,1,1,0,0,1,0,1, 0,0,1,1,0,1,1,0,
     1,0,1,0,1,1,0,0, 0,1,1,0,1,0,1,1], dtype=np.int32
)
# Low-frequency coefficient pair (survives JPEG quantization at q=60)
COEF_A = (1, 2)
COEF_B = (2, 1)


def embed_watermark(img_bgr, bits, alpha=ALPHA, redundancy=REDUNDANCY):
    """Embed bits in the Y (luminance) channel using DCT coefficient swapping.
    Each bit is embedded in `redundancy` consecutive blocks for robustness."""
    ycrcb = cv2.cvtColor(img_bgr, cv2.COLOR_BGR2YCrCb).astype(np.float32)
    Y = ycrcb[:, :, 0]
    h, w = Y.shape
    bh, bw = h // BLOCK, w // BLOCK

    # Expand bits with redundancy
    expanded = np.repeat(bits, redundancy)
    n = len(expanded)
    if n > bh * bw:
        raise ValueError("Image too small for payload + redundancy")

    idx = 0
    for by in range(bh):
        for bx in range(bw):
            if idx >= n: break
            y, x = by * BLOCK, bx * BLOCK
            block = Y[y:y+BLOCK, x:x+BLOCK]
            dct = cv2.dct(block)

            a, b = dct[COEF_A], dct[COEF_B]
            bit = expanded[idx]
            if bit == 1:
                if a <= b + alpha:
                    mid = (a + b) / 2
                    dct[COEF_A] = mid + alpha / 2
                    dct[COEF_B] = mid - alpha / 2
            else:
                if b <= a + alpha:
                    mid = (a + b) / 2
                    dct[COEF_A] = mid - alpha / 2
                    dct[COEF_B] = mid + alpha / 2

            Y[y:y+BLOCK, x:x+BLOCK] = cv2.idct(dct)
            idx += 1
        if idx >= n: break

    ycrcb[:, :, 0] = np.clip(Y, 0, 255)
    out = cv2.cvtColor(ycrcb.astype(np.uint8), cv2.COLOR_YCrCb2BGR)
    return out


def extract_watermark(img_bgr, n_bits=len(WATERMARK_BITS), redundancy=REDUNDANCY):
    """Extract bits via majority vote across redundant blocks."""
    ycrcb = cv2.cvtColor(img_bgr, cv2.COLOR_BGR2YCrCb).astype(np.float32)
    Y = ycrcb[:, :, 0]
    h, w = Y.shape
    bh, bw = h // BLOCK, w // BLOCK

    raw = []
    n_total = n_bits * redundancy
    for by in range(bh):
        for bx in range(bw):
            if len(raw) >= n_total: break
            y, x = by * BLOCK, bx * BLOCK
            block = Y[y:y+BLOCK, x:x+BLOCK]
            dct = cv2.dct(block)
            raw.append(1 if dct[COEF_A] > dct[COEF_B] else 0)
        if len(raw) >= n_total: break

    raw = np.array(raw[:n_total]).reshape(n_bits, redundancy)
    # Majority vote per bit
    bits = (raw.sum(axis=1) > redundancy // 2).astype(np.int32)
    return bits


def detection_score(extracted, original=WATERMARK_BITS):
    """Return % of bits matching. >= ~70% indicates watermark detected."""
    matches = np.sum(extracted == original)
    return 100.0 * matches / len(original)


def psnr(a, b):
    mse = np.mean((a.astype(np.float32) - b.astype(np.float32)) ** 2)
    if mse == 0: return float('inf')
    return 20 * np.log10(255.0 / np.sqrt(mse))


# ============ STEP 1: Embed watermark ============
print("="*60)
print("STEP 1: EMBEDDING IMPERCEPTIBLE WATERMARK")
print("="*60)
original = cv2.imread(INPUT_IMG)
# Crop to multiple of BLOCK
h, w = original.shape[:2]
h, w = (h // BLOCK) * BLOCK, (w // BLOCK) * BLOCK
original = original[:h, :w]
print(f"Image size:        {w} x {h}")
print(f"Watermark payload: {len(WATERMARK_BITS)} bits")

watermarked = embed_watermark(original, WATERMARK_BITS)
cv2.imwrite(f"{OUT_DIR}/01_watermarked.png", watermarked)

# Verify imperceptibility
psnr_val = psnr(original, watermarked)
print(f"PSNR (vs original): {psnr_val:.2f} dB  (>40 dB = imperceptible)")

# Verify extraction works on clean watermarked image
extracted_clean = extract_watermark(watermarked)
score_clean = detection_score(extracted_clean)
print(f"Detection on clean watermarked: {score_clean:.1f}% bit match")

# ============ STEP 2: Apply 3 transformations ============
print()
print("="*60)
print("STEP 2: APPLYING 3 TRANSFORMATIONS")
print("="*60)

results = []

# --- Transformation 1: JPEG re-compression (quality 60) ---
# Simulates the lossy compression every image-to-image pipeline applies.
print("\n[1/3] JPEG recompression (quality=60)")
cv2.imwrite(f"{OUT_DIR}/02a_transform1_jpeg.jpg", watermarked,
            [cv2.IMWRITE_JPEG_QUALITY, 60])
t1 = cv2.imread(f"{OUT_DIR}/02a_transform1_jpeg.jpg")
ext1 = extract_watermark(t1)
s1 = detection_score(ext1)
results.append(("JPEG q=60 recompression", s1, psnr(watermarked, t1)))
print(f"        Detection: {s1:.1f}% bit match")

# --- Transformation 2: Gaussian blur + brightness/contrast adjustment ---
# Mimics image-to-image style transfer / neural smoothing artefacts.
print("\n[2/3] Gaussian blur + brightness/contrast shift")
t2 = cv2.GaussianBlur(watermarked, (3, 3), 0.6)
t2 = cv2.convertScaleAbs(t2, alpha=1.10, beta=8)   # +10% contrast, +8 brightness
cv2.imwrite(f"{OUT_DIR}/02b_transform2_blur_color.png", t2)
ext2 = extract_watermark(t2)
s2 = detection_score(ext2)
results.append(("Blur + brightness/contrast", s2, psnr(watermarked, t2)))
print(f"        Detection: {s2:.1f}% bit match")

# --- Transformation 3: Resize down then back up (image-to-image regen sim) ---
# Mimics what happens inside a diffusion img2img encode/decode cycle.
print("\n[3/3] Downscale 70% -> upscale + Gaussian noise (img2img regen sim)")
small = cv2.resize(watermarked, (int(w*0.7), int(h*0.7)),
                   interpolation=cv2.INTER_AREA)
t3 = cv2.resize(small, (w, h), interpolation=cv2.INTER_CUBIC)
noise = np.random.normal(0, 2.5, t3.shape).astype(np.float32)
t3 = np.clip(t3.astype(np.float32) + noise, 0, 255).astype(np.uint8)
cv2.imwrite(f"{OUT_DIR}/02c_transform3_resize_noise.png", t3)
ext3 = extract_watermark(t3)
s3 = detection_score(ext3)
results.append(("Resize cycle + noise (img2img sim)", s3, psnr(watermarked, t3)))
print(f"        Detection: {s3:.1f}% bit match")

# ============ STEP 3: Summary ============
print()
print("="*60)
print("STEP 3: ROBUSTNESS SUMMARY")
print("="*60)
print(f"{'Transformation':<40} {'Detect %':>10} {'PSNR dB':>10}")
print("-"*60)
print(f"{'(baseline: clean watermarked)':<40} {score_clean:>9.1f}% {psnr_val:>9.2f}")
for name, score, p in results:
    survived = "SURVIVED" if score >= 70 else "lost"
    print(f"{name:<40} {score:>9.1f}% {p:>9.2f}   {survived}")
print()
print("Threshold for 'detected': >=70% bit-match (random = 50%).")
print("All output files written to /home/claude/watermark/")

# Save a small report
with open(f"{OUT_DIR}/report.txt", "w") as f:
    f.write("B30 Watermark Robustness Test\n")
    f.write("="*40 + "\n\n")
    f.write(f"Method: Block DCT (8x8) mid-frequency coefficient swap\n")
    f.write(f"Channel: Blue\n")
    f.write(f"Coefficients used: {COEF_A} <-> {COEF_B}, alpha={ALPHA}\n")
    f.write(f"Payload: 64-bit signature\n")
    f.write(f"PSNR vs original: {psnr_val:.2f} dB (imperceptible)\n\n")
    f.write(f"Baseline detection (no transform): {score_clean:.1f}%\n\n")
    f.write("Transformations applied:\n")
    for name, score, p in results:
        verdict = "SURVIVED" if score >= 70 else "lost"
        f.write(f"  - {name}: {score:.1f}% match -> {verdict}\n")
    f.write("\nDetection threshold: >=70% bit match (random baseline = 50%).\n")
print("\nReport saved to report.txt")
```
### Outputs: 
**Image with watermark embedded:**  
<img width="1168" height="784" alt="01_watermarked" src="https://github.com/user-attachments/assets/3255a248-356a-42c9-95ae-5fde6b390a82" />

**Transformation 1: JPEG re-compression (quality 60)**  
- Saves the image as a lower quality jpeg
<img width="1168" height="784" alt="02a_transform1_jpeg" src="https://github.com/user-attachments/assets/cab9e2e2-f139-42ec-88e0-74de3eb247cd" />

**Transformation 2: Blur and Brightness/Contrast Shift**  
<img width="1168" height="784" alt="02b_transform2_blur_color" src="https://github.com/user-attachments/assets/b7524104-d4a1-4047-8b24-d4a2fdb037e3" />

**Transformation 3: Resize down, then back up and adding noise**  
- Simulates an image-to-image regeneration
<img width="1168" height="784" alt="02c_transform3_resize_noise" src="https://github.com/user-attachments/assets/e1866b99-fee3-474d-ae5c-0477b8f99882" />

**Watermark Survivability Report Contents**
```
B30 Watermark Robustness Test
========================================

Method: Block DCT (8x8) mid-frequency coefficient swap
Channel: Blue
Coefficients used: (1, 2) <-> (2, 1), alpha=25.0
Payload: 64-bit signature
PSNR vs original: 58.21 dB (imperceptible)

Baseline detection (no transform): 100.0%

Transformations applied:
  - JPEG q=60 recompression: 100.0% match -> SURVIVED
  - Blur + brightness/contrast: 100.0% match -> SURVIVED
  - Resize cycle + noise (img2img sim): 100.0% match -> SURVIVED

Detection threshold: >=70% bit match (random baseline = 50%).
```

