# Hurricane Ian Cross-View 数据集

[English README](./README.md)

这个目录用于保留仓库中的 Hurricane Ian 本地 cross-view 数据集结构。

这份本地数据是按 pair 组织的，包含卫星图、街景图、数据划分文件和文字描述文件。

## 本地结构

```text
IAN_hurricane/
|-- images/
|   |-- 000000_sat.png
|   |-- 000000_svi.png
|   `-- ...
|-- pairs.csv
|-- captions.csv
|-- train_split.csv
`-- test_split.csv
```

## 当前本地统计

目前这份本地数据包含：

- `pairs.csv` 中 4,121 条配对记录
- `captions.csv` 中 4,121 条描述记录
- `train_split.csv` 中 3,821 条训练记录
- `test_split.csv` 中 300 条测试记录
- `images/` 中共 8,242 张图片
- 其中 `*_sat.png` 有 4,121 张
- `*_svi.png` 有 4,121 张

`pairs.csv` 中的损毁等级分布如下：

| Severity | Count |
| --- | ---: |
| `0_MinorDamage` | 1,407 |
| `1_ModerateDamage` | 1,538 |
| `2_SevereDamage` | 1,176 |
| Total | 4,121 |

## 表结构

`pairs.csv`、`train_split.csv` 和 `test_split.csv` 的字段为：

- `sat_path`
- `svi_path`
- `severity`

`captions.csv` 额外增加：

- `description`

重要说明：

- 你现在这份本地 CSV 不包含纬度和经度字段
- 这些路径字段只能视为这份本地数据快照中的文件引用，不能当作地理坐标信息使用

## 来源与引用

这份数据建议引用以下论文：

Li, H., Deuser, F., Yin, W., Luo, X., Walther, P., Mai, G., Huang, W., and Werner, M. (2025). *Cross-view geolocalization and disaster mapping with street-view and VHR satellite imagery: A case study of Hurricane IAN*. *ISPRS Journal of Photogrammetry and Remote Sensing*, 220, 841-854. [https://doi.org/10.1016/j.isprsjprs.2025.01.003](https://doi.org/10.1016/j.isprsjprs.2025.01.003)

根据该论文公开元数据，作者在论文中引入了一个新的 Hurricane Ian cross-view 数据集，名称为 `CVIAN`。

## 仓库说明

这个仓库只跟踪 `IAN_hurricane/` 的文档和目录骨架，真正的图片与本地 CSV 文件默认仍然被 Git 忽略，不直接提交。
