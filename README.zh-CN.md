# paulwing-codex-skills

[English](README.md) | [中文](README.zh-CN.md)

这是 Paul Wing 维护的个人 Codex skills 仓库，不是 OpenAI 官方仓库。

这里沉淀的是我在真实软件开发和技术问题调查中使用 Codex 的工作流。
当前重点很明确：让 Codex 在非平凡改动前保持克制和计划性，让 AI 写出来的
代码足够清晰、能被真实维护者长期接手，同时把可复用的调查流程沉淀成清晰、
有证据边界的输出。

## 当前 Skills

### `change-planning`

`change-planning` 是一个“先规划，再实现”的 Codex 工作流。

当需求可能涉及多个文件、接口契约、业务模块、数据库表、权限规则或前端流程时，
这个 skill 会引导 Codex：

- 先阅读和理解当前代码，而不是直接动手改
- 识别后端、前端、数据库、文档和验证方式的影响范围
- 先给出简洁的实现方案，等待用户确认
- 未经确认不编辑代码，除非用户已经明确要求按既定方案实现
- 实现后对改动范围做聚焦验证，再汇报结果

它适合日常工程开发：不用写很长的设计文档，但要避免 AI 因为动作太快而改偏、
改散，或者顺手做了不该做的重构。

### `readable-development`

`readable-development` 是一个“可读性优先”的 Codex 工作流。

当用户希望代码更易维护、更容易学习、认知负担更低，或者明确担心 AI 生成代码
过度工程化时，这个 skill 会引导 Codex：

- 优先考虑未来维护者
- 先写直接清晰的实现，再考虑抽象
- 只有在有明确证据时才引入抽象
- 改完后解释代码调用路径
- 根据改动风险匹配验证强度

它适合希望借助 AI 提升开发效率，但又不希望代码库变得比问题本身更难理解的场景。

### `tron-usdt-flow-tracing`

`tron-usdt-flow-tracing` 是一个面向 TRON 链 USDT/TRC20 资金流向追踪的公开链上调查工作流。

当用户提供 TRON 交易哈希、Tronscan 链接或地址，并希望了解 USDT 后续流向时，
这个 skill 会引导 Codex：

- 核验初始交易和代币信息
- 基于 Tronscan 数据继续追踪下游 TRC20 转账
- 保留交易哈希、时间、金额、地址和公开标签
- 区分直接转账、混合账户后的高度相关下游、交易所/桥/服务端点
- 输出证据表、Mermaid 流向图，以及报警或联系交易所风控的行动建议

它适合诈骗损失初筛、合规式证据整理，以及可复用的链上调查报告。它不会承诺追回资金，
不会索要钱包私钥或助记词，也不会把公开链上数据无法证明的身份信息说成确定事实。

## 为什么做这些 Skills

AI 编程助手速度很快，但速度越快，小假设带来的代价也越明显。
这个仓库给重要改动加上轻量的工作流约束：

1. 先理解当前状态。
2. 确定改动边界。
3. 列出受影响的文件和行为。
4. 等用户确认范围。
5. 以维护者能看懂为目标实现。
6. 验证并解释最终代码路径。

目标不是生成冗长方案，而是让 Codex 在动手之前给出用户能快速判断的改动清单，
并在实现时保持简单、清晰、可维护。

## 调用建议

如果你希望 Codex 的行为更稳定、更可控，建议在关心具体工作流时显式调用 skill。
在 Codex CLI 或 IDE 中，可以通过 `/skills` 选择 skill，也可以在 prompt 里直接写
`$change-planning`、`$readable-development` 或 `$tron-usdt-flow-tracing`。

推荐用法：

- `change-planning` 可以保留隐式触发，因为它更像非平凡代码改动前的安全门。
- 当主要诉求是可读性、可维护性或避免过度工程化时，建议显式调用
  `readable-development`。
- 两个都需要时，可以写：`$change-planning $readable-development`，先规划，
  确认后再按可读性优先的风格实现。
- 做 TRON USDT 资金流向追踪时，建议显式调用 `$tron-usdt-flow-tracing`，
  这样可以稳定保留谨慎措辞和证据边界。
- 如果只希望使用一个工作流，直接在 prompt 中说明，例如：
  `这次只使用 $change-planning。`

`readable-development` 已配置为只允许显式调用：

```yaml
policy:
  allow_implicit_invocation: false
```

这样普通代码请求不会自动套用可读性工作流，只有用户通过 `/skills` 或
`$readable-development` 明确指定时才会使用。

`tron-usdt-flow-tracing` 也建议显式调用，避免普通加密货币问题被误当作诈骗调查处理。

## 输出风格示例

### 改动前计划

对于功能或行为改动，`change-planning` 应该引导 Codex 给出类似这样的方案：

