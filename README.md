# AI-media-art-RAG
# 人工智能艺术数据库与 RAG 项目 (AI Art Database & RAG Project)

## 1. 项目简介

本项目旨在构建一个专门针对“人工智能艺术”(AI Art) 的数据库，并以此为基础开发一个 RAG (Retrieval-Augmented Generation) 系统。

与传统爬虫不同，本项目采用“策展优先”(Curation-First) 的策略，通过 Google 可编程搜索引擎 (PSE) 对一个**手动策展的“权威网站白名单”**进行定向、深度的信息搜集，从源头上确保数据的高质量和高相关性。

Google 可编程搜索引擎 (PSE)试用：https://cse.google.com/cse?cx=06a1cc7858e024889

- **中文简介:** 本项目搜集公开的人工智能艺术信息，用于构建一个专门的数据库与 RAG。
- **English:** This project collects public web data on AI art to build a specialized database and RAG.

## 2. 项目目标

1.  **构建元数据库 (Build Metadata Database):** 从海量信息中提取并结构化关键实体，如 `艺术家`, `作品`, `展览`, `所用技术`。
2.  **构建知识库 (Build Knowledge Base):** 搜集高质量的理论文章、作品描述和策展论述，作为 RAG 系统的知识来源。
3.  **实现智能检索 (Enable RAG):** 最终允许用户通过自然语言对这个专业的 AI 艺术知识库进行提问和研究。

## 3. 核心方法 (Methodology)

### 阶段一：策展 (Curation) - 权威网站白名单

项目的基石是一个手动维护的权威信源列表 (`媒体艺术网站列表.xlsx - Sheet1.csv`)。该列表目前包含 188+ 个网站，覆盖：

* **顶尖机构与档案库:** (例如: `ars.electronica.art`, `zkm.de`, `rhizome.org`)
* **权威艺术媒体:** (例如: `e-flux.com`, `hyperallergic.com`, `artnet.com`)
* **高端画廊与市场:** (例如: `ocula.com`, `artsy.net`, `pacegallery.com`)
* **艺术院校与研究中心:** (例如: `iamas.ac.jp`, `ntua.edu.tw`)

### 阶段二：基础设施 (Infrastructure) - Google PSE

我们使用 Google Programmable Search Engine (PSE) 创建了一个自定义搜索引擎，并进行了如下关键配置：

1.  **“仅搜索包含的网站” (Search only included sites):**
    * **关闭**“在整个网络中搜索”。
    * 这确保所有结果 **100%** 来自我们的权威白名单。
2.  **网站添加语法:**
    * 为确保完全覆盖，每个域名都使用两种格式添加：`example.com/*` (裸域名) 和 `*.example.com/*` (所有子域名)。

### 阶段三：数据搜集 (Data Collection) - 分批查询

为了在白名单网站中抓取**所有**相关页面（而不仅是“权重最高”的页面），并规避 Google 的 32 个词查询上限，我们采用了“分批查询”(Batch Queries) 策略。

我们将全面的关键词列表（见 `keywords.json`）拆分为多个、主题集中的查询批次，并**“按日期排序”** (Sort by Date) 来查看结果，以此打破网站权重（Relevance）的干扰。

#### 关键词列表 (示例)

* **批次 1 (核心AI概念):** `"AI"`, `"Artificial Intelligence"`, `"人工智能"`, `"人工知能"`, `"人工智慧"`
* **批次 2 (ML/DL/GAN):** `"neural network"`, `"神经网络"`, `"GAN"`, `"Generative adversarial network"`, `"Machine learning"`, `"Deep learning"`
* **批次 3 (生成式AI):** `"Stable Diffusion"`, `"Style transfer"`, `"generative"`, `"generation"`, `"生成"`
* **批次 4 (LLM):** `"ChatGPT"`, `"Gemini"`, `"Claude"`, `"Language Model"`, `"语言模型"`
* **批次 5 (技术流程):** `"Computer vision"`, `"Big data"`, `"Training"`, `"Pattern recognition"`
* ... (以此类推) ...

### 阶段四：处理与应用 (Processing & Application) [未来工作]

1.  **抓取 (Scrape):** 通过 API 获取所有查询结果的 URL 列表，并抓取这些页面的文本内容。
2.  **提取 (Extract):** 使用 NLP 和 LLM 模型从文本中提取艺术家、作品、展览等结构化信息。
3.  **入库 (Ingest):** 将结构化数据存入元数据库，将纯文本存入 RAG 的向量知识库。

## 4. 项目结构 (Project Structure)

```
.
├── 媒体艺术网站列表.xlsx - Sheet1.csv  # 权威网站白名单
├── keywords.json                # 用于分批搜索的关键词 (JSON 格式)
├── scripts/
│   ├── collector.py             # (待开发) API调用与URL搜集脚本
│   ├── scraper.py               # (待开发) 网页内容抓取脚本
│   └── processor.py             # (待开发) 文本提取与实体识别脚本
├── output/
│   └── urls_collected.csv       # (待开发) 搜集到的原始URL列表
└── README.md                    # 本项目说明
```
