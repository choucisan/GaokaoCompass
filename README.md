---
language:
- zh
- en
license: mit
task_categories:
- tabular-classification
- question-answering
- text-retrieval
tags:
- gaokao
- college-admission
- china
- education
- score-ranking
- enrollment-plan
- admission-data
- tabular
size_categories:
- 10M<n<100M
---

<p align="center">
  <a href="#english"><img src="https://img.shields.io/badge/Language-English-blue?style=flat-square" alt="English"></a>
  <a href="#中文"><img src="https://img.shields.io/badge/Language-中文-red?style=flat-square" alt="中文"></a>
</p>

<p align="center">
  <img src="images/gaokao.jpeg" alt="GaokaoCompass" width="800"/>
</p>

<p align="center">
  <a href="https://github.com/choucisan/GaokaoCompass"><img src="https://img.shields.io/badge/GitHub-GaokaoCompass-181717?style=for-the-badge&logo=github" alt="GitHub"></a>
  <a href="https://huggingface.co/datasets/choucsan/Gaokao-Compass-11M"><img src="https://img.shields.io/badge/%F0%9F%A4%97_HuggingFace-Dataset-yellow?style=for-the-badge" alt="Hugging Face"></a>
  <a href="https://choucisan.github.io/collections/gaokao_compass"><img src="https://img.shields.io/badge/Blog-Post-blue?style=for-the-badge" alt="Blog"></a>
  <a href="https://www.xiaohongshu.com/explore/"><img src="https://img.shields.io/badge/RedNote-Post-red?style=for-the-badge" alt="RedNote"></a>
  <a href="https://choosealicense.com/licenses/mit"><img src="https://img.shields.io/badge/License-MIT-green?style=for-the-badge" alt="License"></a>
</p>

---

# English

# GaokaoCompass — China College Admission Dataset

**GaokaoCompass** is a structured dataset of China's national college entrance examination (Gaokao) admission records, covering **all 31 provinces from 2017 to 2025**. It includes enrollment plans, university admission cutoff scores, major-level admission scores, and score-ranking tables. The dataset is designed to help students, parents, and researchers make informed decisions with transparent, queryable admission data.

---

## Overview

| Metric | Value |
|--------|-------|
| Provinces | 31 |
| Year span | 2017–2025 |
| Total CSV files | 861 |
| Total data rows | 11,327,563 |
| Full coverage (all 4 tables) | 31 provinces × 2022–2025 |

### Per-table statistics

| Table | Files | Rows | Description |
|-------|------:|-----:|-------------|
| enrollment-plan | 227 | 5,862,063 | University enrollment plans by province |
| major-admission | 218 | 4,497,048 | Major-level admission scores |
| school-admission | 227 | 763,558 | University-level admission cutoff scores |
| score-range | 189 | 204,894 | Score-ranking tables (candidate distribution) |

---

## Directory Structure

```text
data/
├── 2017/
│   ├── anhui/
│   │   ├── enrollment-plan.csv        # Enrollment plan
│   │   ├── school-admission.csv       # University admission cutoff
│   │   ├── major-admission.csv        # Major-level admission scores
│   │   ├── score-range.csv            # Score-ranking table
│   │   └── meta.json                  # Summary statistics (6 dashboard cards)
│   ├── beijing/
│   │   └── ...
│   └── ... (31 provinces)
├── 2018/
│   └── ...
└── 2025/
    └── ...
```

---

## Table Schemas

### 1. score-range.csv — Score-Ranking Table

Shows how many candidates scored at each point level, enabling rank lookup.

| Field | Type | Description |
|-------|------|-------------|
| province | string | Province name |
| year | int | Year |
| category | string | Subject track (理科/文科/物理类/历史类/综合) |
| batch | string | Admission batch |
| control_score | int | Batch cutoff score |
| score | int | Score point |
| score_range | string | Score range label |
| segment_count | int | Number of candidates at this score |
| cumulative_count | int | Cumulative count (rank) |
| rank_range | string | Rank range label |

### 2. enrollment-plan.csv — Enrollment Plan

Each row is one university-major enrollment slot in a province.

| Field | Type | Description |
|-------|------|-------------|
| province | string | Province |
| year | int | Year |
| batch | string | Batch (本科一批/本科二批/专科批 etc.) |
| category | string | Subject track |
| university_code | string | University code |
| university_name | string | University name |
| major_group | string | Major group |
| major_code | string | Major code |
| major_name | string | Major name |
| major_note | string | Major notes |
| subject_req | string | Subject requirements |
| plan_count | int | Planned enrollment count |
| duration | string | Program duration |
| tuition | float | Annual tuition (CNY) |

### 3. school-admission.csv — University Admission Cutoff

Each row is one university's minimum admission score in a province.

