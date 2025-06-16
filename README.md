# Object Detection Comparision between YOLOv3 and Faster RCNN

This project compares two object detection models YOLOv3 and Faster R-CNN on the PSU Car Dataset containing aerial images. The goal is to evaluate accuracy, speed, and robustness in detecting cars under challenging visual conditions.

## Motivation.

Detecting vehicles from aerial images is crucial for smart cities, surveillance, and traffic analysis. However, detecting small, occluded, or densely packed cars remains difficult. We explore which architecture offers better trade-offs between performance and speed.

## Dataset Discription.

We used the PSU Car Dataset, consisting of 218 training images (3,364 annotated cars) and 52 test images (738 annotated cars). The dataset features aerial imagery with challenges including small object
sizes, occlusion, and scale variations

## Methodology 

Faster R-CNN Models.
We implemented two versions of Faster R-CNN using PyTorch's TorchVision library:

(i) FRCNN-ResNet50: With a ResNet-50 backbone and Feature Pyramid Network (FPN).

(ii) FRCNN-MobileNetV3: A lightweight alternative using MobileNetV3-Large.

Faster R-CNN is a two-stage detector. The first stage proposes regions via a Region Proposal Network (RPN), and the second stage classifies and refines those proposals. For training, we used cross-entropy loss for classification and Smooth L1 loss for bounding box regression, which is robust to outliers and maintains good localization precision. We trained both models using Stochastic Gradient Descent (SGD) with a momentum of 0.9 for 10 epochs.

YOLO Models
We trained two single-stage models using the Ultralytics PyTorch implementation:

(i) YOLOv3 with a Darknet-53 backbone.

(ii) YOLOv8n (nano version) with a modern lightweight backbone.

YOLO models predict bounding boxes and class scores directly from feature maps at multiple scales. The YOLOv3 loss function combines mean squared error for box regression, binary cross-entropy for objectness, and cross-entropy for classification. YOLOv8n was fine-tuned using pretrained weights (yolov8n.pt) adapted for single-class detection. Training was performed for 10 epochs using Ultralytics' default training loop.

##  Experiments & Results
All four models Faster R-CNN (ResNet-50 & MobileNetV3), YOLOv3, and YOLOv8n were trained for 10 epochs on the PSU Car Dataset using Google Colab with an NVIDIA T4 GPU. Faster R-CNN (ResNet-50) achieved the highest accuracy with a mAP@[0.5:0.95] of 0.688 and AP@0.5 of 0.967, performing best in crowded and occluded scenes. YOLOv3 reached a mAP of 0.653, offering a strong F1-score of 0.651 and faster inference at 12.7 FPS, making it ideal for real-time tasks.

YOLOv8n showed the highest precision (0.786) and fastest real-time performance among YOLO models (14.1 FPS), though it produced more false positives. FRCNN-MobileNetV3 was the fastest overall (38.9 FPS) but had the lowest mAP (0.442), making it suitable only for speed-critical applications. Overall, YOLOv3 balanced accuracy and speed best, while ResNet-50 remained the most reliable for complex detection scenarios.

##  Qualitative Results

We selected five aerial scenes to evaluate the models under real-world challenges: occlusions, dense parking, false positives, and background confusion.
In scenes like crowded intersections and dense lots, FRCNN-ResNet50 consistently identified the highest number of vehicles with fewer false positives.
YOLOv3 and YOLOv8n missed several small or partially visible vehicles but maintained strong real-time responsiveness.
Tree shadows and complex textures tricked YOLOv8n more often, confirming its aggressive detection style.
These tests highlight the balance between robustness (FRCNN) and speed (YOLO) across different use cases.
