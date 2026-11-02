# DataStudio 领域上下文

> 定位已于 2026-11-02 整体转向临床试验数据质量管理平台，取代此前的数据治理平台定位，见 `docs/decisions/0003-pivot-to-clinical-data-quality-platform.md`。详细功能需求见 `docs/requirements/medidata-cds-frd.md`（v2.0，基于 Medidata Knowledge Hub 一手文档），术语见根 `GLOSSARY.md`，一手语料见 `docs/sources/cds-knowledge-hub/`。

## 产品分档

- **Data Surveillance 档**：Data Surveillance 能力套件 + Data Connect。
- **Data Quality Management 档**：上述全部 + RBQM + Cross Study Metrics。

能力套件：Data Connect（Datasets/Imports/Exports/Transformations）、Data Surveillance（Task Oversight、Clean Patient Tracker、Data Sets、Open EDC Queries、Vendor Queries、Listings、AI Reconciliation、Patient Profiles、Custom Graphs、RECIST 1.1、Configuration）、RBQM（Data Quality Hub、Risk Assessment、Risk Inventory、Plan Documents、Protocol Integrity Report、KRI/QTL Dashboard、Statistical Monitoring、Signals、Audit Trail Review、Configuration、Template Library）、Cross Study Metrics。

## 项目目标

DataStudio 是面向临床试验申办方与 CRO 的数据质量管理平台，对标 Medidata Clinical Data Studio（CDS）。它把 Medidata 内部与外部来源的临床数据接入为**单一的审阅与监查环境**，用 AI 与自动化减少人工审阅、对账与重复任务，让数据管理员、中心监查员、医学监查员、风险经理与 CRA 在同一份数据上协作。

平台支持风险质量管理（RBQM）：从关键质量因素（CtQ）与风险规划，到 KRI/QTL 监控、中心统计监测、信号检测、审计追踪审阅、医学监查与方案偏离的完整闭环。

## 一期范围

按优先级建设：

1. 多源数据接入与集成（Data Connect）
2. 数据审阅与数据质量管理（Data Surveillance）
3. 风险质量管理（RBQM）
4. 信号检测与管理（Signals）
5. 审计追踪审阅（Audit Trail Review）
6. 医学监查（Medical Monitoring）
7. 方案偏离管理（Protocol Deviation）
8. 平台通用能力（权限、审计追踪、通知、外部系统集成、AI 治理）

明确不做：自研 EDC/eSource 采集、RTSM、eTMF、影像与 Sensors 深度处理、通用 ETL/数仓、AI 模型自建训练、法规提交本身。

## 参与角色

- **数据管理员（Data Manager）**：数据审阅、质疑管理、对账、数据清理。
- **中心监查员（Central Monitor）**：中心化监查、KRI/QTL 监控、风险处置。
- **医学监查员（Medical Monitor）**：患者安全审阅、医学裁决、方案偏离签核。
- **CRA**：现场相关偏离与站点合规审阅。
- **风险经理（Risk Manager）**：CtQ/RACT/IQMP、QTL 与风险策略。
- **统计师 / 生物统计**：中心统计监测、统计分析与异常阈值。
- **临床程序员 / 数据集成人员**：数据接入、转换、数据集与导出、R/Python 集成。
- **系统管理员**：用户、角色、权限与研究配置。
- **审计员**：只读查看审计追踪与处置记录。
- **申办方 / CRO 管理者**：跨研究、跨站点概览与治理。
- **外部数据提供方**：提供外部数据集、配合数据问题处置。

一名用户可兼任多个角色，权限按角色与数据范围（研究/国家/站点/数据域）双重约束，取交集。

## 统一语言

核心术语见根 `GLOSSARY.md`。平台沿用 Medidata 专有名词（Rave、CTMS、SDTM、KRI、QTL、CSM、CtQ、RACT、IQMP、ATR、RECIST 1.1、TransCelerate、ICH E6(R3)、DTA 等），映射表见 FRD 第 13 章。

## 领域模型与生命周期

数据层级为：研究 → 国家 → 站点 → 受试者 → 访视；以及 源系统 → 数据集 → 数据点。转换产生派生数据集并保留端到端血缘。

核心事项生命周期：

```text
信号：新建 → 已路由 → 评估中 → 已确认(转问题) / 已驳回(噪声)
问题：待确认 → 已分派 → 处理中 → 待验证 → 已关闭（旁路：已豁免）
方案偏离：潜在偏离 → 待审阅 → 严重度确认 → 已批准 → 已同步（驳回：关闭）
```

## 接入与安全约束

- 数据来自 Medidata（含 Rave）自动流式接入，非 Medidata 源自服务导入。
- 研究数据每日自动刷新 4 次（约 9:00/15:00/21:00/3:00 EST），每次产生数据快照；Rave 与 Data Connect 激活数据集 6–12 小时内可用；Signals 每日 6:00 EST 提取。
- 数据到达即按数据传输协议（DTA）自动校验，未通过不进入审阅。
- 平台对源系统保持只读，不直接修改业务系统数据（质疑经源 EDC 执行）。
- API 凭据不得明文存储或展示；传输与静态加密；最小权限。
- 所有受控动作进入不可篡改的审计追踪；AI 输出仅为建议，不得自动形成法规意义上的裁决。
- 界面支持中/英，内置国际单位（SI）换算。

## 合规基线

- 21 CFR Part 11：审计追踪、访问控制、电子签名（适用范围待 GxP 范围确认）。
- ALCOA+ 数据完整性；数据转换血缘可追溯。
- ICH E6(R3) / RBQM：CtQ、RACT、IQMP、KRI、QTL、中心统计监测。
- 研究级数据隔离与受试者隐私；留存与法定冻结策略待确认。
- 支持 GAMP 5 计算机化系统验证（CSV）追溯。

## 一期建议验收指标

- 目标规模：单研究受试者 ≥ 5,000、数据点 ≥ 1 亿、并发审阅用户 ≥ 200（默认假设，待确认）。
- 多源数据接入覆盖率与 DTA 校验通过率。
- KRI/QTL 监控覆盖率。
- 信号/问题按期关闭率与方案偏离审批及时率。
- 审计追踪完整率（关键动作 100% 留痕）。

## 尚待明确

- 适用司法辖区、GxP 范围、受监管记录类型及系统预期用途。
- 电子签名适用范围与哪些动作构成法规意义上的签署。
- 药物警戒（PV）系统边界与安全报告传输路径。
- 身份源、SSO 方案、职责分离规则与定期权限复核。
- 合规留存矩阵、法定冻结、备份恢复及归档策略。
- 目标规模、SLA 与全部 NFR 数值（FRD 第 10 章为假设）。
- 与 CTMS/Rave 的接口版本与契约。
- 多语言与地区合规适用性（GDPR/HIPAA 等）。
