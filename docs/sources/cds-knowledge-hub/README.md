# CDS Knowledge Hub 抓取语料

本目录保存从 Medidata Knowledge Hub 抓取的 **Clinical Data Studio 及其功能用户指南** 一手文档文本，作为 `docs/requirements/medidata-cds-frd.md` 的证据来源。

## 来源

- 门户：`https://learn.medidata.com/en-US/bundle/<bundle>/`
- 后端 API：`https://learn-be.medidata.com/api/bundle/<bundle>/toc` 与 `.../page/<nav_path>`
- 抓取日期：2026-11-02。
- 需登录访问，语料为登录后抓取。

## 抓取的 bundle

| bundle | 功能页数 | 说明 |
|---|---|---|
| `clinical-data-studio` | 623 | 产品主页 / about / 各模块 release notes（v2.0 来源） |
| `data-connect` | 269 | Data Connect 功能用户指南（v3.0 补充） |
| `data-surveillance-and-rbqm` | 317 | Data Surveillance 与 RBQM 功能用户指南（v3.0 补充） |
| `ingestor` | 94 | Ingestor 功能用户指南（v3.0 补充） |

> v2.0 仅抓了 `clinical-data-studio` bundle，遗漏了后三个**功能用户指南** bundle；v3.0 补齐。

## 内容

| 文件 | 说明 |
|---|---|
| `toc.json` | `clinical-data-studio` 原始 TOC（839 条目） |
| `toc.tsv` | 扁平化 TOC，列为 `depth \t title \t nav_path` |
| `pages/<nav_path>.txt` | `clinical-data-studio` 单页文本（623 个，去 HTML） |
| `corpus.txt` | `clinical-data-studio` 全页面合并语料 |
| `toc-<bundle>.json` | 其余三个 bundle 的原始 TOC |
| `pages-<bundle>/<nav_path>.txt` | 其余三个 bundle 的单页文本 |
| `corpus-<bundle>.txt` | 其余三个 bundle 的合并语料 |

## 注意

- 仅为英文原文留档，供检索与差异比对，不代表最终产品需求。
- 语料不包含 Medidata 的内部实现、数据库表结构、API 契约与算法参数。
- 回溯需求来源时，在对应 `pages-*` 目录或 `corpus-*.txt` 中按页面名检索。
