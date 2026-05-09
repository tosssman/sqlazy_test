# SQLazy
告别嵌套SQL地狱，用自然语言分步写查询，100%无幻觉生成可审计的标准SQL
---
## 🎯 解决什么痛点？
复杂OLAP分析查询，用原生SQL写往往是多层嵌套、超长窗口函数，不仅开发效率低、可读性差，更关键的是：
- AI直接生成的SQL经常出现语法对但语义错的幻觉，无法审计，不敢用于生产环境；
- 复杂SQL调试极其困难，无法定位每一步的计算结果；
- 不同数据库的SQL方言不兼容，一套查询要反复改写；
- 新手难以驾驭窗口函数、多层子查询、有序计算等高级SQL能力。
SQLazy通过分步自然语言查询，彻底解决这些问题。
## ✨ 核心优势
### 🔍 白盒可审计，100%无幻觉
和纯AI生成SQL的黑盒模式完全不同：
- 你的需求会被转换成可读的分步规范自然语言，你可以逐行确认逻辑是否正确；
- 逻辑确认后，由编译器生成最终SQL，绝不会出现语义幻觉；
- 完美适配企业代码审计、生产环境高可靠性要求。
### 🧩 分步开发，像搭积木一样写复杂查询
再复杂的分析需求，都可以拆成多个简单步骤，每一步只做一个动作，无需关心SQL嵌套逻辑。
#### 典型示例：计算股票最长连续上涨天数
用 SQLazy 分步实现，逻辑一目了然，而且可以分步调试：
```
t2 = stock 筛选 CODE 等于 100046
t3 = t2 排序 DT 升序
t4 = t3 分段 CL 变小 命名 NoRisingDays
t5 = t4 汇总 DT 计数 命名 ContinuousDays 分组 NoRisingDays
结果 = t5 汇总 ContinuousDays 最大 命名 max_ContinuousDays
```
对应的原生 Oracle SQL 需要 4 层嵌套 + 窗口函数，可读性极差，也难以调试：
```sql
WITH t3 AS (
  SELECT CODE, DT, CL FROM stock WHERE CODE = 100046
)
SELECT MAX(ContinuousDays) AS max_ContinuousDays
FROM (
  SELECT NoRisingDays, COUNT(DT) AS ContinuousDays
  FROM (
    SELECT CODE, DT, CL,
      SUM(CASE WHEN CL < col__3 THEN 1 ELSE 0 END)
      OVER (ORDER BY DT ASC) + 1 AS NoRisingDays
    FROM (
      SELECT t3.*, LAG(CL) OVER (ORDER BY DT ASC) AS col__3
      FROM t3
    ) sub__4
  ) t4
  GROUP BY NoRisingDays
) t5;
```
### 🐞 分步调试，告别SQL调试噩梦
### 🔄 一次编写，全数据库兼容
### 🤖 AI辅助，降低使用门槛
### 💸 免费商用，无功能限制
---
## 🚀 快速开始
- 在线体验：https://sqlazy.com
- IDE下载（Natural SPL）：https://sqlazy.com/download
---
## 🖥️ WEB版与桌面IDE版本差异
| 功能 | Web版 (sqlazy.com) | 桌面IDE |
|------|---------------------|---------|
| 自然语言→SQL | ✅ 包含SQL生成全能力 | ✅ 包含SQL生成全能力 + 原生SPL代码生成 |
| AI整理/规划 | ✅（有次数限制） | ✅（用户自备LLM Key，无使用限制，支持自定义Prompt） |
| 分步调试 | ✅（数据量受限） | ✅（基于esProc引擎，支持大数据量） |
| 数据不出域 | ❌ | ✅（支持企业私域部署付费方案） |
| 界面与语法语言支持 | 仅支持英文界面与英文语法词汇 | 完整中英双语界面，支持中英文语法词汇 |
| 数据库方言支持 | 与核心编译器同源 | 与核心编译器同源 |
| 费用规则 | 完全免费 | 个人/团队联网免费使用，企业私域部署需商业付费 |
---
## 📚 示例库
所有示例均为真实业务场景实现，可直接点击链接在线体验：
- 排名与排序：https://github.com/tosssman/sqlazy_test/tree/main/examples/01-ranking-sorting
- 分组与汇总：https://github.com/tosssman/sqlazy_test/tree/main/examples/02-grouping-aggregation
- 行列转换：https://github.com/tosssman/sqlazy_test/tree/main/examples/03-row-column-transform
- 表间关联：https://github.com/tosssman/sqlazy_test/tree/main/examples/04-table-join-comparison
- 有序计算：https://github.com/tosssman/sqlazy_test/tree/main/examples/05-ordered-calculation
- 筛选与定位：https://github.com/tosssman/sqlazy_test/tree/main/examples/06-filter-location
- 字符串与日期：https://github.com/tosssman/sqlazy_test/tree/main/examples/07-string-datetime
- 数据补全与对齐：https://github.com/tosssman/sqlazy_test/tree/main/examples/08-data-completion-alignment
- 嵌套查询替代：https://github.com/tosssman/sqlazy_test/tree/main/examples/09-subquery-window-function
- 复杂报表查询：https://github.com/tosssman/sqlazy_test/tree/main/examples/10-complex-report-query
---
## 📖 文档与授权
### SQLazy 语法规范(Prompts)
- 语法总览：https://github.com/tosssman/sqlazy_test/blob/main/docs/zh-CN/识别功能（语法总览）.md
- 基础公共规则：https://github.com/tosssman/sqlazy_test/blob/main/docs/zh-CN/功能_公共.md
- 数据读取类：
  - 文件读取：https://github.com/tosssman/sqlazy_test/blob/main/docs/zh-CN/文件.md
  - SQL执行：https://github.com/tosssman/sqlazy_test/blob/main/docs/zh-CN/SQL.md