| Field | Type | Description |
|-------|------|-------------|
| province | string | Province |
| year | int | Year |
| category | string | Subject track |
| batch | string | Batch |
| university_code | string | University code |
| university_name | string | University name |
| major_group | string | Major group |
| subject_req | string | Subject requirements |
| min_score | int | Minimum admission score |
| min_rank | int | Minimum admission rank |
| control_score | int | Provincial batch cutoff |
| score_diff | int | Score above batch cutoff |
| admit_count | int | Number admitted |
| school_province | string | Province where university is located |
| school_nature | string | Ownership (公办 public / 民办 private) |
| is_985 | int | Project 985 university (1/0) |
| is_211 | int | Project 211 university (1/0) |

### 4. major-admission.csv — Major-Level Admission Scores

Each row is one university-major's admission detail in a province.

| Field | Type | Description |
|-------|------|-------------|
| province | string | Province |
| year | int | Year |
| category | string | Subject track |
| batch | string | Batch |
| university_code | string | University code |
| university_name | string | University name |
| major_group | string | Major group |
| major_code | string | Major code |
| major_name | string | Major name |
| major_note | string | Major notes |
| subject_req | string | Subject requirements |
| min_score | int | Minimum score |
| min_rank | int | Minimum rank |
| max_score | int | Maximum score |
| avg_score | float | Average score |
| admit_count | int | Number admitted |
| school_province | string | University location |
| school_nature | string | Ownership |
| is_985 | int | Project 985 (1/0) |
| is_211 | int | Project 211 (1/0) |

---

## meta.json — Summary Statistics

Each province/year directory contains a `meta.json` with pre-computed statistics for 6 dashboard cards.

| Card | Key fields | Description |
|------|-----------|-------------|
| exam_overview | total_candidates, categories, batch_lines | Total candidates, batch cutoff scores |
| university_stats | total, is_985, is_211, public, private | University counts by type |
| enrollment_stats | total_plan, university_count, top_universities | Enrollment plan summary |
| top_majors | name, plan_count, university_count | Most popular majors |
| score_distribution | score, segment_count, cumulative_count | Sampled score distribution |
| admission_bands | min_score, max_score, median_score | Score ranges per batch |

Example (Zhejiang 2024):

```json
{
  "exam_overview": {
    "categories": [
      {
        "name": "综合",
        "total_candidates": 281057,
        "max_score": 699,
        "batch_lines": [
          {"batch": "平行录取一段", "score": 492},
          {"batch": "平行录取二段", "score": 269}
        ]
      }
    ],
    "total_candidates": 281057
  },
  "university_stats": {
    "total": 1608,
    "is_985": 37,
    "is_211": 108,
    "public": 900,
    "private": 400
  }
}
```

---

## Quick Start

### Load from Hugging Face

```python
from datasets import load_dataset

dataset = load_dataset("choucsan/Gaokao-Compass-11M")
```

### Read a CSV directly

```python
import pandas as pd

# Load 2024 Zhejiang enrollment plan
df = pd.read_csv("data/2024/zhejiang/enrollment-plan.csv")

# Search for Computer Science majors
cs = df[df["major_name"].str.contains("计算机科学与技术", na=False)]
print(cs[["university_name", "major_name", "plan_count", "tuition"]])
```

### Look up a score ranking

```python
import pandas as pd

# Load 2024 Henan score-ranking table
df = pd.read_csv("data/2024/henan/score-range.csv")

# Find rank for a score of 600
row = df[df["score"] == 600]
print(f"Rank at 600: {row['cumulative_count'].values[0]}")
```

### Use meta.json

```python
import json

with open("data/2024/zhejiang/meta.json", encoding="utf-8") as f:
    meta = json.load(f)

print(f"Total candidates: {meta['exam_overview']['total_candidates']}")
print(f"Universities: {meta['university_stats']['total']}")
print(f"Project 985 universities: {meta['university_stats']['is_985']}")
```

---

## Data Pipeline

Raw data is sourced from provincial education examination authorities and the National Education Examination Authority (阳光高考). The ETL pipeline:

1. **Discovery** — Scan raw Excel files per province, classify by type (enrollment plan / school admission / major admission / score range)
2. **Column normalization** — Map 30+ Chinese column name variants to unified English fields
3. **Cleaning** — Normalize subject tracks, safe numeric casting, strip decimals from code fields
4. **Deduplication** — MD5-based file dedup; row-level dedup by (university, batch, category, major_code)
5. **Year extraction** — Parse year from filenames and directory paths
6. **Validation** — Null-rate checks, error summary, completeness report

---

## 2025 Coverage

