---
{"aliases":[],"tags":[],"title":"AI学习","date":"2024-09-30T16:32:54+08:00","date_modify":"2025-06-28T23:25:54+08:00","dg-publish":true,"permalink":"/400_Archives/AI学习/","dgPassFrontmatter":true,"created":"2024-09-30T16:32:54+08:00","updated":"2025-06-28T23:25:54+08:00"}
---

<https://modelscope.cn/my/mynotebook/preset>
[[__Publish__/01_技术/技术卡片笔记\|技术卡片笔记]]

# 1. 概念

## 1.1.1. **框架**

- **PyTorch**: 动态计算图，易于调试和开发，广泛用于学术研究和快速原型。
- **TensorFlow**: 静态计算图（2.x 版本支持动态计算图），强大的生产部署能力，广泛应用于工业界。

## 1.1.2. **Jupyter**

- JupyterLab 是一个基于 Web 的交互式开发环境，广泛用于数据科学、机器学习、科学计算和教育等领域。它是 Jupyter 项目的一部分，提供了一个强大且灵活的平台，用于创建和共享文档，这些文档可以包含代码、文本、可视化和交互式控件。
- `.ipynb` 是 Jupyter Notebook 文件的扩展名，代表 "Interactive Python Notebook"。这种文件格式是 Jupyter 项目的一部分，用于存储和共享包含代码、文本、可视化和其他内容的交互式文档。
- `.ipynb` 文件实际上是一个 JSON 格式的文本文件

## 1.1.3. 设备

## 1.1.4. CPU

- **中央处理器 (Central Processing Unit)**: 计算机的核心处理单元，负责执行大部分计算任务。CPU 通常具有较少的核心，但每个核心的性能较高，适用于串行计算任务。
- **使用场景**: 适合执行一般的编程任务、数据预处理、模型推理等不需要大量并行计算的任务。

## 1.1.5. GPU

- **图形处理单元 (Graphics Processing Unit)**: 专门用于处理图形计算任务的处理器。GPU 具有大量的核心，适合大规模并行计算。

## 1.1.6. CUDA

- **Compute Unified Device Architecture**: 由 NVIDIA 开发的一种并行计算平台和编程模型，允许开发者在 NVIDIA GPU 上执行计算任务。
- **使用场景**: 深度学习框架（如 TensorFlow、PyTorch）通常使用 CUDA 来加速计算任务。需要安装 NVIDIA 的 CUDA Toolkit 和相应的驱动程序。

## 1.1.7. GPU:X 和 CUDA:X

- **GPU:X** 和 **CUDA:X**: 这些表示在具有多个 GPU 的系统中，选择特定的 GPU 设备进行计算。
    - **GPU:X**: 通常用于指定第 X 个 GPU，X 从 0 开始。例如，GPU:0 表示第一个 GPU，GPU:1 表示第二个 GPU。
    - **CUDA:X**: 类似于 GPU:X，指定第 X 个 GPU 设备进行 CUDA 计算。
