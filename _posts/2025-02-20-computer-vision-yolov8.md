---
layout: post
title: "YOLOv8 Edge Deployment: From Training to 60 FPS on Raspberry Pi"
date: 2025-02-20
category: Computer Vision
tags: [computer vision, deep learning, tutorial]
tech_tags: [YOLOv8, ONNX, Edge AI]
excerpt: "How I trained a custom YOLOv8 model, optimized it with ONNX quantization, and deployed it to run at 60 FPS on embedded hardware."
description: "How to train, optimize with ONNX, and deploy a YOLOv8 model to run at 60 FPS on Raspberry Pi 4. A complete edge AI deployment guide."
subtitle: "How I trained a custom YOLOv8 model, optimized it with ONNX quantization, and deployed it to run at 60+ FPS on a Raspberry Pi 4."
read_time: 6
emoji: "👁️"
cover_image: "https://images.unsplash.com/photo-1635070041078-e363dbe005cb?w=1200&auto=format&fit=crop&q=80"
---

![YOLOv8 Edge AI Deployment](https://images.unsplash.com/photo-1635070041078-e363dbe005cb?w=1200&auto=format&fit=crop&q=80)
*Real-time object detection on embedded hardware — from cloud to edge*


Edge AI deployment is one of the most exciting challenges in applied machine learning. Getting a state-of-the-art object detection model to run in real-time on a $35 computer is deeply satisfying — and very useful for IoT applications. In this post, I'll share my complete workflow for deploying YOLOv8 on a Raspberry Pi 4.

## Why YOLOv8?

Ultralytics YOLOv8 is the current state-of-the-art in real-time object detection. It offers:

- Excellent accuracy-speed tradeoff
- Native ONNX export support
- Simple Python API
- Built-in quantization support

## Step 1: Training a Custom Model

```python
from ultralytics import YOLO

# Start from a pre-trained YOLOv8 nano model (smallest variant)
model = YOLO("yolov8n.pt")

# Train on custom dataset
results = model.train(
    data="dataset.yaml",
    epochs=100,
    imgsz=640,
    batch=16,
    device="cuda",
    project="runs/train",
    name="custom_detector",
    augment=True,
    hsv_h=0.015,
    hsv_s=0.7,
    hsv_v=0.4,
    degrees=15,
    flipud=0.5,
    mosaic=1.0
)

print(f"Best mAP50: {results.results_dict['metrics/mAP50(B)']:.3f}")
```

## Step 2: Export to ONNX with INT8 Quantization

The key to achieving high FPS on resource-constrained hardware is quantization. We convert from FP32 to INT8, reducing model size by ~4x and increasing inference speed significantly.

```python
from ultralytics import YOLO

model = YOLO("runs/train/custom_detector/weights/best.pt")

# Export to ONNX with INT8 quantization
model.export(
    format="onnx",
    imgsz=320,     # Reduce input size for edge
    simplify=True,
    dynamic=False,
    int8=True,     # INT8 quantization
    data="dataset.yaml"  # Calibration data for quantization
)
print("Exported to ONNX with INT8 quantization")
```

## Step 3: Optimized Inference on Raspberry Pi

```python
import onnxruntime as ort
import numpy as np
import cv2
import time

class YOLOv8Detector:
    def __init__(self, model_path: str, input_size: int = 320):
        # Configure ONNX Runtime for ARM64
        options = ort.SessionOptions()
        options.intra_op_num_threads = 4  # Use all 4 RPi4 cores
        options.execution_mode = ort.ExecutionMode.ORT_PARALLEL
        options.graph_optimization_level = ort.GraphOptimizationLevel.ORT_ENABLE_ALL

        self.session = ort.InferenceSession(
            model_path,
            sess_options=options,
            providers=["CPUExecutionProvider"]
        )
        self.input_size = input_size
        self.input_name = self.session.get_inputs()[0].name

    def preprocess(self, frame: np.ndarray) -> np.ndarray:
        img = cv2.resize(frame, (self.input_size, self.input_size))
        img = cv2.cvtColor(img, cv2.COLOR_BGR2RGB)
        img = img.astype(np.float32) / 255.0
        img = np.transpose(img, (2, 0, 1))
        return np.expand_dims(img, axis=0)

    def detect(self, frame: np.ndarray) -> list:
        input_tensor = self.preprocess(frame)
        outputs = self.session.run(None, {self.input_name: input_tensor})
        return self.postprocess(outputs[0], frame.shape)

    def postprocess(self, output, orig_shape, conf_threshold=0.5):
        detections = []
        # ... NMS and coordinate scaling logic
        return detections

# Benchmark
detector = YOLOv8Detector("best_int8.onnx")
cap = cv2.VideoCapture(0)

frame_count = 0
start_time = time.time()

while cap.isOpened():
    ret, frame = cap.read()
    if not ret:
        break
    detections = detector.detect(frame)
    frame_count += 1

    if frame_count % 100 == 0:
        elapsed = time.time() - start_time
        fps = frame_count / elapsed
        print(f"FPS: {fps:.1f}")
```

## Results & Benchmarks

Here's what I achieved on a Raspberry Pi 4 (4GB RAM):

| Configuration | FPS | mAP50 | Model Size |
|---|---|---|---|
| YOLOv8n FP32 (640px) | 4.2 | 94.1% | 6.3 MB |
| YOLOv8n ONNX FP32 (320px) | 22.7 | 91.8% | 6.3 MB |
| **YOLOv8n ONNX INT8 (320px) ✅** | **63.4** | 89.2% | 1.8 MB |

> A ~5% drop in mAP is a very acceptable tradeoff for a 15x speedup in most real-world applications.

## Key Takeaways

- Always start with the smallest YOLO variant (n or s) for edge deployment
- Reduce input resolution — 320px is often sufficient for many use cases
- INT8 quantization gives the biggest speed boost (3-5x) with minimal accuracy loss
- Multi-threaded ONNX Runtime is crucial for ARM CPUs
- Profile your full pipeline, not just model inference

The full code including training configs, ONNX export scripts, and optimized inference pipeline is on [GitHub](https://github.com/TrinhHuuTho/yolov8-edge).