- 表结构操作类：
  - 常数表：https://github.com/tosssman/sqlazy_test/blob/main/docs/zh-CN/常数表.md
  - 序列表：https://github.com/tosssman/sqlazy_test/blob/main/docs/zh-CN/序列表.md
  - 导出表：https://github.com/tosssman/sqlazy_test/blob/main/docs/zh-CN/导出表.md
  - 取出列：https://github.com/tosssman/sqlazy_test/blob/main/docs/zh-CN/取出列.md
  - 主键设置：https://github.com/tosssman/sqlazy_test/blob/main/docs/zh-CN/主键.md
- 数据过滤与排序类：
  - 筛选过滤：https://github.com/tosssman/sqlazy_test/blob/main/docs/zh-CN/筛选.md
  - 排序：https://github.com/tosssman/sqlazy_test/blob/main/docs/zh-CN/排序.md
  - 排名：https://github.com/tosssman/sqlazy_test/blob/main/docs/zh-CN/排名.md
  - 去重：https://github.com/tosssman/sqlazy_test/blob/main/docs/zh-CN/去重.md
- 数据计算类：
  - 基础计算：https://github.com/tosssman/sqlazy_test/blob/main/docs/zh-CN/计算.md
  - 计算列：https://github.com/tosssman/sqlazy_test/blob/main/docs/zh-CN/计算列.md
  - 分段分组：https://github.com/tosssman/sqlazy_test/blob/main/docs/zh-CN/分段.md
  - 聚合汇总：https://github.com/tosssman/sqlazy_test/blob/main/docs/zh-CN/汇总.md
- 表关联与集合操作类：
  - 表拼接关联：https://github.com/tosssman/sqlazy_test/blob/main/docs/zh-CN/拼接.md
  - 匹配过滤：https://github.com/tosssman/sqlazy_test/blob/main/docs/zh-CN/匹配.md
  - 集合运算：https://github.com/tosssman/sqlazy_test/blob/main/docs/zh-CN/集合.md
  - 数据对齐：https://github.com/tosssman/sqlazy_test/blob/main/docs/zh-CN/对齐.md
  - 行扩展：https://github.com/tosssman/sqlazy_test/blob/main/docs/zh-CN/扩展.md
  - 行列旋转：https://github.com/tosssman/sqlazy_test/blob/main/docs/zh-CN/旋转.md
  - 列表生成：https://github.com/tosssman/sqlazy_test/blob/main/docs/zh-CN/列表.md

### 许可说明

本仓库不包含源码，SaaS及IDE功能为非开源产品，可在保持设备联网的前提下免费使用（含商用场景）；本仓库内的所有示例代码可自由修改、分发、商用，无额外限制。

---
## 🛣️ RoadMap
### ✅ 已实现
- 核心：分步自然语言转标准SQL、白盒可审计、分步调试
- 数据库支持：MySQL、PostgreSQL、Oracle
- 基础能力：排名排序、分组汇总、表间关联、筛选定位、有序计算
### 🔜 近期规划
- 新增数据库支持：Snowflake、BigQuery
- 新增能力：递归查询
- 新增AI能力：自动将任意自然语言需求拆解为SQLazy分步指令
### ⚠️ 短期暂不支持
- 极低版本数据库兼容（如MySQL 5.5以下）
---
## 🤝 社区与反馈
- Issue：https://github.com/tosssman/sqlazy_test/issues
- Discussion（讨论区）：https://c.esproc.com/
- 商业授权咨询：contact@scudata.com
---
## ℹ️ 底层技术
SQLazy 基于 esProc SPL 构建，SPL相关资料可参考：https://github.com/SPLWare/esProc
