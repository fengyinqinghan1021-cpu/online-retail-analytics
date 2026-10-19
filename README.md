# Online Retail II | 数据分析作品集

在线作品集：<https://fengyinqinghan1021-cpu.github.io/online-retail-analytics/>

GitHub 仓库：<https://github.com/fengyinqinghan1021-cpu/online-retail-analytics>

## 项目概览

使用 UCI 公开数据集 **Online Retail II**（106.7 万行，2009-12 至 2011-12），完成从原始交易数据到管理看板的完整分析链路：

**SQL 查询 → Python 清洗与 EDA → Excel / Power BI 看板 → 5 条业务结论**

项目关注四条业务主线：营收与季节性与市场结构、客户价值分层、商品组合与长尾、退货与运营节奏。

## 数据源与口径

- 数据源：[UCI Online Retail II](https://archive.ics.uci.edu/dataset/502/online+retail+ii)
- 原始数据：1,067,371 行，两张年度工作表，2010-12 有 9 天重叠
- 清洗后：1,044,527 行；只删除跨表重复，保留同表内可能的真实重复录入
- 净销售额 = 商品销售金额 − 退货金额；排除运费、手续费、手工调整等特殊 SKU
- 客户级 RFM 仅覆盖有 Customer ID 的客户；游客订单仍计入整体营收
- 退货分析同时展示全部退货率与剔除两笔批量退货后的常规退货率

## 核心指标

| 指标 | 数值 |
|---|---:|
| 净销售额 | £18.98M |
| 有效订单 | 39,516 |
| 客户数（有 ID） | 5,852 |
| 全部退货率 | 3.65% |
| 常规退货率 | 2.41% |
| 英国营收占比 | 85.5% |
| 非英国市场同比 | +15.3% |

## 5 条业务结论

1. **Q4 峰值明确**：22 个完整月份中，11 月净销售额平均为月均的 1.83 倍。
2. **客户高度集中**：前 1% 客户贡献 31.1% 营收，前 20% 客户贡献 76.7%。
3. **海外是主要增量**：非英国市场同比 +15.3%，英国仅 +0.4%。
4. **退货需分层管理**：全部退货率 3.65%；两笔批量退货占退货额 34.1%，剔除后常规退货率 2.41%。
5. **商品组合呈长尾**：1,038 个 A 类 SKU 贡献 80% 营收，2,430 个 C 类 SKU 仅贡献 5%。

## 交付物

- [Excel 管理看板](downloads/online-retail-dashboard.xlsx)
- [项目报告 PDF](downloads/online-retail-report.pdf)
- [Power BI 工程包](downloads/online-retail-pbip.zip)
- [SQL + Python 代码包](downloads/online-retail-code.zip)

## 技术架构

```text
原始 Excel（两张年度表）
  → DuckDB SQL：字段画像、跨表去重、清洗抽取
  → pandas：描述补全、销售/退货拆分、RFM、cohort、ABC
  → 汇总表（Parquet / CSV）
  → Excel 看板 + Power BI（TMDL 语义模型 + PBIR 报表）
  → docs/ 静态站点（GitHub Pages）
```

## 项目结构

```text
retail-analytics/
├─ data/
│  ├─ raw/                 # 原始 Online Retail II
│  ├─ processed/           # 清洗后明细、维度、汇总表
│  └─ query_results/       # SQL 画像结果
├─ sql/                    # 画像与清洗抽取
├─ python/                 # 01–14 全流程脚本
├─ outputs/                # Excel、图表、预览
├─ reports/                # 项目报告 HTML/PDF
├─ powerbi/OnlineRetailII/ # PBIP 工程
└─ docs/                   # GitHub Pages 静态站点
```

## 更新与维护

### 数据更新

```powershell
python python/01_ingest.py
python python/02_sql_pipeline.py --full
python python/03_clean_eda.py
```

### 看板更新

```powershell
python python/04_build_excel.py
python python/06_build_powerbi.py
python python/07_validate_powerbi.py
```

### 站点更新

```powershell
python python/08_build_portal.py
python python/12_build_github_pages.py
python python/13_check_github_pages.py
python python/14_smoke_test_pages.py
```

`docs/` 是 GitHub Pages 的发布目录，当前使用 `main` 分支根目录。重新生成后提交并推送即可触发 Pages 重建。

### 质量门槛

- PBIP 报表 JSON 通过微软官方 Schema 校验；28 个字段/度量引用全部解析到 TMDL。
- 站点 18 个内部链接可解析，最大单文件低于 GitHub 100 MB 限制。
- 本地 HTTP 冒烟测试覆盖首页、图表、Excel、PDF、Power BI 包和代码包。

## 说明

项目用于数据分析与作品展示。Power BI 工程包是下载文件，需要 Power BI Desktop 打开；GitHub Pages 只托管静态页面和下载资源。
