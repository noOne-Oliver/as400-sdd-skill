# AS400-SDD Framework - OpenCode CLI Skill

> 适用场景：OpenCode CLI 环境执行 AS400 RPG 开发任务

---

## 框架概述

这是一个 AS400 开发框架，基于 Spec-Driven Development (SDD) 理念。

## 使用方法

### 启动 Skill

在 OpenCode CLI 中加载此 skill：

```
/skill load as400-sdd
```

或直接引用：

```
@as400-sdd-skill
```

### 核心命令

#### 1. 新建开发任务

```bash
as400-sdd new --requirement "描述需求" --format [fixed|free|mixed]
```

#### 2. 生成 Spec

```bash
as400-sdd spec --generate --template standard
```

#### 3. 生成代码

```bash
as400-sdd generate --spec <spec-file> --type [read|write|batch|report]
```

#### 4. 代码 Review

```bash
as400-sdd review --code <code-file> --spec <spec-file>
```

---

## Skill 配置

```yaml
name: as400-sdd
version: 1.0.0
description: AS400 RPG Development Framework
environment:
  file_format: [fixed, free, mixed]
  required_fields:
    - business_rules
    - database_design
    - code_format
prompts:
  spec: prompts/spec-generation.md
  code: prompts/code-generation.md
  review: prompts/code-review.md
```

---

## Prompt 模板

### Spec 生成 Prompt

当用户要求生成 Spec 时，使用以下模板：

```
## 任务
根据以下需求，生成规范的 Spec 文档。

## 需求
{用户描述的需求}

## 输出格式
按照 AS400-SDD Spec 模板输出，包含：
1. 基本信息
2. 背景与目标
3. 业务规则（主逻辑、校验、权限、事务）
4. 数据库设计（PF/LF 文件）
5. 程序设计
6. 代码格式选择
7. Copilot 生成规则

## 要求
- 代码格式必须指定：Fixed / Free / 混用
- 业务规则必须完整
- 文件定义必须提供字段列表
```

### 代码生成 Prompt

当用户要求生成代码时：

```
## 任务
根据以下 Spec，生成 AS400 RPG 代码。

## Spec
{完整 Spec 内容}

## 代码格式要求
{Spec 中指定的格式}

## 输出要求
1. 完整可编译的代码
2. 包含完整错误处理
3. 遵循指定格式（Fixed/Free）
4. 所有字段引用与 Spec 一致
```

### Code Review Prompt

当用户要求 Review 代码时：

```
## 任务
Review 以下 AS400 RPG 代码。

## 代码格式
{Fixed / Free / 混用}

## Spec 要点
{关键业务规则}

## 检查项

### 格式检查
- Fixed Format: C-spec、列位置、续行符
- Free Format: **FREE 声明、关键字大写、结构完整

### 正确性检查
- 字段引用与 Spec 一致
- 业务逻辑与 Spec 一致
- 错误处理完整

### Copilot 幻觉检查
- 所有字段在文件中存在
- 无"看起来合理但不存在"的代码

## 输出
问题列表 + 修复建议
```

---

## 代码格式规范

### Fixed Format

```
列定义：
- 列 1-5: 顺序号
- 列 6: 续行符(*)
- 列 7-21: 关键字
- 列 22-25: Factor 1
- 列 26-35: 操作码
- 列 36-50: Factor 2
- 列 51-80: Result
```

### Free Format

```
要求：
- **FREE 或 /free 声明
- 关键字大写
- END-xxx 结构完整
- 缩进 2 空格
- 单引号字符串
```

---

## 常见错误

| 错误类型 | 描述 | 预防 |
|----------|------|------|
| 字段幻觉 | 生成不存在的字段 | 提供文件定义 |
| 格式错误 | Fixed/Free 混用 | Spec 指定格式 |
| 处理缺失 | 无错误处理 | 要求 INFSPC |
| 事务遗漏 | 无 COMMIT | Spec 明确事务 |

---

## 输出格式

```markdown
## AS400-SDD 执行结果

### 任务类型
{new|spec|generate|review}

### 状态
- [ ] 成功
- [ ] 有问题

### 产物
（生成的 Spec/代码/Review 结果）

### 下一步
（建议的后续操作）
```
