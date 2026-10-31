---
cssclasses:
  - full-width
---

# Models

## YOLO
https://codeberg.org/CCodeRun/darknet
## RT-DETER
DCAM-DETR?

## SSD

# Overview

|              | Architecture             | Purpose                                                   | Training sped | Inference speed | Notes |
| ------------ | ------------------------ | --------------------------------------------------------- | ------------- | --------------- | ----- |
| YOLO (v8/11) | CNN/Single Stage         | Main model; often used in papers                          | Fast          | Real time       |       |
| RT-DETER     | Transformer (End-To-End) | Transformer reference; YOLO speed + Transformer precision | Slow          | Real time       |       |
| Faster R-CNN | CNN/Two Stage            | Two vs Single stage                                       | Medium        | Ok              |       |
