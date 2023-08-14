# Examine pre-trained deep learning models on PYNQ-ZU, using Vitis-AI and DPU-PYNQ

## 1. Vitis-AI - Tools on PC
https://github.com/Xilinx/Vitis-AI/tree/2.5

It is suggested that user installs Ubuntu on a PC with NVIDIA graphics card. Otherwise, user can use CPU version, which is slower.

On PC, run:
```
git clone -b 2.5 https://github.com/Xilinx/Vitis-AI
cd Vitis-AI
```

User follows this [instruction](https://github.com/Xilinx/Vitis-AI/tree/2.5#installation) to install and build vitis-ai docker image.

## 2. DPU-PYNQ - Tools on PYNQ-ZU
https://github.com/Xilinx/DPU-PYNQ/tree/dev_3.0.0

There are 2 ways to install DPU-PYNQ on PYNQ-ZU: (1) via PyPI and (2) from source.

(1) On PYNQ-ZU, run:
```
source /etc/profile.d/pynq_venv.sh
pip3 install pynq-dpu
cd $PYNQ_JUPYTER_NOTEBOOKS
pynq get-notebooks pynq-dpu -p . # Download examples
```

(2) On PYNQ-ZU, run:
```
git clone -b dev_3.0.0 https://github.com/Xilinx/DPU-PYNQ
source /etc/profile.d/pynq_venv.sh
pip3 install ./DPU-PYNQ --no-build-isolation
cd $PYNQ_JUPYTER_NOTEBOOKS
pynq get-notebooks pynq-dpu -p . # Download examples
```

## 3. [Tutorial] Make a pre-trained model run on DPU
### 3.1. Object Detection: YOLOv3 on VOC
- Example notebook: [dpu_yolov3.ipynb](https://github.com/Xilinx/DPU-PYNQ/blob/master/pynq_dpu/notebooks/dpu_yolov3.ipynb)
- Tutorial: [Object Detection with YOLOv3](DPU-PYNQ_YOLOv3.md)
### 3.2. Object Detection: YOLOv3 and webcam
- Example notebook: [dpu_yolov3_webcam.ipynb](dpu_yolov3_webcam.ipynb)
### 3.3. Object Detection: YOLOv3 on GStreamer video stream
- Example notebook: [dpu_yolov3_gstreamer.ipynb](dpu_yolov3_gstreamer.ipynb)
### 3.4. Segmentation: ENet on Cityscapes
- Example notebook: [dpu_enet_cityscapes.ipynb](https://github.com/Xilinx/DPU-PYNQ/blob/dev_3.0.0/pynq_dpu/notebooks/dpu_enet_cityscapes.ipynb)
- Tutorial: [20230420_DSDL_Segmentation_ENet_PYNQ.pdf](20230420_DSDL_Segmentation_ENet_PYNQ.pdf)

## Reference
### 1. Vitis-AI - Frequently Asked Questions
https://xilinx.github.io/Vitis-AI/docs/reference/faq.html

### 2. Vitis AI v2.5 Document
https://docs.xilinx.com/r/2.5-English/ug1414-vitis-ai/Vitis-AI-Overview

### 3. IP and Tool Version Compatibility
https://xilinx.github.io/Vitis-AI/docs/reference/version_compatibility.html
