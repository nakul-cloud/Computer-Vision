# 📄 Pattern Substitution Risk Detection 🛡️

This notebook demonstrates **document image analysis** tasks related to **compression risks**, **pattern substitution detection**, and **safe compression rules** for scanned documents. Built with **OpenCV**, **scikit-image**, **NumPy**, and **Matplotlib**. 🔍

## 🎯 Project Objective

The main goal is to analyze how **compression artifacts** affect document images and downstream tasks:

- 🔍 **Detect similar symbols** that could be substituted/mixed up
- 📉 **Quantify compression degradation** (PSNR/SSIM + visual edges)
- 🕵️ **Spot silent data corruption** between lossless/lossy scans
- 🧮 **Test downstream task failure** (simple digit classification)
- ⚖️ **Design safe compression rules** based on image complexity

This simulates **production document processing pipelines** where compression can silently break OCR/ML accuracy.

## 📋 Input Images / Data

The notebook uses **5 key images**:

| Image | Purpose | Characteristics |
|-------|---------|----------------|
| `document.jpg` | Symbol grouping | Binary document with text/symbols |
| `text.png` | JPEG compression test | Clean grayscale text |
| `scan_lossless.jpeg` | Corruption detection | High-quality scan reference |
| `scan_lossy_com.png` | Corruption detection | Compressed scan (suspect) |
| `digits_{q}.jpg` | Downstream task test | Compressed digit images (q=90,50,20) |

**Note**: Images should contain **scanned documents**, **forms**, or **text-heavy content** for meaningful results.

## 🔄 Pipeline Overview

**5 analysis stages**:

1. 🔗 **Pattern substitution risk** - Group similar symbols by area  
2. 📊 **Compression quality metrics** - PSNR/SSIM + edge preservation
3. 🕵️ **Silent corruption detection** - Lossless vs lossy scan comparison
4. 🚫 **Downstream task breakage** - Simple digit classifier accuracy drop
5. ⚖️ **Safe compression rules** - Entropy + edge density decision logic

Each stage reveals different **failure modes** of document compression.

---

## 1. 🔗 Pattern Substitution Risk

**Problem**: Similar-looking characters (1/l, O/0, rn/m) can be confused after compression.

**What it does**:
- Loads binary document → **connected component labeling**
- Extracts **area + bounding box** for each symbol/character
- **Groups similar symbols** using area-based similarity (10% threshold)
- Reports number of unique symbol classes found

**Why this matters**  
✅ Reveals **potential character confusion classes** before OCR  
✅ Compression can make similar symbols **indistinguishable**  
✅ **16 symbol groups** detected in example (reasonable for document)

---

## 2. 📊 Human-Visible vs Machine-Relevant Differences

**Problem**: Compression looks "good to human eye" but breaks machine vision.

**What it does**:
- Compresses text image at **JPEG qualities**: 90, 50, 20
- Computes **PSNR** (pixel noise) + **SSIM** (structural similarity)
- **Visualizes edge maps** (Canny) to show **machine perception**

**Key Results**:
- Lossless: (956, 768)
- Lossy: (956, 768) [after resize]
- Diff map: Binary regions showing changes


**Why this matters**  
🔍 **Pinpoints exact corruption locations**  
⚠️ **Even "high-quality" compression** can alter critical text pixels  
✅ **20px threshold** catches meaningful changes, ignores noise

---

## 4. 🚫 Compression Breaking Downstream Tasks

**Problem**: Compression destroys simple ML rules.

**What it does**:
- Tests **rule-based digit classifier**: `"1" if h > 2*w else "0"`
- Runs on **compressed digit images** (qualities 90,50,20)
- Measures **accuracy drop** as compression worsens

**Result**: `Accuracy: 0.0` ❌

**Why this matters**  
💥 **Simple rules fail completely** on compressed input  
💥 **Real OCR/ML models** would perform even worse  
✅ Demonstrates **production failure mode** - compression breaks pipeline

---

## 5. ⚖️ Designing Safe Compression Rules

**Problem**: Need **automatic compression decisions** per image.

**What it does**:
- Computes **entropy** (image complexity)
- Computes **edge density** (text/detail richness)
- **Decision tree**:
- Edge density > 15% → Lossless
- Entropy < 4 bits → Lossy OK
- Else → High-quality lossy