| Provinces | enrollment-plan | school-admission | major-admission | score-range |
|-----------|:-:|:-:|:-:|:-:|
| 28 provinces | ✅ | ✅ | ✅ | ✅ |
| Qinghai (青海) | ✅ | ❌ | ❌ | ❌ |
| Shanxi (山西) | ✅ | ✅ | ❌ | ✅ |
| Tibet (西藏) | ✅ | ✅ | ✅ | ❌ |

---

## Use Cases

- **College application** — Query eligible universities and majors by score and rank
- **Admission trend analysis** — Visualize cutoff score and enrollment changes over years
- **University comparison** — Compare admission difficulty across 985/211/public/private institutions
- **Major popularity** — Rank majors by enrollment quota and number of offering universities
- **Education research** — Academic studies and policy analysis on Gaokao data

---

# 中文

# 高考录取数据平台 · GaokaoCompass

**GaokaoCompass** 是一个面向中国高考（普通高等学校招生全国统一考试）的结构化录取数据集，覆盖 **31 个省份、2017–2025 年**的招生计划、院校投档线、专业录取分数和一分一段表数据。旨在为考生、家长和教育研究者提供透明、可查询的高考录取信息参考。

---

## 数据概览

| 指标 | 数值 |
|------|------|
| 省份 | 31 |
| 年份跨度 | 2017–2025 |
| 数据文件总数 | 861 个 CSV |
| 数据总行数 | 11,327,563 |
| 四表齐全省份/年份 | 2022–2025 年 31 省全覆盖 |

### 各表统计

| 数据表 | 文件数 | 数据行数 | 说明 |
|--------|--------|----------|------|
| enrollment-plan | 227 | 5,862,063 | 各省高校招生计划 |
| major-admission | 218 | 4,497,048 | 各高校专业录取分数 |
| school-admission | 227 | 763,558 | 各高校院校投档线 |
| score-range | 189 | 204,894 | 一分一段表（考生排名） |

---

## 数据结构

```text
data/
├── 2017/
│   ├── anhui/
│   │   ├── enrollment-plan.csv        # 招生计划
│   │   ├── school-admission.csv       # 院校投档线
│   │   ├── major-admission.csv        # 专业录取分数
│   │   ├── score-range.csv            # 一分一段表
│   │   └── meta.json                  # 统计摘要（6类卡片）
│   ├── beijing/
│   │   └── ...
│   └── ...（31省）
├── 2018/
│   └── ...
└── 2025/
    └── ...
```

---

## 四类数据表说明

### 1. score-range.csv · 一分一段表

考生分数排名数据，反映每个分数段的考生人数分布。

| 字段 | 类型 | 说明 |
|------|------|------|
| province | string | 省份 |
| year | int | 年份 |
| category | string | 科类（理科/文科/物理类/历史类/综合） |
| batch | string | 批次 |
| control_score | int | 批次控制线 |
| score | int | 分数 |
| score_range | string | 分数区间 |
| segment_count | int | 本段人数 |
| cumulative_count | int | 累计人数（排名） |
| rank_range | string | 排名区间 |

### 2. enrollment-plan.csv · 招生计划

各高校在各省的招生专业和计划人数。

| 字段 | 类型 | 说明 |
|------|------|------|
| province | string | 省份 |
| year | int | 年份 |
| batch | string | 批次（本科一批/本科二批/专科批等） |
| category | string | 科类 |
| university_code | string | 院校代码 |
| university_name | string | 院校名称 |
| major_group | string | 专业组 |
| major_code | string | 专业代码 |
| major_name | string | 专业名称 |
| major_note | string | 专业备注 |
| subject_req | string | 选科要求 |
| plan_count | int | 计划人数 |
| duration | string | 学制 |
| tuition | float | 学费（元/年） |

### 3. school-admission.csv · 院校投档线

各高校在各省的最低录取分数和位次。

| 字段 | 类型 | 说明 |
|------|------|------|
| province | string | 省份 |
| year | int | 年份 |
| category | string | 科类 |
| batch | string | 批次 |
| university_code | string | 院校代码 |
| university_name | string | 院校名称 |
| major_group | string | 专业组 |
| subject_req | string | 选科要求 |
| min_score | int | 最低分 |
| min_rank | int | 最低位次 |
| control_score | int | 省控线 |
| score_diff | int | 批次线差 |
| admit_count | int | 录取人数 |
| school_province | string | 学校所在省份 |
| school_nature | string | 办学性质（公办/民办） |
| is_985 | int | 是否 985 高校（1/0） |
| is_211 | int | 是否 211 高校（1/0） |

### 4. major-admission.csv · 专业录取分数

各高校各专业在各省的录取分数详情。

