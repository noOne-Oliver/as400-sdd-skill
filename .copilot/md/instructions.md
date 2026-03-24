---
name: as400-sdd
description: AS400 RPG 开发框架 - Spec-Driven Development with AI Assistance
version: 1.0.0
tags: [as400, rpg, ibm-i, development, sdd]
---

# AS400-SDD Framework

> 适用场景：AS400 RPG 开发，Fixed Format / Free Format 双格式支持

---

## 框架概述

这是一个轻量级的 AS400 开发框架，基于 Spec-Driven Development (SDD) 理念。

### 核心流程

```
需求 → 写 Spec → Copilot 生成代码 → Review → 合并上线
```

### 角色

| 角色 | 职责 |
|------|------|
| Team Leader | 编写 Spec（指定格式） |
| 开发者 | 按 Spec 生成代码 |
| Reviewer | 代码审查 |

---

## 快速开始

### Step 1: 理解需求

当用户描述一个 AS400 开发需求时：

1. 询问或确认代码格式：**Fixed Format** 还是 **Free Format**
2. 确认涉及的文件（PF/LF）和业务规则
3. 确认程序类型（读取/写入/批处理/报表）

### Step 2: 生成 Spec

使用以下模板生成 Spec 文档：

```markdown
# [功能名称] - Spec v1.0

## 1. 基本信息
| 字段 | 内容 |
|------|------|
| 负责人 | |
| 优先级 | 高/中/低 |
| 代码格式 | Fixed Format / Free Format / 混用 |

## 2. 背景与目标
（为什么要做这个功能）

## 3. 业务规则
### 3.1 主业务逻辑
1. （步骤1）
2. （步骤2）

### 3.2 数据校验规则
| 字段 | 校验类型 | 规则 |
|------|----------|------|
| | | |

### 3.3 权限规则
（谁能执行）

### 3.4 事务规则
- [ ] 需要 COMMIT/ROLLBACK

## 4. 数据库设计
### 涉及文件
| 文件名 | 类型 | 用途 | 关键字段 |
|--------|------|------|----------|
| | | | |

## 5. 程序设计
| 程序名 | 类型 | 说明 |
|--------|------|------|
| | | |

## 6. 代码格式
- [ ] Fixed Format
- [ ] Free Format
- [ ] 混用

## 7. Copilot 生成规则
（针对此功能的特殊规则）
```

### Step 3: 生成代码

根据 Spec 生成的代码格式，选择对应的 Prompt：

#### Free Format Prompt 模板

```markdown
## 任务
生成一个 RPGLE [读取/写入/批处理] 程序。

## 格式
必须使用 Free Format，包含 `**FREE` 声明。

## 输入参数
（参数定义）

## 业务规则
1. （规则1）
2. （规则2）

## 文件定义
```rpgle
// 文件定义源码路径或字段列表
```

## 要求
1. 使用 `**FREE` 声明
2. 使用 DCL-PRC / DCL-PI 定义过程接口
3. 使用 EXTNAME 引用文件
4. 包含完整的错误处理（INFSPC）
5. 字符比较使用 %TRIM

## 输出
直接输出完整代码。
```

#### Fixed Format Prompt 模板

```markdown
## 任务
生成一个 RPG [读取/写入/批处理] 程序。

## 格式
必须使用 Fixed Format，基于列的写法。

## 列定义
- 列 1-5: 顺序号（可选）
- 列 6: 续行符(*)
- 列 7-21: 关键字
- 列 22-25: Factor 1
- 列 26-35: 操作码
- 列 36-50: Factor 2
- 列 51-80: Result

## 文件定义
（文件名字段列表）

## 要求
1. 使用 C-spec 格式
2. 操作码在列 7-21
3. 代码在 1-80 列内
4. 超过 80 列用 * 续行
5. 包含完整的错误处理

## 输出
直接输出完整代码。
```

### Step 4: Code Review

使用以下清单进行 Review：

```markdown
## Review 清单

### 格式检查（根据 Spec 指定的格式）

**Free Format 检查：**
- [ ] 有 `**FREE` 或 `/free` 声明
- [ ] 关键字大写（IF/ELSE/ENDIF/DCL）
- [ ] 结构完整（IF 有 ENDIF）
- [ ] 缩进 2 空格
- [ ] 单引号字符串

**Fixed Format 检查：**
- [ ] C-spec 格式（列 7-21 操作码）
- [ ] 代码在 1-80 列内
- [ ] 续行符在列 6
- [ ] 注释在列 1

### 业务逻辑检查
- [ ] 符合 Spec 中的业务规则
- [ ] 字段引用与文件定义一致
- [ ] 错误处理完整

### Copilot 幻觉检查
- [ ] 所有引用的字段在文件中存在
- [ ] 没有"看起来合理但不存在"的字段

### 输出
发现的问题列表 + 修复建议。
```

---

## 代码格式规范

### Fixed Format（旧版 RPG）

```rpgle
     C     *ENTRY        PLIST
     C                   PARM                    P_ORDNO      10
     C     P_ORDNO       CHAIN     ORDHDR                      90
     C                   IF        %FOUND
     C                   MOVEL     'A'         W_STATX
     C                   ENDIF
     C                   RETURN
```

**特点：**
- 基于列位置（1-80 列）
- C-spec 描述逻辑
- 操作码列 7-21
- 续行符列 6

### Free Format（现代 RPGLE）

```rpgle
**FREE

DCL-PI MAIN;
  p_OrderNo CHAR(10);
END-PI;

chain p_OrderNo ORDHDR;
if %found;
  W_STATX = 'A';
endif;

return;
```

**特点：**
- 自由格式，无需列位置
- `**FREE` 声明
- END-xxx 结构
- 类似现代编程语言

---

## 常见错误预防

### 字段幻觉
**问题：** Copilot 生成不存在的字段引用

**预防：**
- 始终提供文件定义源码路径
- 明确列出禁止使用的字段

### 错误处理缺失
**问题：** 文件操作无错误判断

**预防：**
- 要求包含 INFSPC 或 MONITOR
- 要求 ELSE 分支

### 事务控制缺失
**问题：** 多文件操作无 COMMIT/ROLLBACK

**预防：**
- 在 Spec 中明确事务要求
- Review 时检查 COMMIT/ROLLBACK

---

## 输出格式

当完成开发任务时，按以下格式输出：

```markdown
## 完成

### Spec
（对应的 Spec 文档）

### 代码
（生成的代码）

### Review 结果
- [ ] 通过
- [ ] 有问题（列出问题）

### 下一步
（如果有）
```
