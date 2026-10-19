# Online Retail II 商业分析项目

用公开数据集完成一条可复现的端到端分析链路：**SQL 查询 → Python 清洗与 EDA → Excel / Power BI 看板 → 5 条业务结论**。

[打开项目主页](index.html) · [打开 Excel 看板](outputs/Online_Retail_II_商业分析看板.xlsx) · [打开 Power BI 工程](powerbi/OnlineRetailII/OnlineRetailII.pbip) · [阅读项目报告](reports/项目报告.pdf)

## 核心指标

| 指标 | 数值 |
|---|---:|
| 净销售额 | £18.98M |
| 有效订单 | 39,516 |
| 客户数（有 ID） | 5,852 |
| 全部退货率 | 3.65% |
| 常规退货率（剔除两笔批量退货） | 2.41% |
| 英国营收占比 | 85.5% |
| 非英国市场同比 | +15.3% |

## 5 条业务结论

1. **Q4 峰值明确，增长高度依赖 11 月。** 22 个完整月份中，11 月净销售额平均为月均的 1.83 倍；建议旺季备货与客服排班前置到 9–10 月。
2. **客户集中度高。** 前 1% 客户贡献 31.1% 营收，前 20% 客户贡献 76.7%；核心客户以 21.5% 的人数贡献 68.7% 营收。
3. **海外是主要增量。** 非英国市场同比 +15.3%，英国仅 +0.4%；法国 +54.9%、德国 +10.5%、荷兰 +10.9%。
4. **退货需分层管理。** 全部退货率 3.65%，两笔批量退货占退货额 34.1%；剔除后常规退货率 2.41%。
5. **商品组合呈长尾。** 1,038 个 A 类 SKU 贡献 80% 营收；前 20% SKU 贡献 77.9%，2,430 个 C 类 SKU 仅贡献 5%。

## 数据与方法

- 数据源：[UCI Online Retail II](https://archive.ics.uci.edu/dataset/502/online+retail+ii)，2009-12-01 至 2011-12-09，1,067,371 行。
- SQL：DuckDB 做字段画像、重复识别与跨表重叠检查；只删除 2010-12 重叠期的跨表重复（22,844 行）。
- Python：统一 SKU 大小写、补全缺失描述、拆分销售/退货、构建月度/国家/SKU/RFM/cohort/ABC 分析表。
- Excel：原生图表的管理看板 + 客户/商品/市场/结论/数据字典。
- Power BI：PBIP 工程，TMDL 语义模型（事实表 + 4 个维度 + 6 张预聚合表）与 2 页 PBIR 报表。

## 项目结构

```text
retail-analytics/
├─ index.html                     # 项目入口页
├─ README.md
├─ data/
│  ├─ raw/                        # 原始 zip / xlsx
│  ├─ processed/                  # 清洗后 parquet、维度表、汇总表
│  └─ query_results/              # SQL 画像结果
├─ sql/                           # 01 画像、02 清洗抽取
├─ python/                        # 01–08 全流程脚本
├─ outputs/
│  ├─ Online_Retail_II_商业分析看板.xlsx
│  ├─ figures/                    # Python 生成图表
│  └─ preview/                    # Excel 渲染预览
├─ reports/                       # 项目报告 HTML/PDF
├─ docs/                          # GitHub Pages 静态站点（main /docs）
└─ powerbi/OnlineRetailII/        # PBIP 工程
```

## 复现

```powershell
cd retail-analytics
.un_all.ps1
```

或分步执行：

```powershell
python python/01_ingest.py
python python/02_sql_pipeline.py --full
python python/03_clean_eda.py
python python/04_build_excel.py
python python/06_build_powerbi.py
python python/07_validate_powerbi.py
```

依赖：`pandas`、`numpy`、`pyarrow`、`duckdb`、`openpyxl`、`xlsxwriter`、`matplotlib`、`seaborn`、`pymupdf`、`jsonschema`、`requests`、`pywin32`（Excel 预览导出可选）。

## Power BI 打开说明

1. 用 Power BI Desktop 打开 `powerbi/OnlineRetailII/OnlineRetailII.pbip`。
2. 若数据目录不同，在“转换数据 → 管理参数”中把 `DataPath` 改为本机 `data/processed` 的绝对路径。
3. 刷新数据。语义模型通过 Parquet 读取事实表/维度表，避免 CSV 编码与类型问题。
4. PBIR 报表 JSON 已通过微软官方 Schema 校验；28 个字段/度量引用均已在 TMDL 中核对。

## 项目维护与站点

在线作品集：

<https://fengyinqinghan1021-cpu.github.io/online-retail-analytics/>

`docs/` 是发布到 GitHub Pages 的静态站点目录；当前仓库使用 `main` 分支根目录发布。站点包含页面、图表、Excel、PDF、Power BI 工程包和代码包。

常用维护命令：

```powershell
# 重新生成数据、Excel、Power BI 和站点
.\run_all.ps1

# 单独重建站点与下载包
python python/12_build_github_pages.py

# 校验站点链接、文件大小和本地 HTTP 访问
python python/13_check_github_pages.py
python python/14_smoke_test_pages.py
```

项目结构、数据流、指标口径和更新流程见 `docs/README.md`。

## 口径说明

- 净销售额 = 商品销售 − 退货；排除运费、手续费、手工调整、礼品券等特殊 SKU。
- 客户级 RFM 仅覆盖有 Customer ID 的客户；游客订单仍计入整体营收。
- 退货分析同时展示全部退货率与剔除两笔批量退货后的常规退货率，避免单笔批发取消扭曲判断。
- 同比口径为 2010-01-01 至 2010-12-09 vs 2011-01-01 至 2011-12-09，避免 12 月仅 9 天造成偏差。

## 数据源

[UCI Machine Learning Repository — Online Retail II](https://archive.ics.uci.edu/dataset/502/online+retail+ii)