| 字段 | 类型 | 说明 |
|------|------|------|
| province | string | 省份 |
| year | int | 年份 |
| category | string | 科类 |
| batch | string | 批次 |
| university_code | string | 院校代码 |
| university_name | string | 院校名称 |
| major_group | string | 专业组 |
| major_code | string | 专业代码 |
| major_name | string | 专业名称 |
| major_note | string | 专业备注 |
| subject_req | string | 选科要求 |
| min_score | int | 最低分 |
| min_rank | int | 最低位次 |
| max_score | int | 最高分 |
| avg_score | float | 平均分 |
| admit_count | int | 录取人数 |
| school_province | string | 学校所在省份 |
| school_nature | string | 办学性质 |
| is_985 | int | 是否 985 |
| is_211 | int | 是否 211 |

---

## meta.json · 统计摘要

每个省份/年份目录下包含一个 `meta.json` 文件，提供 6 类卡片的统计信息，适合直接用于前端可视化展示。

| 卡片 | 字段 | 说明 |
|------|------|------|
| exam_overview | total_candidates, categories, batch_lines | 高考人数、批次控制线 |
| university_stats | total, is_985, is_211, public, private | 院校统计 |
| enrollment_stats | total_plan, university_count, top_universities | 招生计划统计 |
| top_majors | name, plan_count, university_count | 热门专业排名 |
| score_distribution | score, segment_count, cumulative_count | 分数分布采样 |
| admission_bands | min_score, max_score, median_score | 各批次录取分数段 |

示例（2024 浙江）：

```json
{
  "exam_overview": {
    "categories": [
      {
        "name": "综合",
        "total_candidates": 281057,
        "max_score": 699,
        "batch_lines": [
          {"batch": "平行录取一段", "score": 492},
          {"batch": "平行录取二段", "score": 269}
        ]
      }
    ],
    "total_candidates": 281057
  },
  "university_stats": {
    "total": 1608,
    "is_985": 37,
    "is_211": 108,
    "public": 900,
    "private": 400
  }
}
```

---

## 快速使用

### 从 Hugging Face 加载

```python
from datasets import load_dataset

dataset = load_dataset("choucsan/Gaokao-Compass-11M")
```

### 直接读取 CSV

```python
import pandas as pd

# 读取 2024 年浙江的招生计划
df = pd.read_csv("data/2024/zhejiang/enrollment-plan.csv")

# 查询计算机科学与技术专业的招生计划
cs = df[df["major_name"].str.contains("计算机科学与技术", na=False)]
print(cs[["university_name", "major_name", "plan_count", "tuition"]])
```

### 查询一分一段表

```python
import pandas as pd

# 读取 2024 年河南理科一分一段表
df = pd.read_csv("data/2024/henan/score-range.csv")

# 查看 600 分对应的排名
row = df[df["score"] == 600]
print(f"600分排名: {row['cumulative_count'].values[0]}")
```

### 使用 meta.json

```python
import json

with open("data/2024/zhejiang/meta.json", encoding="utf-8") as f:
    meta = json.load(f)

print(f"浙江2024高考人数: {meta['exam_overview']['total_candidates']}")
print(f"招生院校数: {meta['university_stats']['total']}")
print(f"985高校: {meta['university_stats']['is_985']} 所")
```

---

## 数据来源

数据来自各省教育考试院、阳光高考平台等公开渠道，经清洗、标准化和去重后整理为统一格式。

**处理流程：**
1. **文件发现**：扫描各省原始 Excel 文件，按类型自动分类
2. **列名标准化**：30+ 种原始列名映射为统一英文字段
3. **数据清洗**：科类标准化、数值安全转换、代码字段去小数点
4. **去重**：按文件大小 + MD5 去除重复文件，专业条目按 (院校, 批次, 科类, 专业代码) 去重
5. **年份提取**：从文件名和目录路径自动提取年份
6. **质量验证**：空值率检查、错误汇总、数据完整性报告

---

## 2025 年数据覆盖

| 省份 | enrollment-plan | school-admission | major-admission | score-range |
|------|:-:|:-:|:-:|:-:|
| 28 省 | ✅ | ✅ | ✅ | ✅ |
| 青海 | ✅ | ❌ | ❌ | ❌ |
| 山西 | ✅ | ✅ | ❌ | ✅ |
| 西藏 | ✅ | ✅ | ✅ | ❌ |

---

## 应用场景

- **考生志愿填报**：根据分数和排名查询可报考的院校和专业
- **录取趋势分析**：历年分数线、招生人数变化趋势可视化
- **院校对比**：985/211/公办/民办院校在各省的录取难度对比
- **专业热度分析**：各专业招生计划人数和开设院校数量统计
- **教育研究**：高考数据的学术研究和政策分析

---

## 许可证

[MIT License](https://choosealicense.com/licenses/mit)

---

## 联系方式

如有问题、纠错或合作需求：

[choucisan@gmail.com](mailto:choucisan@gmail.com)
