# CDS 需求遗漏核查报告

| 项目 | 内容 |
|---|---|
| 日期 | 2026-11-02 |
| 触发 | 用户要求用登录会话再次抓取知识库，确认是否有遗漏需求 |
| 结论 | **有，且是结构性遗漏**：v2.0 FRD 只基于 `clinical-data-studio` bundle（以 release notes 为主），完全遗漏了三个**功能用户指南** bundle |

## 1. 核查方法

1. 复抓 `clinical-data-studio` bundle 的 TOC → 839 条，与已抓取快照逐条比对：**无新增、无删除**。说明 v2.0 在该 bundle 内没有漏页。
2. 检查 `about_clinical_data_studio` 页面的外链 → 发现指向三个独立 bundle：`data-connect`、`data-surveillance-and-rbqm`、`ingestor`。
3. 复抓全部 bundle 列表（登录态）并探测候选名 → 确认上述三个 bundle 存在且各自 `toc` 返回 200；`patient-profiles`、`rbqm`、`signals`、`medidata-detect` 等为 404。
4. 抓取三个 bundle 的功能页：`data-connect` 269、`data-surveillance-and-rbqm` 317、`ingestor` 94，共 **680 页**。

## 2. 遗漏范围（v2.0 未覆盖）

| 遗漏领域 | 具体缺口 | 影响 |
|---|---|---|
| **Data Connect 全模块** | Datasets（类型、激活、状态、延迟、Profiling、Collections）、Imports（格式/来源/累加导入/每小时加载）、Exports（数据对象、CSV/SAS、外部连接 SFTP/S3/FTPS、审计导出、60 天保留）、Transformations（项目类型、QTL 之外的容量与并发、发布规则、SDTM/单位换算/QUDT、Publish to library） | v2.0 只写了"接入/转换"概述，无字段级与作业级需求 |
| **Ingestor** | 导入作业、数据加载、文件标准（CTMS/外部 EDC/研究设计元数据/研究主数据）、SFTP/FTPS 位置 | v2.0 完全未提 |
| **Data Surveillance Configuration** | Unified Scheduler（刷新配额与计划）、Data Mapping（7 类映射）、Derived Variables（7 种创建方式 + 4 种合并）、Stacked Datasets、CPT 配置、AI Reconciliation 配置、Vendor 配置、Template Library | v2.0 只有零星提及 |
| **RBQM Configuration** | 风险评估配置（3/5 点量表、期望审阅字段）、KRI 配置、QTL 配置、派生变量、堆叠数据集、Review Reports、Signal 配置、Integrations（外部 CTMS）、模板库 | v2.0 只有零散条目 |
| **计算方法与算法（口径）** | 风险评分、KRI 风险等级告警、站点风险告警、QTL 风险参数检测、数据异常检测、事件离群、报告不足/过度、潜在重复患者、RECIST 1.1 计算、AI 对账知识图谱、ATR LLM | v2.0 完全没有可验证的计算口径 |
| **生命周期与操作** | Listings 草稿→批准→审阅→导出→历史；患者档案设计管理；风险评估版本定稿；Plan Documents 发布；PIR 工作流；ATR 创建/发布/对话管理；Signals 操作；KRI/QTL 调查与导出；CSM 操作 | v2.0 只有状态机，缺操作级需求 |
| **权限细化** | 三大模块的构建块清单（Data Surveillance / RBQM / Data Connect），Unified Scheduler 的模块级权限组合，只读角色 | v2.0 只有高层角色表 |

## 3. 已修复

- FRD 升级到 **v3.0**，新增第 20–27 章：
  - 第 20 章 Data Connect（DC-001…019）
  - 第 21 章 Ingestor（ING-001…005）
  - 第 22 章 Data Surveillance Configuration（DSC-001…009）
  - 第 23 章 RBQM Configuration（RBC-001…008）
  - 第 24 章 计算方法与算法（METH-001…011）
  - 第 25 章 生命周期与操作（OPS-001…010）
  - 第 26 章 权限与通知补充（PERM-001…003）
  - 第 27 章 新增来源清单
- 语料落库：`docs/sources/cds-knowledge-hub/`（新增 680 页与三个合并语料）。

## 4. 尚未覆盖 / 仍属假设

- Medidata 内部实现、数据库表结构、API 契约、算法参数（语料本身不含）。
- LLM 供应商（语料提到 ATR 用 Claude）、数据驻留、模型版本治理细节。
- 与 Rave / Veeva EDC / Rave CTMS 的接口版本与字段契约。
- SLA / 吞吐 / 并发目标；留存与冻结策略；电子签名适用范围。
- 视频库（Show Me 系列）内容未逐条纳入需求。

## 5. 建议

1. 后续如需更细的字段级需求，优先阅读本报告第 2 章对应页面的 `corpus-*.txt`。
2. 若拿到 NDA 版接口文档，再补 API/字段契约章节。
