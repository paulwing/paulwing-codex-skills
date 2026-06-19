# paulwing-codex-skills

[English](README.md) | [中文](README.zh-CN.md)

这是 Paul Wing 维护的个人 Codex skills 仓库，不是 OpenAI 官方仓库。

这里沉淀的是我在真实软件开发中使用 Codex 的工作流。当前重点很明确：
在 Codex 进行非平凡代码改动之前，让它先理解项目现状，给出清晰、简洁、
可确认的改动方案，然后再开始实现。

## 当前 Skill

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

## 为什么做这个 Skill

AI 编程助手速度很快，但速度越快，小假设带来的代价也越明显。
这个 skill 给重要改动加了一道轻量规划门：

1. 先理解当前状态。
2. 确定改动边界。
3. 列出受影响的文件和行为。
4. 等用户确认范围。
5. 再实现并验证。

目标不是生成冗长方案，而是让 Codex 在动手之前给出一份用户能快速判断的改动清单。

## 计划输出风格示例

对于功能或行为改动，Codex 应该给出类似这样的方案：

```text
功能逻辑
- 增加按产品型号和在线状态筛选设备树。
- 保持普通业务查询默认只处理 status = 1 的有效数据。

后端改动
- internal/app/device/types.go：增加请求字段和响应结构。
- internal/app/device/repository.go：扩展查询条件。
- internal/app/device/service.go：校验筛选参数并保留现有默认行为。

前端改动
- src/pages/device/DeviceTreePage.tsx：增加筛选控件。
- src/api/device.ts：让请求参数与后端契约保持一致。

数据库/初始化脚本改动
- 不需要修改表结构。

验证方式
- 运行设备查询相关的后端聚焦测试。
- 运行前端 typecheck/build，覆盖被改页面。
```

这就是这个 skill 想要稳定保留的核心行为：改动前先给出直接、聚焦、可确认的方案。

## 仓库结构

```text
skills/
`-- change-planning/
    `-- SKILL.md
```

## 在 Codex 中安装

使用 Codex 自带的 skill installer 从 GitHub 安装：

```bash
python3 ~/.codex/skills/.system/skill-installer/scripts/install-skill-from-github.py --repo paulwing/paulwing-codex-skills --path skills/change-planning
```

安装后重启 Codex，让它重新发现 skill。

## 更新已安装版本

如果已经安装过，想更新到最新版本：

```bash
rm -rf ~/.codex/skills/change-planning
python3 ~/.codex/skills/.system/skill-installer/scripts/install-skill-from-github.py --repo paulwing/paulwing-codex-skills --path skills/change-planning
```

更新后重启 Codex。

## 兼容性

这个仓库优先面向 OpenAI Codex。skill 本身采用标准的 `SKILL.md` 目录格式，
因此也可能迁移到其他支持 Agent Skills 的工具中使用，但 Codex 是主要目标。