```text
功能逻辑
- 增加按产品型号和在线状态筛选设备树。
- 保持普通业务查询默认只处理 status = 1 的有效数据。

后端改动
- internal/app/device/types.go（改）：增加请求字段和响应结构。
- internal/app/device/repository.go（改）：扩展查询条件。
- internal/app/device/service.go（改）：校验筛选参数并保留现有默认行为。

前端改动
- src/pages/device/DeviceTreePage.tsx（改）：增加筛选入口和列表联动。
- src/components/device/DeviceTreeFilters.tsx（增）：页面已拥挤时拆出筛选控件。
- src/api/device.ts（改）：让请求参数与后端契约保持一致。

数据库/初始化脚本改动
- 不需要修改表结构。

验证方式
- 运行设备查询相关的后端聚焦测试。
- 运行前端 typecheck/build，覆盖被改页面。
```

这就是 `change-planning` 想要稳定保留的核心行为：改动前先给出直接、聚焦、可确认的方案。

### 可读性改动汇报

对于已经完成的非平凡代码改动，`readable-development` 应该引导 Codex 用面向维护者的方式说明代码路径：

```text
已调整设备树筛选路径，让查询逻辑保持直接、局部、可读。

请求入口在 src/pages/device/DeviceTreePage.tsx，筛选参数通过 src/api/device.ts
传给后端。后端参数校验在 internal/app/device/service.go，最终的状态和型号
查询条件在 internal/app/device/repository.go 中构建。

这次没有新增抽象，因为行为仍然只服务于当前页面和查询。验证：已运行设备相关
后端聚焦测试和前端 typecheck。
```

这就是 `readable-development` 想要稳定保留的核心行为：代码实现保持简单，同时让下一个维护者能快速找到关键路径。

### TRON USDT 资金流向报告

对于 TRON USDT 追踪请求，`tron-usdt-flow-tracing` 应该引导 Codex 输出简洁、
证据优先的报告：

```text
摘要
初始 TRON USDT 转账已确认。资金从 Binance-Hot 11 转入用户报告的收款地址，
随后经过活跃中转地址。当前最有操作价值的终点是 OKX Hot Wallet 8。

关键流向
| 步骤 | 时间 | 金额 | 转出 | 转入 | 交易哈希 | 证据强度 |
|---|---:|---:|---|---|---|---|

Mermaid 图
flowchart LR
    A["Binance-Hot 11"] --> B["用户报告收款地址"]
    B --> C["中转地址"]
    C -.-> D["混合后的下游流向"]
    D --> E["OKX Hot Wallet 8"]

建议动作
保留交易所提现记录，报警时提交上述哈希和地址，并联系交易所风控/合规渠道。
不要提供助记词、私钥、验证码，也不要相信付费追回资金的人。
```

这就是 `tron-usdt-flow-tracing` 想要稳定保留的核心行为：提供可操作的资金流向证据，
同时不夸大公开链上数据能证明的内容。

## 仓库结构

```text
skills/
|-- change-planning/
|   |-- agents/
|   |   `-- openai.yaml
|   `-- SKILL.md
|-- readable-development/
|   |-- agents/
|   |   `-- openai.yaml
|   `-- SKILL.md
`-- tron-usdt-flow-tracing/
    |-- agents/
    |   `-- openai.yaml
    |-- assets/
    |   `-- mermaid-template.mmd
    |-- references/
    |   |-- evidence-standards.md
    |   |-- report-template.md
    |   `-- tronscan-workflow.md
    `-- SKILL.md
```

## 在 Codex 中安装

使用 Codex 自带的 skill installer 从 GitHub 安装：

```bash
python3 ~/.codex/skills/.system/skill-installer/scripts/install-skill-from-github.py --repo paulwing/paulwing-codex-skills --path skills/change-planning
```

```bash
python3 ~/.codex/skills/.system/skill-installer/scripts/install-skill-from-github.py --repo paulwing/paulwing-codex-skills --path skills/readable-development
```

```bash
python3 ~/.codex/skills/.system/skill-installer/scripts/install-skill-from-github.py --repo paulwing/paulwing-codex-skills --path skills/tron-usdt-flow-tracing
```

安装后重启 Codex，让它重新发现 skill。

## 更新已安装版本

如果已经安装过某个 skill，想更新到最新版本：

```bash
rm -rf ~/.codex/skills/change-planning
python3 ~/.codex/skills/.system/skill-installer/scripts/install-skill-from-github.py --repo paulwing/paulwing-codex-skills --path skills/change-planning
```

```bash
rm -rf ~/.codex/skills/readable-development
python3 ~/.codex/skills/.system/skill-installer/scripts/install-skill-from-github.py --repo paulwing/paulwing-codex-skills --path skills/readable-development
```

```bash
rm -rf ~/.codex/skills/tron-usdt-flow-tracing
python3 ~/.codex/skills/.system/skill-installer/scripts/install-skill-from-github.py --repo paulwing/paulwing-codex-skills --path skills/tron-usdt-flow-tracing
```

更新后重启 Codex。

## 兼容性

这个仓库优先面向 OpenAI Codex。skill 本身采用标准的 `SKILL.md` 目录格式，
因此也可能迁移到其他支持 Agent Skills 的工具中使用，但 Codex 是主要目标。
