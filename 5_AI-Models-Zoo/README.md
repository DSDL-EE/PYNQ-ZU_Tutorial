# Examine pre-trained deep learning models on PYNQ-ZU, using Vitis-AI and DPU-PYNQ

## 1. Vitis-AI - Tools on PC
https://github.com/Xilinx/Vitis-AI/tree/v2.5

## 2. DPU-PYNQ - Tools on PYNQ-ZU
https://github.com/Xilinx/DPU-PYNQ/tree/dev_3.0.0

On PYNQ-ZU, run:
```
git clone -b dev_3.0.0 https://github.com/Xilinx/DPU-PYNQ
pip3 install ./DPU-PYNQ --no-build-isolation
```

## 3. [Tutorial] Make a pre-trained model run on DPU
- Example notebook: [dpu_enet_cityscapes.ipynb](https://github.com/Xilinx/DPU-PYNQ/blob/dev_3.0.0/pynq_dpu/notebooks/dpu_enet_cityscapes.ipynb)
- Tutorial: [20230420_DSDL_Segmentation_ENet_PYNQ.pdf](20230420_DSDL_Segmentation_ENet_PYNQ.pdf)

## Reference
### 1. Vitis-AI - Frequently Asked Questions
https://xilinx.github.io/Vitis-AI/docs/reference/faq.html

### 2. Vitis AI v2.5 Document
https://docs.xilinx.com/r/2.5-English/ug1414-vitis-ai/Vitis-AI-Overview

### 3. IP and Tool Version Compatibility
https://xilinx.github.io/Vitis-AI/docs/reference/version_compatibility.html