# Vision Transformer (ViT) CIFAR-10 实验对比

## 实验方案超参数对比

本项目实现了三种不同的ViT训练配置，用于CIFAR-10图像分类任务的对比实验。

<table>
    <tr>
        <td><strong>参数</strong></td>
        <td><strong>Method 1</strong></td>
        <td><strong>Method 2</strong></td>
        <td><strong>Method 3</strong></td>
    </tr>
    <tr>
        <td>Epochs</td>
        <td>6</td>
        <td>6</td>
        <td>5</td>
    </tr>
    <tr>
        <td>Learning Rate</td>
        <td>2e-5</td>
        <td>2e-5</td>
        <td>2e-5</td>
    </tr>
    <tr>
        <td>Image Size</td>
        <td>224 x 224</td>
        <td>224 x 224</td>
        <td>224 x 224</td>
    </tr>
    <tr>
        <td>Batch Size</td>
        <td>10</td>
        <td>64</td>
        <td>16</td>
    </tr>
    <tr>
        <td>Weight Decay</td>
        <td>0.01</td>
        <td>0.00</td>
        <td>0.01</td>
    </tr>
    <tr>
        <td>Data Transforms (Training)</td>
        <td><code>RandomResizedCrop, RandomHorizontalFlip, Normalize(mean=0.5, std=0.5)</code></td>
        <td><code>RandomResizedCrop(scale=(0.08, 1)), RandomHorizontalFlip, RandAugment(2, 9), Normalize(mean=0.5, std=0.5)</code></td>
        <td><code>RandomResizedCrop, RandomHorizontalFlip, FeatureExtractor Normalization</code></td>
    </tr>
    <tr>
        <td>Data Transforms (Testing and Validation)</td>
        <td><code>Resize, CenterCrop, Normalize(mean=0.5, std=0.5)</code></td>
        <td><code>Resize, CenterCrop, Normalize(mean=0.5, std=0.5)</code></td>
        <td><code>Resize, CenterCrop, FeatureExtractor Normalization</code></td>
    </tr>
    <tr>
        <td>Mixed Precision</td>
        <td>No</td>
        <td>No</td>
        <td>Yes (FP16)</td>
    </tr>
    <tr>
        <td>Model</td>
        <td>ViT-Base-Patch16-224</td>
        <td>ViT-Base-Patch16-224</td>
        <td>ViT-Base-Patch16-224</td>
    </tr>
    <tr>
        <td>Training Time</td>
        <td>75min</td>
        <td>60min</td>
        <td>60min</td>
    </tr>
    <tr>
        <td>Test Accuracy</td>
        <td>98.79%</td>
        <td>98.51%</td>
        <td>98.72%</td>
    </tr>
    <tr>
        <td>Test F1-Score</td>
        <td>98.79%</td>
        <td>98.51%</td>
        <td>98.72%</td>
    </tr>
</table>


## 实验重点对比

### Method 1 - 基础配置
- **特点**: 小批次训练，基础数据增强
- **优势**: 训练稳定，参数调优简单
- **适用**: 资源有限环境，快速验证

### Method 2 - 高级增强配置
- **特点**: 大批次训练 + RandAugment自动数据增强
- **优势**: 更强的泛化能力，先进的数据增强策略
- **适用**: 追求更高性能，有充足计算资源

### 当前基础配置
- **特点**: 中等批次 + 混合精度训练
- **优势**: 平衡性能与资源消耗，现代化训练技术
- **适用**: 标准实验环境，可重现结果

## 关键技术差异

### 数据增强策略
- **Method 1**: 传统增强（随机裁剪 + 水平翻转）
- **Method 2**: 先进增强（RandAugment自动策略）
- **当前配置**: HuggingFace标准预处理

### 训练优化
- **批次大小**: 10 → 64 → 16 (平衡内存与性能)
- **正则化**: 有权重衰减 → 无权重衰减 → 有权重衰减
- **精度**: FP32 → FP32 → FP16 (混合精度)

### 预期性能排序
基于超参数配置分析，预期性能排序为：
1. **Method 2**: 大批次 + 高级数据增强
2. **Method 1**: 稳定的基础配置  
3. **当前配置**: 中等配置但有混合精度优化

## 实验文件结构
