# California Wildfire Eaton Fire

[English README](./README.md)

这个仓库保存的是 Eaton Fire 项目的代码、文档和目录骨架，用来支持一个可复现的灾后图像分析流程。真正的大体量数据，例如原始街景图片、遥感瓦片和大部分生成结果，默认都保留在本地，不直接上传到 GitHub。

这个仓库的原则是：

- GitHub 里保留代码和文档
- 原始数据与大文件留在本地
- 通过保留目录骨架，方便在另一台机器上复现同样的流程

## 最终数据集

### 1. `Eaton_Fire/`

这是项目中的权威标注数据集，也是后续监督学习和统计分析的基础。

- 按 6 个损毁等级分类存放
- 包含每个类别的点位 CSV
- 包含全量附件索引和汇总 CSV
- 最终规模为 18,428 个 points，19,780 个 attachments

详细说明见：[`Eaton_Fire/README.md`](./Eaton_Fire/README.md)

### 2. `Altadena_Images/`

这是基于 `Eaton_Fire_attachments_index.csv` 生成的配对数据集，按 sample 组织。

- 每个附件记录对应一个 `sample_xxxxx` 文件夹
- 每个 sample 可以包含 `street_view.jpg` 和 `remote_sensing.jpg`
- 同时保留配对索引表和下载日志
- 最终规模为 19,780 个 sample 文件夹，19,746 个完整配对，91 个 GeoTIFF 瓦片

详细说明见：[`Altadena_Images/README.md`](./Altadena_Images/README.md)

## 数据汇总

下面的统计来自最终的 `Eaton_Fire_summary.csv`。

| 损毁类别 | Points | Attachments |
| --- | ---: | ---: |
| Destroyed (>50%) | 9,419 | 9,490 |
| Major (26-50%) | 70 | 155 |
| Minor (10-25%) | 148 | 312 |
| Affected (1-9%) | 858 | 1,809 |
| Inaccessible | 40 | 33 |
| No Damage | 7,893 | 7,981 |
| Total | 18,428 | 19,780 |

这里的 `points` 表示受灾评估点位，`attachments` 表示与这些点位关联的图片附件，所以两者数量本来就不一定相同。

## 仓库结构

```text
.
|-- README.md
|-- README_CN.md
|-- requirements.txt
|-- Eaton_Fire/
|-- Altadena_Images/
|-- scripts/
|   |-- data_prep/
|   |-- features/
|   `-- visualization/
|-- notebooks/
|-- data/
`-- outputs/
```

## 主要脚本

- `scripts/data_prep/parse_metadata.py`：解析 `Eaton_Fire/` 中的标注数据，并索引 `Altadena_Images/` 中的 sample 文件夹
- `scripts/data_prep/train_val_test_split.py`：生成训练集、验证集和测试集
- `scripts/features/extract_clip_embeddings.py`：提取标注数据和 sample 数据的 CLIP 特征
- `scripts/features/sample_raster_values.py`：在点位上采样遥感栅格特征
- `scripts/visualization/map_damage_points.py`：生成交互式损毁地图
- `scripts/visualization/sample_grid.py`：导出样本拼图，便于 QA 检查

## 典型使用流程

### 1. 安装依赖

```bash
python -m venv .venv
.venv\Scripts\Activate.ps1
pip install -r requirements.txt
pip install -r Altadena_Images/requirements.txt
```

### 2. 解析元数据

```bash
python scripts/data_prep/parse_metadata.py --root .
```

会生成：

- `data/eaton_fire_metadata.csv`
- `data/eaton_fire_points.geojson`
- `data/altadena_unlabeled_index.csv`

### 3. 生成数据划分

```bash
python scripts/data_prep/train_val_test_split.py --root .
```

### 4. 提取 CLIP 特征

```bash
python scripts/features/extract_clip_embeddings.py --root . --split labeled
python scripts/features/extract_clip_embeddings.py --root . --split unlabeled
```

### 5. 按需加入遥感特征

```bash
python scripts/features/sample_raster_values.py --root . --raster /path/to/dNBR.tif --band_name dNBR
```

### 6. 生成质检可视化结果

```bash
python scripts/visualization/map_damage_points.py --root .
python scripts/visualization/sample_grid.py --root . --n 6
```

## 版本管理策略

会提交到 GitHub 的内容：

- 源代码
- notebooks
- 文档
- 目录骨架和 `.gitkeep`

只保留在本地、不提交的内容：

- 原始街景图片
- 遥感瓦片和配对输出
- 生成的特征数组和元数据表
- 生成的地图、图片和报告
- 虚拟环境和压缩包

## 备注

- `Eaton_Fire/` 是项目中最重要的标注数据集。
- `Altadena_Images/` 是基于同一份附件索引构建的 sample 级配对数据集。
- 代码里目前仍然把 paired split 叫做 `unlabeled`，这只是脚本里的工作流命名，不代表它没有源数据字段。
