# 中文车牌检测与识别系统

停车场出入口、道路卡口抓拍、车辆管理等场景中，车牌号码的自动识别是最基础也最关键的一环。但国内车牌类型复杂——蓝牌、绿牌（新能源）、黄牌、双层牌照各不相同，加上光照变化、倾斜角度、遮挡等干扰，通用 OCR 的识别率往往不够用。本项目基于 YOLOv5 做车牌定位 + 自定义 CNN 做字符识别，针对中文车牌做了专项优化，在各类牌照和复杂场景下都能保持较高的识别准确率。

## 痛点与目的

- **问题**：通用 OCR 在车牌场景下存在识别率低的问题——中文字符、省份简称、军警牌照等特殊字符的准确率不足，且对倾斜、模糊、光照不均等情况鲁棒性差
- **方案**：分两步走——先用 YOLOv5 检测图片中的车牌位置并矫正，再用专门训练的字符识别模型逐字识别车牌号码
- **效果**：支持蓝牌、绿牌（新能源）、双层车牌等多种类型，提供图片/视频/摄像头三种检测模式

## 核心功能

- **车牌检测**：基于 YOLOv5 精确定位图像中的车牌区域
- **字符识别**：自定义 PlateNet 识别车牌字符（含中文省份简称）
- **双层车牌处理**：自动拆分合并双层车牌进行识别
- **多输入模式**：支持图片文件、视频文件和摄像头实时输入
- **多推理后端**：支持 PyTorch、ONNX、OpenVINO 多种推理方式
- **模型导出**：支持将模型导出为 ONNX、TorchScript 等格式
- **CCPD 数据集支持**：提供 CCPD 车牌数据集的预处理脚本

## 使用方法

### 安装依赖

```bash
pip install -r requirements.txt
```

### 车牌检测与识别

```bash
python detect_plate.py --source imgs/
```

### 使用摄像头实时识别

```bash
python detect_plate.py --source 0
```

### 训练车牌检测模型

```bash
python train.py --data data/plate.yaml --cfg models/yolov5s.yaml --weights weights/yolov5s.pt
```

## 项目结构

```
.
├── detect_plate.py               # 车牌检测+识别主程序
├── detect_demo.py                # 检测 Demo
├── train.py                      # YOLOv5 训练脚本
├── test.py                       # 测试脚本
├── export.py                     # 模型导出（ONNX/TorchScript）
├── onnx_infer.py                 # ONNX 推理
├── openvino_infer.py             # OpenVINO 推理
├── ccpd_process.py               # CCPD 数据集预处理
├── plate_recognition/            # 车牌字符识别模块
│   ├── plateNet.py               # 字符识别网络（PlateNet）
│   ├── plate_rec.py              # 识别推理逻辑
│   └── double_plate_split_merge.py  # 双层车牌拆分合并
├── models/                       # YOLOv5 网络结构定义
├── utils/                        # 工具函数
├── weights/                      # 模型权重
├── imgs/                         # 测试图片
├── fonts/                        # 中文字体文件
├── data/                         # 数据集配置
└── requirements.txt              # 依赖列表
```

## 适用场景

- 停车场出入口车牌自动识别
- 道路卡口抓拍与车辆管理
- 智慧交通与电子收费系统
- 小区/园区车辆出入管理
- 车牌 OCR 技术研究与学习

## 技术栈

| 组件 | 技术 |
|------|------|
| 车牌检测 | YOLOv5 |
| 字符识别 | PlateNet（自定义 CNN） |
| 深度学习 | PyTorch |
| 推理加速 | ONNX Runtime, OpenVINO |
| 图像处理 | OpenCV |

## License

MIT License
