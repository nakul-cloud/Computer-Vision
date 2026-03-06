# 🎥 YOLOv8 Real-Time Object Detection Pipeline 🚀

Complete **production-ready YOLOv8 video detection**! Downloads YouTube Shorts, runs inference, saves annotated video. **Zero setup** - auto-installs everything!

## 🎯 Project Objective

**End-to-end video detection**:

- 📥 Auto-download YouTube Shorts
- 🧠 YOLOv8 inference (custom/pretrained)
- 🎬 Annotated video output
- 📊 FPS + stats tracking

**Demo**: YouTube → **587 frames** → annotated MP4

## 🖼️ Input / Output

| Step | File |
|------|------|
| Input | YouTube URL |
| Download | `video.mp4` |
| Model | `best.pt` / `yolov8n.pt` |
| **Output** | **`shorts.mp4`** + HTML |

## 🔄 Pipeline Overview

#### 1. 🔧 **Auto-install** tensorflow + ultralytics + yt-dlp
#### 2. 📥 **Download** YouTube Short (yt-dlp)
#### 3. 🧠 **Load YOLOv8** (custom or pretrained)
#### 4. 📹 **Process video** frame-by-frame
#### 5. ✏️ **Draw boxes** + labels
#### 6. 💾 **Save annotated MP4**

## 1. 🔧 Auto-Setup

!pip install tensorflow ultralytics yt-dlp
✓ tensorflow 2.20.0 | ultralytics 8.4.10

 

## 2. 📥 YouTube Download

yt-dlp https://youtube/shorts/...
✓ video.mp4 (360×640, 30 FPS, 587 frames)

 

## 3. 🧠 YOLOv8 Loading

- model = YOLO('best.pt') # Custom

OR
- model = YOLO('yolov8n.pt') # Pretrained nano



## 4. 📹 Video Processing

for frame in video:
results = model(frame, conf=0.25)
annotated = results.plot()
out.write(annotated)



**Progress**: `587/587 frames ✓`

## 5. ✏️ Annotation

- **Bounding boxes** + confidence
- **Class labels** color-coded
- **Same FPS/resolution** preserved

## 6. 📊 Stats

- Resolution: 360×640 | FPS: 30 | Frames: 587
- Output: shorts.mp4 ✓



## 🎓 Key Features

✅ **Zero setup** - auto everything  
✅ **Production pipeline**  
✅ **Custom + pretrained** models  
✅ **Real-time inference**  
✅ **HTML preview**  

## 🚀 Extensions

- **Batch processing**
- **Webcam live**  
- **Object tracking**
- **Streamlit app**
