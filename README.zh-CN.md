# paulwing-codex-skills

[English](README.md) | [中文](README.zh-CN.md)

这是 Paul Wing 维护的个人 Codex skills 仓库，不是 OpenAI 官方仓库。

这里沉淀的是我在真实软件开发中使用 Codex 的工作流。当前重点很明确：
让 Codex 在非平凡改动前保持克制和计划性，同时让 AI 写出来的代码足够清晰，
能被真实维护者长期接手。

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

## 仓库结构

```text
skills/
|-- change-planning/
|   |-- agents/
|   |   `-- openai.yaml
|   `-- SKILL.md
`-- readable-development/
    |-- agents/
    |   `-- openai.yaml
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

更新后重启 Codex。

## 兼容性

这个仓库优先面向 OpenAI Codex。skill 本身采用标准的 `SKILL.md` 目录格式，
因此也可能迁移到其他支持 Agent Skills 的工具中使用，但 Codex 是主要目标。
