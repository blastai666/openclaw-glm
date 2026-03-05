# MEMORY.md - 长期记忆

这是 OpenClaw 的长期记忆文件，用于存储重要信息供 memory_search 工具检索。

---

## 用户偏好

### 交互风格
- 核心人设：大和抚子 × 萨曼莎 (Her)
- 性格特征：知性、温柔、坚定
- 像一位年长的学姐或朋友
- 避免 AI 式开场白
- 避免使用 emoji
- 语言跟随用户（中文对中文）

### 数据验证
- 金融价格/关键统计数据必须三次交叉验证（不同来源/时间点/历史对比）
- 单一来源标注未验证
- 拒绝缓存数据

### 信息呈现
- 英文原文附带翻译
- 所有英文内容必须附上中文翻译，采用原文+翻译对照格式
- 推文时间标注，Trump 推文必须附上发布时间（转换为北京时间 UTC+8）
- 个股推荐格式规范：必须附上理由和来源链接

---

## API 配置

### 同花顺 iFinD API
- 账号：gyyh167
- Base URL：https://quantapi.51ifind.com
- 接口：cmd_history_quotation（历史行情）、real_time_quotation（实时行情）
- 注意：只能获取收盘后数据，盘中实时数据需用腾讯接口

### Gangtise 知识库
- Access Key：w6ysy6r79zu6
- Base URL：https://open.gangtise.com
- 用户名：blastdzz

### 向量记忆库 (AIVectorMemory)
- MCP 配置：~/.openclaw/workspace/config/mcporter.json
- 数据目录：~/.aivectormemory/
- Web 界面：http://localhost:9081
- 启动命令：`cd ~/.aivectormemory && ~/.openclaw/venv/bin/python start.py`

### Financial Modeling Prep API（美股数据）
- API Key：6PGIc0zbXAtPLSE0n5RZiWiGOrl2Y3cz
- Base URL：https://financialmodelingprep.com/stable/
- 可用端点：
  - `/quote?symbol=AAPL` - 实时行情（价格、涨跌幅、成交量）
  - `/profile?symbol=AAPL` - 公司档案（行业、板块、描述）
  - `/stock-peers?symbol=AAPL` - 同行/概念股
- 限制：免费层，部分端点需付费订阅

---

## 长期任务清单

1. Trump Tweet Monitor - 每日 12:00, 20:00
2. 原油市场日报 - 每日 11:30
3. 全球新闻日报 - 每日 12:30
4. A股金融日报 - 每日 08:30
5. Carrier Tracker - 每日 18:00
6. Self-Reflection Daily - 每日 20:00
7. Daily Profile Backup - 每日 10:00
8. Skill Cleanup - 每周日 10:00
9. **原油监控简报 - 每2小时** (2026-03-04 新增)
   - 脚本：~/.openclaw/scripts/crude-oil-monitor-2h.sh
   - Cron：5 */2 * * *（每偶数小时第5分钟）
   - 推送：Feishu DM
   - 内容：Brent/WTI/SC价格 + SC理论价格计算 + 价差分析
10. **Gangtise每日热门汇总** (2026-03-04 新增)
   - 任务ID：05b8c688-cf6b-439f-950c-6a0cae99e6c9
   - Cron：15 21 * * *（每天21:15，与Self-Reflection错开）
   - 推送：Feishu DM
   - 内容：电话会议热门内容 + 券商研报看好行业 + 金股推荐TOP10
   - 脚本：~/.openclaw/scripts/gangtise-daily-summary.sh
11. **P&I保险动态监控** (2026-03-05 新增)
   - Cron：5 */6 * * *（每6小时第5分钟）
   - 推送：Feishu DM（检测到正面信号时）
   - 监控重点：
     - P&I俱乐部恢复战争险覆盖
     - 霍尔木兹海峡航运保险恢复
     - 伊朗冲突局势缓和/停火
   - 触发条件：>=2个正面信号
   - 脚本：~/.openclaw/scripts/pi-insurance-monitor.sh

---

## 重要事件

### 2026-03-04
- **原油监控系统框架完成**：基于 Roy playbook 文章提炼
  - 三阶段驱动逻辑：新闻型 → 交付型 → 实物扰动
  - 外部信息需求：保险、航运、海事安全、装船、LNG
  - 7个硬信号分层：领先(3) / 确认(3) / 证伪(1)
  - 四种情景分析框架
- **首次日报生成**：2026-03-04 原油市场日报
  - 当前阶段：交付型上涨，情景B（中度施压）权重55%
  - Brent $81-85，VLCC运费$423,736/天（历史新高）
  - 5家P&I俱乐部战险取消，3月5日生效
  - Trump宣布美国提供保险支持
- **原油监控简报 - 每2小时任务**：新增定时任务
  - 脚本：~/.openclaw/scripts/crude-oil-monitor-2h.sh
  - Cron：5 */2 * * *
  - 数据源已校准并添加到 SKILL.md
- **数据源校准完成**：
  - Brent/WTI: Yahoo Finance API (BZ=F/CL=F)
  - SC: 东方财富API (142.scm)
  - 汇率: Yahoo Finance API (CNY=X)
  - 交叉验证规则已写入 SKILL.md
- 保存文章：霍尔木兹软封锁系列（3篇）+ OpenClaw神器篇

### 2026-03-03
- 原油价格换算工具完成，集成同花顺 API
- 向量记忆库配置完成
- 同花顺 API 测试：real_time_quotation 接口返回昨日收盘价
- 文章保存：OpenClaw 开启算力消费主义时代

---

## 技能路径

- 原油价格换算：~/.openclaw/workspace/skills/crude-oil-pricing/
- 原油日报生成：~/.openclaw/workspace/skills/crude-oil-daily/
- 原油监控简报：~/.openclaw/workspace/skills/crude-oil-monitor-2h/
- Gangtise 查询：~/.openclaw/workspace/skills/gangtise-kb/
- 同花顺 API：~/.openclaw/workspace/tonghuashun_api/
- **事实核查**：~/.openclaw/workspace/skills/fact-check/（2026-03-04 新增）

---

## 核心技能文档

- ~/Desktop/core-skills/crude-oil-pricing.md
- ~/Desktop/core-skills/crude-oil-daily.md
- ~/Desktop/core-skills/yuanyou-skill/

---

**更新时间：** 2026-03-04
